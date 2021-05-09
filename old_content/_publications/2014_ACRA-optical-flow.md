---
layout: post
title:  "A Filtering Approach for Computation of Real-Time Dense Optical-flow for Robotic Applications"
journal: "ACRA 2014"
date:   2014-12-03 00:00:00 +1100
categories: publications conference
---

### Abstract

The present article presents an iterative filter approach for the  computation  of  optical-flow. The filter  is  based  on  an  update  and  propagation loop. The propagation stage takes the currently  computed flow  to  predict  the  value at  the  next  time  iteration.   The  update  stage takes this prediction, together with the stream of  images,  and  corrects  the  optical-flow  field. This leads to an incremental approach to build optical-flow.   Regions  of  the  image  where  no flow computation is possible are filled in by a diffusion term in the propagation step.  Ground truth validation of the algorithm is provided by simulating a high-speed camera in a 3D scene. The local computations and convolution based implementation is well suited for real-time systems with high-speed and high-acuity cameras.

[PDF](http://www.araa.asn.au/acra/acra2014/papers/pap130.pdf)

```
@InProceedings{2014_Adarve,
  Title                    = {A Filtering Approach for the Computation of Real-Time Dense Optical-flow for Robotic Applications},
  Author                   = {Adarve, Juan David and Austin, David and Mahony, Robert},
  Booktitle                = {Proc. Australasian Conf. Robot. Autom.},
  Year                     = {2014},
  Owner                    = {jadarve},
  Timestamp                = {2013.05.10}
}
```