#**Finding Lane Lines on the Road** 

[//]: # (Image References)
[image_gray]: writeup/gray.jpg
[image_canny]: writeup/canny.jpg
[image_roi]: writeup/roi_edged_image.jpg
[image_lanes_full]: writeup/lanes_image.jpg
[image_lanes_roi]: writeup/roi_lanes_image.jpg
[image_combined]: writeup/combined.jpg

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:

1. The original image is converted to grayscale, which improves contrast and simplifies gradient calculations. 
2. The image goes through a blur filter and canny edge detecion filter. Canny will highlight in white all the pixels where there is high contrast in the image based on the gradient. This effect filters a lot of background and highlights lanes, but it also catches other edges we don care about
3. A region of interest mask is applied to gnore edges identifies far from the location in the image where we expect to find the lanes. This works under the assumption that we are always capturing video with a similar camera at the same spot relative to the car view.
4. A 
5. 
6. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image_gray]
![alt text][image_canny]
![alt text][image_roi]
![alt text][image_lanes_full]
![alt text][image_lanes_roi]
![alt text][image_combined]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
