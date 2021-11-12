---
layout: page
title: Research
---
# <center> SMAC: A Simultaneous Mapping and Completion System for RGB-D Sensors
## Abstract
We proposes a versatile simultaneous mapping and completion system for indoor scenes, which aims to provide semantic, lightweight and complete 3D dense models for virtual reality applications. In order to reconstruct complete environments, on the one hand, we provide an image synthesis strategy aimed at restoring 2D pixels occluded by objects. On the other hand, the depth information of these occluded parts is calculated based on a ray casting approach. After restoring the 3D occluded areas, we take advantage of a resolution adaptive TSDF algorithm to build lightweight 3D models according to the texture information of scenarios.

![overview](./example.png)

The overview of the whole SAMC systems is shown as below:

![refineoverview](./refineoverview.png)

The system is tested in multi living room SLAM datasets, including the ICL and Scannet, TUM RGB-D datasets, the following videos are the experimental results.

<iframe width="720" height="540" src="../video/demoICLlr0.mp4" frameborder="0" allowfullscreen></iframe>



In this video, you can see that these blue plane points are the results of sparse map completion.  For a pair of RGB-D images, we firstly extract the geometric-interested (planar and non-planar) and semantic-interested (objects) regions that are used to analyze the occlusion situations. Commonly, those obscured areas are distributed on walls and floor. After detecting occlusion masks, we have known the non-planar points in the map, then we use the ray-casting method to finish the sparse-map completion, we compute the linear equation between the camera center and the non-planar points, we use multi-geometric constraints to calculate the most appropriate cross point, then we can get the sparse-map completion. 

However, the occlusion phenomenon that appears in a certain view is possibly observed in new views. Therefore, it is important to update the predicted boundary lines and occluded planes created by each key-frame. After a new key-frame being selected, we project the completion information to the current frame, and the points that fall outside the non-planar area will be deleted. Furthermore, the endpoints of boundary lines are updated when related planes' regions grow up.

What's more, it's not enough to get only the sparse-map completion, because sparse map cannot be applied directly in the robot devices, so we finish the dense-map reconstruction at the same time. We take advantage of the occlusion masks to separate them into different parts according to the cross-line between each two planes. In this way each occlusion part will not be affected by other regions, and the whole synthesis process can be accelerated using parallel computing method, which is more efficient than traditional method.

We use the laplacian inpaint [1] as the method of our RGB image synthesis, we raise a new method about the depth synthesis, first we detect the occlusion areas in current image and judge which plane this area belongs to, secondly we project the occlusion area into the world position, by using the planar equation, we can calculate the depth of the occlusion area, therefore the depth synthesis can be accomplished.

Here are the RGB and Depth image without synthesis

<center class="half">
    <img src="../rgb.png" width="300"/>
    <img src="../depth.png" width="300"/>
</center>


Here are the RGB and Depth image after inpainted.

<center class="half">
    <img src="../rgb_inpaint.png" width="300"/>
    <img src="../depth_inpaint.png" width="300"/>
</center>


Then we can use these images to finish the dense reconstruction using the TSDF.

![dense reconstruction](./denserecon.png)

## source code

This work is under the instruction of the Dr. Yanyan Li, Ph.D student in Computer Aided Medical Procedures & Augmented Reality, Technical University of Munich. There is still some work need to be improved such as data association, the code will open-source when finish all the functions in this SMAC system.

## reference
[1] J. H. Lee, I. Choi and M. H. Kim, "Laplacian Patch-Based Image Synthesis," 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2016, pp. 2727-2735, doi: 10.1109/CVPR.2016.298. <br />
