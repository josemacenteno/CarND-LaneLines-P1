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
    ![alt text][image_roi]
    
4. A Hugh transform  to map points (pixels identified by Canny) into lines, and then lines are grouped together using the cv2.HoughLinesP method which returns several segments that may belong to the a lane. The most important part ofthis homework was to extrapolate from the segments into a single lane inside the region of interest. 
    **I modified the draw_lines function to discard slopes smaller than 0.3, which gets rid of horizontal lines that are painted on the road but don help us identify the lanes. Next, the segments are grouped by sign, positive slopes are used to extrapolate the left lane, negative slope are used to extrapolate the right lane. The segments in each lane are ordered by slope. Outlier sloped segments are discarded (queeping only the middle two quartiles). Finally the remaining segments with close to median slope are grouped together with the function np.polyfit (using a 1 degree polinomial). This gives us a slope and y0 we can use to draw the lanes.** 
    
    Note: For now we ignore the fact that the line is drawing too much outside of the region of interest.
     ![alt text][image_lanes_full]

5. The region of interest masked is applied again to make sure we don't draw extrapolated lane markings outside of it
    ![alt text][image_lanes_roi]

6. Finally the lane image is combined with the original image to produce the result
    ![alt text][image_combined]    


###2. Identify potential shortcomings with your current pipeline

The region of interest mask is defined very tightly around the areas in the video based on the examples. If the camera is moved even slightly, or placed on a different car, or even if another camera is used the algorithm may failed badly.

The algorithms used also kind of rely on fixed parameters assuming brightness, contrast and colors similar to the ones from the example videos. This only applies to a very small subset of situations in the ral world.


###3. Suggest possible improvements to your pipeline

* My code fails on the challenge. I would need to improve the code robustness. For example I have some divisions where I don check if the denominator could be 0 for example.
* The final result is a little bit jumpy. If I keep track of prevous lanes I may be able to smooth transitions, I could also apply some kind of stabilizing algorithm to the input video itself 
* Parameters could be calculated dinamically based on the image characteristics. For example a histogram could help better identify threshold values for the filters. 
* Adding more advanced algorithms can help to identify lanes on challenging frames.
* Preprocessing the image to make contrast more evenly distributed across frames might help


###4. Results:
White lanes video:
[![WHITE LANE](http://img.youtube.com/vi/RuXgtg9qRJE/0.jpg)](http://www.youtube.com/watch?v=RuXgtg9qRJE)


Yellow lane on left video:
[![YELLOW LANE](http://img.youtube.com/vi/8_xE28lyCB8/0.jpg)](http://www.youtube.com/watch?v=8_xE28lyCB8)


