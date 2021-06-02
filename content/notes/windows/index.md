---
title: Windows Notes
menu:
  notes:
    name: Windows
    identifier: notes-windows
    weight: 10
---


{{< note title="Environment variables" >}}
```powershell
# get all variables
Get-ChildItem Env:

# get one (Path)
Get-ChildItem Env:Path

# set a variable
$Env:VULKAN_SDK = "C:/VulkanSDK/1.2.176.1"

# append to Path
$Env:Path += ";C:/Python27"
```
{{< /note >}}
