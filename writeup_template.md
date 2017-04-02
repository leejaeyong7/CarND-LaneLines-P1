# **Finding Lane Lines on the Road** 
### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 4 steps.

First, I filtered out the images with white and yellow filtering criteria, where white color criteria filters out all pixels with one of RGB value less then 200, and yellow criterial filters out all pixels with following criteria: 

> ( (G * 0.6 < R < G * 1.5) | (G - 30 < R < G + 30) ) & ( R > B + 70 | R > B * 1.5 ) & (R > 140) & (B < 150)

which restricts Yellow color as: R in somewhat close range of G, R larger than some threshold of B, R over some threshold, B less then some threshold.

Second, I have converted images to grayscale and performed gaussian blur and canny edge detection to obtain edges.

Third, I have performed hough transform with masking ROI and tuned parameters.

Finally, I drew line over image using modified draw_lines function.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by filtering lines by slope and position. More precisely, I have filtered out all left/right lines whose slope does not fit in between -10/4 and -4/10, and 4/10 and 10/4 respectively for left and right lines. Then, I performed least squares fit to estimate optimal slope and disposition for left and right line. Finally I've drawn both lines from y = minimum value of ROI_y to y = maximum value of ROI_y where ROI is a region I've used for hough transform.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when there are lot of outliers in edge slope calculation. If there are lot of outliers, since Least Squares solution is very susceptible to outliers, it will yield slope with a lot of offset. 

Another shortcoming could be due to color filtering. Since my definition of white and yellow color in mathmatical formula is done by purely filtering based on pixel values, at night or on extreme weather condition, it may not be robust in color filtering.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to run RANSAC or Robust Least Squares to exclude outliers instead of regular Least Sqaures fit.
Another improvement would be 

Another potential improvement could be to have a color filtering based on gradient. Instead of filtering with threshold, if we detect relative change in color toward white or yellow, it will be robust regardless of night or day. This also can possibly remedied(but not quite optimally resolved) by performing white balancing / yellow balancing of the image and filtering out with threshold.