---
layout: post
title:  "A Filter Formulation for Computing Real Time Optical Flow"
journal: "IEEE Robotics and Automation Letters"
date:   2016-07-01 00:00:00 +1100
categories: publications journal
thumbnail:  /assets/publications/2016_Adarve_RAL_oflow.jpg
---

### Abstract

Dense optical flow is a crucial visual cue for obstacle avoidance and motion control for robotic systems functioning in complex unstructured environments. In order to compute image motion for modern highly dynamic robots, it is necessary to use high speed vision systems. To perceive small and thin objects such as tree branches, fence poles, and other similar objects, high resolution vision systems must be used. The data stream from a high-resolution, high-speed vision system will saturate the onboard data bus and overload the onboard processor of almost all existing robotic systems. To implement such a vision system, we believe it is necessary to process the data *in situ* at the camera using dedicated processing hardware, a vision processing paradigm that we term **Rapid Embedded Vision** (REV) systems. Even then, the task of processing multiple hundred hertz of dense high-resolution images is not possible using classical algorithms and classical computing hardware. A new processing paradigm that actively exploits the high frame rate of the image sequences, and high performance computing hardware such as GPU or FPGA must be employed. This paper proposes a filtering algorithm for the computation of dense optical flow fields in real time. The filter is designed as a pyramidal structure of update and propagation loops, where an optical flow state is constantly refined with new image data from the camera. The computational properties of the proposed algorithm makes it well suited for implementation in GPU and FPGA systems. We present results from a GPU implementation achieving frame rates in the order of 800 Hz at VGA resolution. Experimental validation and comparison is provided for both synthetic and real life high speed video sequences at frame rates of **300 Hz** and **1016x544** pixel resolution.

[DOI: LRA.2016.2532928](https://doi.org/10.1109/LRA.2016.2532928)

[PDF from ResearchGate](https://www.researchgate.net/publication/295833376_A_Filter_Formulation_for_Computing_Real_Time_Optical_Flow)

```
@ARTICLE{2016_Adarve_optical_flow, 
    author={J. D. Adarve and R. Mahony}, 
    journal={IEEE Robotics and Automation Letters}, 
    title={A Filter Formulation for Computing Real Time Optical Flow}, 
    year={2016}, 
    volume={1}, 
    number={2}, 
    pages={1192-1199},
    doi={10.1109/LRA.2016.2532928}, 
    ISSN={2377-3766}, 
    month={July},
}
```