---
layout: post
title: Content-Aware Image Resizing
tags: 
excerpt_separator: <!--more-->
---

Due to the increase in devices with digital displays over the last few decades, 
various different screen sizes/shapes need to be accounted for to make image content 
on web-pages and other media adaptable across these varying display shapes.
The most primitive solution to this issue is to simply shrink or stretch images accordingly.
However, this inevitably causes scenarios where images are stretched or shrunk to an extent 
that causes the original image to be completely distorted.

<!--more-->

For my midterm project in Computational Photography, I implemented the algorithm for content-aware image resizing discussed in
[these](https://faculty.idc.ac.il/arik/SCWeb/imret/imret.pdf) [papers](https://faculty.idc.ac.il/arik/SCWeb/vidret/vidret.pdf). The principle behind this process is to detect and remove 
paths of least-energy in a given image. Energy, in this context, is the magnitude with which a pixel differs with its neighboring
region. Pixels with high-energy usually contain the most salient elements of an image,
and as such should not be expanded or shrunken so as to maintain the integrity of those salient elements.

To find these least-energy path, we can define the cost of a path as the accumulation of the pixel the path starts on,
and the lowest-energy pixels on the path leading to that pixel. (Let's assume that we're dealing with vertical paths only)
Since we can define this expression recursively, we can apply a simple dynamic programming solution. Starting from the top row
of the image, the path cost of each pixel is just the energy of that pixel itself. From every row after, the path cost can be defined
as the energy of that pixel, plus the lowest path-cost of the 3 adjacent pixels above it. We can then choose the least cost
path as the lowest accumulated value of the bottom row of the image.

After we've defined this cost-matrix, removal is fairly straight-forward. Simply choose and remove the lowest cost of the bottom row,
working your way up from the adjacent pixels and picking the lowest of those adjacent pixels. If we want to remove more than a single
seam, which is most often the case, we need to recompute the cost-matrix, due to the new adjacencies introduced by the first removal.

Insertion, however, introduces another degree of complexity. If we were to simply compute the lowest-cost path and insert a duplicate
seam, we would repeatedly pick the same seam, thereby not usefully expanding the image. As such, we need to apply
the removal process for the n seams, while recording the pixel positions of those paths 
and then duplicate each of those seams in the original image.