---
layout: post
title:  "Spherepix: a Data Structure for Spherical Image Processing"
journal: "IEEE Robotics and Automation Letters"
date:   2017-04-01 00:00:00 +1100
categories: publications journal
thumbnail:  /assets/publications/2016_Adarve_RAL_spherepix.jpg
---

### Abstract

This letter introduces the ``spherepix'' data structure for efficient implementation of low-level image processing operations on spherical images.
Efficient implementation of low-level image processing depends heavily on separability of the convolution kernels that form the fundamental building blocks of most algorithms.
Due to the curvature of the sphere, it is not possible to place an orthogonal grid pixelation globally on its surface, making direct application of classical separable kernel convolutions impossible.
In the spherepix data structure we propose an alternative approach consisting of a collection of overlapping (near orthogonal) grid patches covering the sphere's surface.
Close to the boundaries of patches we introduce data interpolation between patch grids to ensure information flow between grid patches.
After each image processing subroutine, we reconcile data in the overlapping regions to homogenize the global data representation.
We claim that the additional computational cost of data interpolation and data reconciliation is easily compensated by the computational saving and algorithmic simplicity of applying existing image processing subroutines in the grid patches.
The approach is demonstrated by implementing a SIFT feature point algorithm in spherepix coordinates and comparing precision, recall, and computational cost of the proposed approach to documented modifications of the SIFT algorithm specifically developed for implementation on spherical images.

[DOI: LRA.2016.2645119](https://doi.org/10.1109/LRA.2016.2645119)

[PDF from ResearchGate](https://www.researchgate.net/publication/311893485_Spherepix_a_Data_Structure_for_Spherical_Image_Processing)

```
@ARTICLE{2016_Adarve_spherepix, 
    author={J. D. Adarve and R. Mahony}, 
    journal={IEEE Robotics and Automation Letters}, 
    title={Spherepix: A Data Structure for Spherical Image Processing}, 
    year={2017}, 
    volume={2}, 
    number={2}, 
    pages={483-490}, 
    doi={10.1109/LRA.2016.2645119}, 
    ISSN={2377-3766}, 
    month={April},}
```