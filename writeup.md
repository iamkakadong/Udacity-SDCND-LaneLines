# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps as following:

1. Convert the image to Grayscale
2. Perform gaussian blur
3. Run Canny edge detection
4. Apply region of interest to limit the edges detected
5. Run Hough transform to find line groups
6. Cluster detected line segments into two clusters based on slope information
7. Extend line segments in each cluster to form a complete line

In order to draw a single line on the left and right lanes, I split the draw_lines()
function into two parts:
1. Cluster detected line segments
2. Interpolate complete line based on line segments in each cluster

### 2. Identify potential shortcomings with your current pipeline

There are several potential shortcomings with the current method:
1. Region of interest is a strong assumption to make and would not be able to
detect any lane outside of the region
2. In the current line segment clustering method, I'm simply using if slope > 0
as the criteria, thus may not work when the lane is not straight, or the position
of the camera is not centered.
3. I use a simple linear-regression method to interpolate lines. It is not robust
to outliers, as can be seen in some frames of output video.

### 3. Suggest possible improvements to your pipeline

There are several ways to improve the current pipeline for the above shortcomings.
1. Use a more relaxed region of interest, e.g. bottom half of the image, and utilize
other informations, such as lanes should be continuous over time, to "learn" the
region of interest over time.
2. The clustering algorithm can be improved to use a k-means method with outlier
removal.
3. Can first remove outliers in the line group, then do regression. Or apply
robust regression method such as Support-vector regression.
