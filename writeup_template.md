# **Finding Lane Lines on the Road** 

## ... the "magical power of a few lines of code" ...
### ... and "how difficult it can become in the end" 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

- Make a pipeline that finds lane lines on the road
- Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./images_output/grayscale.jpg "Grayscale" 
[image2]: ./images_output/mask.jpg "Mask"
[image3]: ./images_output/canny.jpg "Detected edges"
[image4]: ./images_output/final.png "Interpolated lines"
[image5]: ./images_output/tree3.jpg "Tree"

---

### Reflection

### 1. Pipeline description

In the beginning of my pipeline, I collected all variables. This way it becomes easier to tweak the algorithm.

My pipeline consisted of the following steps:

**Conversion to grayscale**: The image is being converted to grayscale.

![Conversion to grayscale][image1]

**Gaussian smoothing**: Blurring the image helped to further reduce the noise. A `radius` of **3** pixels was sufficent. 

**Edge detection**: Using Canny's edge detection algorithm, the edges within the image are being identified based on the gradient of grayscale values around each pixel. With a `Lower Threshold` of **60** and an `Upper Threshold` of **120**, it was possible to obtain acceptable results

**Mask application**: A mask was applied, to limit the information to the relevant area only. It was formed by a 4-point polygon, as indicated in the image below. Based on the training photos, the mask was ranging from the lower bottom of the image, at first. 
However, in the optional part of this project, the vehicle's hood was visible in the lower part of the images and causing a lot of trouble. Therefore, the lower points had to be moved a little bit upwards.  

![Mask application][image2]

**Hough transformation**: In order to actually identify the line information from the Canny'ed image, a Hough transformation was applied. This way, each of the detected edges within the mask was converted to polar coordinates and the line information could be extracted after the parameters were tweaked to  complete the task sufficiently. The result was an image, where the lane lines were already pretty well identified.

![Identified lane lines][image3]

**Extrapolation of lane lines**: Beyond detecting individual lines, the task was to extrapolate this information and generate two single, averaged lines. To accomplish this task, I sorted the detected lines into groups with a negative gradient and a positive gradient, thus dividing information for the left and the right line. This way, it was becoming possible to draw the two averaged lines.  

![Interpolated lane lines][image4]







### 2. Identify potential shortcomings with your current pipeline


As soon as light conditions become less ideal, e.g. by a tree casting some shadows on the road, the pipeline can be distracted. This can be seen in the image below. 
![Tree][image5]


### 3. Suggest possible improvements to your pipeline

There are several options to further improve the pipeline:

* Define limits for gradients based on what is likely to occur. Discard all detected lines with gradients outside of the trusted range.
* Increase image's contrast beforehand, thus possibly reducing the effect of shadows
* Instead of a simple grayscale conversion, increase the trust-worthiness of light colours (white, yellow) instead of grayish dark colours
* Take information from previous images into account. This could help reduce the slight unsteadiness / shaking of the lines.
* Define the tweak parameters dynamically, taking different light situations into account
