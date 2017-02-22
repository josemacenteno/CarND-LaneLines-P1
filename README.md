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
    ![alt text][image_gray]
2. The image goes through a blur filter and canny edge detecion filter. Canny will highlight in white all the pixels where there is high contrast in the image based on the gradient. This effect filters a lot of background and highlights lanes, but it also catches other edges we don care about
    ![alt text][image_canny]
3. A region of interest mask is applied to gnore edges identifies far from the location in the image where we expect to find the lanes. This works under the assumption that we are always capturing video with a similar camera at the same spot relative to the car view.
    ![alt text][image_roi]m  to map points (pixels identified by Canny) into lines, and then lines are grouped together using the cv2.HoughLinesP method which returns several segments that may belong to the a lane. The most important part ofthis homework was to extrapolate from the segments into a single lane inside the region of interest. **I modified the draw_lines function to discard slopes smaller than 0.3, which gets rid of horizontal lines that are painted on the road but don help us identify the lanes. Next, the segments are grouped by sign, positive slopes are used to extrapolate the left lane, negative slope are used to extrapolate the right lane. The segments in each lane are ordered by slope. Outlier sloped segments are discarded (queeping only the middle two quartiles). Finally the remaining segments with close to median slope are grouped together with the function np.polyfit (using a 1 degree polinomial). This gives us a slope and y0 we can use to draw the lanes.** Note: For now we ignore the fact that the line is drawing too much outside of the region of interest.
     ![alt text][image_lanes_full]

5. The region of interest masked is applied again to make sure we don't draw extrapolated lane markings outside of it
    ![alt text][image_lanes_roi]
6. Finally the lane image is combined with the original image to produce the result
    ![alt text][image_combined]    

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
