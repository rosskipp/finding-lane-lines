**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps:

1. Convert the images to grayscale
2. Apply a gaussian blur transform
3. Run Canny edge detection
4. Apply a region of interest mask
5. Apply the Hough tranform
6. Run the result of Hough through my `draw_lines` function
7. Print the final image with the calculated lines.

My new `draw_lines` function calls a helper function called `combine_lines`. This new function calculates the slopes and intercepts for all the lines that come out of Hough. It then splits these slopes and interecps into lists of slopes and intercepts where the slope is positive and negative. The next stip is process these two categories (positive and negative slopes and itercepts). Because I was finding that very rarely my Hough transform was not returning any lines, I keep track of the last values for these lists, and return the previous values if there isn't a new value. Otherwise, I take the median of the list in a hope to not be as affected by an outlier line. With these median values for both positive and negative intercepts, I calculate the the points that I want to draw on the images just using y=mx+b and return these lines to the `draw_lines`. They're then drawn on the origin image using the provied `weighted_img` function.


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are false lines that are in the region of interest, but near horizontal. Say from a patched crack. This will really affect my slope and intercpet average system for finding the line.

Another shortcoming could be when the pipeline doesn't find any lines on the road, it just returns the previous set of lines. This could cause some large issues if there are multiple frames where the pipeline can't find any lines. I didn't see this issue, but it would be bad if it did happen!


###3. Suggest possible improvements to your pipeline

A possible improvement would be to filter out more of the lines that have slopes or intercepts that fall way outside the expectation. On the bonus video, my pipline doesn't handle near horizontal lines - and these cause the average line to vary drastically.

Another potential improvement could be to track more of the previous line information and factor that into the current line estimation, like a moving average. So you would keep track of the last 5 lines, and use that as a weighted average with the current estimation. This would improve the frame to frame variation that I have currently.
