# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteCurve.jpg "Before Extrapolation"
[image3]: ./test_images_output/extrapolated_solidWhiteCurve.jpg "After Extrapolation"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I added guassian blur with a kenel
size of 5 to help limit spurious lane detection, then I used canny edge detection with a low threshold of 50 and a high
threshold of 150, then I masked the region with a quadrilateral that was most likely to contain relevant lane markers,
then I used hough transforms to take "connect" pixels determined by canny edge detection as being part of lines into
actual lines.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first classifying
lines as either belonging to the left or the right line marker based on their slope. If the slope was positive, this
indicated a right line marker and if it was negative a left line marker. (This is the exact opposite of what you would expect
if the origin were at the bottom left corner, but this is caused by the origin being in the top left corner and becoming
more positive as you move down.) Next, I used polyfit to extrapolate a line (i.e. a polynomial with only 1 degree of freedom)
that best fit the data. Then I calculated the end points of my lines by calculating the x values given a y value that
was equal to the height of the image and three-fifths of the height of image for each lane respectively. This created a
single line segment for each line that started and ended at approximately where I wanted them to for purposes of modeling
where the location of the lane lines were.

The below images show edges detected before and after extrapolation.

##### Before Extrapolation
![image2]

##### After Extrapolation
![image3]

### 2. Identify potential shortcomings with your current pipeline

The main shortcoming was that my edge detection approach was a little bit too vulnerable to noise. This was illustrated
from some small "flickering" deviations from the actual lanes that my model exhibited. The challenge video also shows
that my approach was vulnerable to the presence of other cars in oncoming lanes.


### 3. Suggest possible improvements to your pipeline

I could make it more robust by playing with the parameters (for example, using a larger kernel with gaussian blur). I also could also use a 
Bayesian model that would adjust new data that seems unlikely given past data. However, if enough of the new data was
discovered, it would accept that things have changed (perhaps because of an accident or unusual event).

For the challenge, where problems were caused by cars, if I were to address it the Bayesian approach would likely be
useful. Also, trying to limit the region of interest so that data from the oncoming lane would be less likely to be 
detected. Finally, trying to use computer vision techniques to explicitly detect and ignore cars would obvioulsy be
helpful (but also maybe overkill as this point).
