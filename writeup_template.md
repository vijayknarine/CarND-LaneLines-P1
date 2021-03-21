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

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]

Image processing pipeline is created with the following steps:

![pipeline]['Pipeline.png']


Adjusted the parameters based on the test images as below:

##### Canny edges:

low_threshold = 255/3
high_threshold = 255/2

##### Region of interest

These vertices are hardcoded into the code and optimized based on the ROI selected in each test image. 

vertices = np.array([[(120,539),(880,539),(510,310), (460,310)]])  

used CV2 to plot the points and the used matplotlib to visualize the actual points that makes the polygon. 

##### Hough Transform. 

Since the line is passing below origin choosed rho = 1, theta = np.pi/180 , threshold=40,min_line_len=10, max_line_gap=60

Tuned the values of threshold, max_line_gap , min_line_lenth based on all 05 test images. Changing rho to 2 and adjusting the other parameters resulted in intersecting lanes. 

Modified the draw lines by detecting slope of each line and based on the value ( either negative or positive ) created 02 separate array of lines. identified the mean slope and intercept values and extrapolated the lines from the bottom of the image to the top points ( y_value )  in the region of interest. 

This performed better the first white road video, however on the Yello road video, it resulted in intersecting lanes. Upon on analysis identified , there are outliers such as horizontal lines in the yellow lane, white car moving near the white lane. 

Plotted the histogram of the slope of yellow road video ( subclip from 6 to 10 secs ) , identified outliers exist between the values -0.5 and 0.5. Updated the drawlines function to reject all slopes which lies in this range, this resolved the issues in the Yellow video 



### 2. Identify potential shortcomings with your current pipeline


1. Vertices of ROI are hardcoded.
2. If road has curvatures , lane lines will not be detected by HoughTransform. 
3. Pipeline failed in the optional challenge video. Side wall and it's edges are detected as lanes by the Pipeline, fails to detect the lanes covered with the tree shadows. 

### 3. Suggest possible improvements to your pipeline

1. scanning the image with multiple smaller Regions of interest and identifying Lines in those smaller ROCs and extrapolating it to the whole image may help to detect curves.
2. Applying dilation may remove Shadows of the tree ,  still exploring this option. 
