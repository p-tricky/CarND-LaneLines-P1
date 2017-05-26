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

My pipeline consists of 6 steps.  Firts I convert the image to grayscale. Next, I
blur the grayscale image with a 5x5 Gaussian kernel.  Then, I perform canny edge
detection to sharpen the image.  I mask all of the image except for the region
where I expect the lines to be.  Finally, I perform a hough transform and send
the lines to my draw lines function.  

In order to draw a single line on the left and right lanes, I modified
the draw_lines() function by first filtering out any of the lines that have
considerably different slope or positioning than I would expect from lane lines.
First, I transformed the lines to slope-intercept form. Expecting to see a bimodal distribution
with peaks corresponding to the angles of the lanes, I created a histogram of the
slopes.  From the historgam, I determined angle thresholds for each lane line.
I assigned lines to the left or right lane using the thresholds.  To filter out
lines based on positioning, I filtered out any lines whose intercept deviated by more
than two standard deviations from its lane average.

After the filtering step, I averaged each lanes slope and drew the two lane lines from the
bottom of the screen to a point 30 pixels before the point of lane line intersection.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is a curve in the
road.  Since I am just averaging slopes, there is no way to account for a curve.

Another shortcoming could be changes in camera positioning.  If the camera got bumped
or if it was sitting at a different height/position (e.g., attached to a different sized car)
the algorithm would stop working because the lane angle ranges and intercept positions are
hardcoded into the algorithm.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to connect the hough transform line output instead of averaging it.
This way I could account for curves, assuming the curved parts of the lines survived the filtering
step.

Another potential improvement could be to use a smarter algorithm.  If I trained a classifier to
identify lane lines then I wouldn't have to worry as much about camera positioning.
