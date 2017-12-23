# **Finding Lane Lines on the Road**

## Writeup Template

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurve_out"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
1. I converted the images to grayscale, using grayscale image can remove color information as well as keep image information in finding lanes
2. I applied Gaussian Blur on grayscale image, with `kernel_size = 5`. Using Gaussian Blur I can filter out some high frequent information on image, reduce noise on following steps
3. I applied Canny Edges with `low_thresh = 50` and `high_thresh = 150`, to keep edge information on the images, with lane outline information included
4. I created region of interest by defining 4 vertices. Bottom two vertices are on bottom left and bottom right of image, top two vertices are around middle of image width and 2/5 of image height.
5. I applied Hough Transformation on canny edge image at interest region, to find lanes.
6. I scanned all lines founded by Hough Transformation, inferred the middle point at left and right lane, then extrapolate to top and bottom by slope
7. Lastly, I draw lines with red color on initial image

In order to draw a single line on the left and right lanes, I modified the `draw_lines()` function by scanning all lines founded by Hough Transformation. During scanning, I decide which side (left/right) one line is belonged to by its slope. After aggregate all lines at left and right side, I then calculated the middle point of left and right lanes, as well as slopes of both lanes. Finally, I extrapolate left and right middle points with their slopes to top and bottom of interest region defined previously.

One example of detected lanes can be seen below:

![alt text][image1]
![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be region of interest. Currently I hard-code region vertices by ration on image shape, the intuition is lanes should most often fall into this region if camera is installed in middle front of car. But when camera position changes or big curves on images, this region of interest is not robust.

Another shortcoming I found could be `draw_lines()` function. Similar to shortcoming above, when extrapolate lanes, I hard-code bottom and top region on image, which could be un-robust. Another thing happened in this function is when no lines are detected on left/right side, there could be no way I could draw lanes, so I have to infer by hard-code.

One last shortcoming is when I experimented on `challenge.mp4`, some frames would have obviously incorrect detections. Right now I could not figure out why this would happen, may have more investigation later.


### 3. Suggest possible improvements to your pipeline

One improvement could be define interest region vertices and `draw_lines()` top/bottom vertices more robustly, by analyzing result of Hough Transformation first.

Another potential improvement could be gaining more intuition on setting parameters of Canny Edges and Hough Transformation. Right now in current pipeline, most of them are actually hard-coded by experiments on example images, while in real situation, I need to gain more intuition on how to setup those parameters, to make pipeline more robust.
