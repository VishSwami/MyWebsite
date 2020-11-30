---
layout: post
title: Video Stabilization with Robust L1 Optimal Camera Paths 
tags: 
excerpt_separator: <!--more-->
---

Video stabilization is a commonly known process for shake correction in hand-taken video. For my final
project for Computational Photography, I implemented the algorithm for stabilization post-processing
discussed in [this paper](https://www.cc.gatech.edu/cpl/projects/videostabilization/stabilization.pdf).

<!--more-->

The first step in this process is to establish a means of extracting a model of the camera's motion.
For this paper, the movement we are modeling is limited to two dimensions. Using the OpenCV library,
we can use the feature-tracking tools it provides to determine the image warps that most closely
align adjacent frames in the video. These image warps can then be broken down into translational
and affine components. By tracking the translation component of the warp between each frame pair, we can create
a linear motion model of the camera through the entire video.

Our next goal is to then determine the frame warps that would transform this video into a smoother camera path.
Since our overall goal is to create an output video that may have been shot with proper stabilization equipment,
we want our camera path to be a sequence of constant, linear, and parabolic motion (no movement, 
panning, and ease-in and out respectively). However, we also want our stabilized video to most accurately represent
the intended movement of the original. As such, we need to apply some constraints to how far the ideal path
can stray from the original, and that the output does not warp the image out of frame. Due to stabilization needing
to rotate the image, some fraction of the source video must be cropped as to avoid the stabilized output
from leaving the frame. Given our goal and limitations, we can model this problem as a linear program, 
as seen in algorithm 1 in the discussed paper.

After determining the appropriate warps from the original to optimal camera path, we need to actually warp
the source video. Once again, applying OpenCV, we can find the manner in which the warps for each frame shift the 
cropped corners of the image. Once we know how the corners move, we can then apply the appropriate warp that will
move the warped positions of the cropped corners in to the corners of the frame, resulting in the stabilized output.