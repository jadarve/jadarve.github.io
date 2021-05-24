---
title: Introducing rules_vulkan for Bazel
date: 2021-05-23
menu:
  sidebar:
    name: Introducing rules_vulkan
    identifier: introducing-rules-vulkan
    weight: 0
---

[Bazel](https://bazel.build/) is a good multi-platform, multi-language build system designed for correctness and scalability. It supports programming languages such as Go, C++, Java, and Python, through the use of rule systems for each of them. Rules are stored in repositories (git, http archives) and are downloaded on-demand when they are referenced.

I use Bazel as the build system for my [lluvia project](https://lluvia.ai). I use C++ as the core language of the project, Python for creating easy to use wrappers for prototyping, Lua for describing compute stages, and finally, GLSL for coding compute shaders that run on the GPU.

For GLSL and Vulkan, I used some custom rules coded within the project for defining shaders and shader libraries. As those rules grew, I saw the opportunity to maintain them as an independent project that can serve anyone trying to use GLSL and Vulkan in their project.

## rules_vulkan

The [`rules_vulkan`](https://github.com/jadarve/rules_vulkan) project provides rules for accessing the [Vulkan SDK](https://vulkan.lunarg.com/sdk/home) installed on the host system. By exposing the SDK to Bazel, one gets access to:

* The vulkan headers and libraries for creating C++ binaries.
* Shader tools such as `glslc` for compiling shaders into SPIR-V representation, among others.

## Configuration

Include the following configuration in your project's `WORKSPACE` file.

```python
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "rules_vulkan",
    remote = "https://github.com/jadarve/rules_vulkan.git",
    tag = "v0.0.1"
)

load("@rules_vulkan//vulkan:repositories.bzl", "vulkan_repositories")
vulkan_repositories()
```

By calling `vulkan_repositories()`, the package will look for the Vulkan SDK installed in your Operating System and create the following extra repositories:

| Repository       | Defined Targets                                                                                                                                              |
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `vulkan_windows` | `vulkan_cc_library` for compiling C/C++ targets that depend on Vulkan. `glslc` filegroup for the GLSL compiler, used internally to compile shaders.          |
| `vulkan_linux`   | Nothing at the moment. The GLSL compiler is accessed directly from the system.                                                                               |

## Rules

### `glsl_header_library`

A GLSL header library defines a collection of `.glsl` files that can be included during compilation of a `glsl_shader`:

```python
load("@rules_vulkan//glsl:defs.bzl", "glsl_header_library")

glsl_header_library(
    name = "mylib_glsl_library",
    hdrs = [
        "mylib/mylib.glsl",
        "mylib/color/color.glsl",
    ],
    # path from the repository's root that will be stripped
    strip_include_prefix = "shader_lib",
    visibility = ["//visibility:public"]
)
```

### `glsl_shader`

A `glsl_shader` compiles a shader file (`.vert`, `.frag`, `.tesc`, `.tese`, `.geom`, `.comp`) to SPIR-V representation:

```python
load("@rules_vulkan//glsl:defs.bzl", "glsl_shader")

glsl_shader(
    name = "assign_shader",
    shader = "assign.comp",
    deps = [
        "//shader_lib:mylib_glsl_library"
    ],
    visibility = ["//visibility:public"]
)
```

## Referencing vulkan libraries in CC targets

As mentioned above `rules_vulkan` also creates extra repositories to access the Vulkan SDK installed on the host operating system: `vulkan_windows`, and `vulkan_linux`. For Windows in particular, it currently defines the `vulkan_cc_library` to access the vulkan headers and libraries. Below is an [example from the repo](https://github.com/jadarve/rules_vulkan/tree/main/examples) to compile a simple binary that lists the available extensions:

```python
load("@rules_cc//cc:defs.bzl", "cc_binary")

config_setting (
    name = "linux",
    constraint_values = [
        "@platforms//os:linux"
    ],
    visibility = ["//visibility:public"]
)

config_setting (
    name = "windows",
    constraint_values = [
        "@platforms//os:windows"
    ],
    visibility = ["//visibility:public"]
)

cc_binary(
    name = "list_extensions",
    srcs = [
        "list_extensions.cpp"
    ],
    linkopts = select({
        "//:linux": [
            "-lvulkan",
        ],
        "//:windows": [],
    }),
    deps = select({
        "//:windows": [
            "@vulkan_windows//:vulkan_cc_library",
        ],
        "//conditions:default": [],
    }),
    visibility = ["//visibility:public"]
)
```

The code for `list_extensions.cpp` is:

```cpp

#include <cstdlib>
#include <iostream>

#include <vulkan/vulkan.hpp>

int main() {

    const vk::ApplicationInfo appInfo = vk::ApplicationInfo()
            .setPApplicationName("list_extensions")
            .setApplicationVersion(0)
            .setEngineVersion(0)
            .setPEngineName("rules_vulkan");

    const vk::InstanceCreateInfo instanceInfo = vk::InstanceCreateInfo()
            .setPApplicationInfo(&appInfo);

    vk::Instance instance;
    vk::Result result = vk::createInstance(&instanceInfo, nullptr, &instance);
    if (result != vk::Result::eSuccess) {
        std::cerr << "error creating vulkan instance" << std::endl;
        exit(-1);
    }

    auto extensions = vk::enumerateInstanceExtensionProperties();
    for (const auto& ext: extensions) {
        std::cout << ext.extensionName << std::endl; 
    }

    return EXIT_SUCCESS;
}
```

## Future work

In the example above, notice that for linux, the dependency on Vulkan is accessed directly from the OS. In particular for Ubuntu, the LunarG SDK can be installed using the regular `apt install` method and hence, the headers, libraries, and tools (glslc), are installed distributed across different folders in the system. This makes it hard for creating a `vulkan_linux` repository as I need to access several locations from the system. That still does not work.

Also, there is no support for MacOS at the moment.
