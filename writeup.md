## **Finding Lane Lines on the Road** 


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

My pipeline consisted of 6 steps. First, the input images are converted into _gray scale_ and a _gaussian mask_ is applied over it. Then we used the canny detection algo to _detect edges_. FOllowd by it is the _Region of Interest (ROI)_ mask wherein we observe only a sub section (a polygon) of the original image on the lookout for lane lines. Then we _drew lines_ and extrapolated them over the entire ROI zone. The final step involved to _overlay_ the current processed image over the original image to see line marking on the actual color image.

In order to draw a single line on the left and right lanes, I modified the _draw_lines()_ function as follows.

- Find a single point representing an hough line, the cenre point. Repeat it for all the hough lines.
- Based on the slope of each hough line (with a threshold of 0.5 to leave out horizontal patches), we decide if it belongs to the left or the right lanes
- Simultaneously loop through the identifed points to find top most (max point) and bottom most (min point) point for left and right lines.
- An addition helper function _extrapolate_line_ extends this max and min points to the ROI boundaries.
    - If the number of detected lines (at the end of hough transform) if one or two, we take the identified lines start and end points to extrapolate the lines.
    - If we have more than two points, we use the centre points of lines to evtrapolate and draw lane lines 
- Draw a line on either sides connecting the extreme points.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when we have more curved lines on the road. As this techinique of line extrapolation uses only the strat an dend points to draw lines, it would likeley fail with curved lines. 

Another shortcoming could be when we have missing lanes or missing parts of lanes (white or yellow patches missing due to wear out) at top and bottom which could mislead the extrapolation to extend lines over a large imaginary zones (which is ofcourse undersirable).


### 3. Suggest possible improvements to your pipeline

It would be better to skip few frames inbetween rather than fining lanes across all consecutive frames.
A possible improvement would be to adopt better extrapolation techinique so that it works better with curved lanes (as with the challenge example).
