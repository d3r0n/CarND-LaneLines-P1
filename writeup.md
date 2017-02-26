**Finding Lane Lines on the Road**
### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.
First, I converted the images to grayscale.
Second, I use Gaussian blur to smoothen the image and reduce noise.
Using Canny edge detection algorithm to find edges of objects.
Fourth, I select area where most likely is the road to reduce false positives.
Fifth is the most complex step. Here I detect lines.
  - I use Hough Transformation on the found points to find formulas of lines.
  If enough lines corresponding to found points crosses in Hough space it means that they are one line.
  - I filter out the lines with too steep or too flat slope since they are false positives adding too much noise.
  - I split set of found lines according to their slope parameter. Those with positive alpha are part of left lane line and those with alpha negative are part of right lane.
  - In those groups I compute average slope and the intercept of found functions, giving me approximation of the lane line in current frame.
  - Since this approximation, due to shadows, flares etc., can be skewed I average the lane function (slope and intercept) with previous 50 lane functions using Weighted Moving average.
    I was inspired by Exponential Smoothening but here instead of smoothing the pixels on the image I smooth the functions. The result is more than satisfying.
  - Last in this step, by using found function definitions I paint lane highlights.
Sixth step is a Exponential Smoothening on painted lane highlights using last 10 frames to reduce flicker.

###2. Identify potential shortcomings with your current pipeline

My solution successfully find white lane in optional challenge but unfortunately still have a problem with yellow lane.

Other shortcoming is that if the road is not in the centre of the video the code needs recalibrating. Same might happen in less sunny video.
In rain, in snow...

###3. Suggest possible improvements to your pipeline

To deal with sun and shadows a possible improvement would be to use different color channel than RGB. Something like HLS or YCbCr.

Another potential improvement could be to some advanced methods like DNN.
