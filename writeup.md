# **Finding Lane Lines on the Road** 

## ... the "magical power of a few lines of code" ...
### ... and "how difficult it can become in the end" 

---

## Goal: Finding Lane Lines on the Road

The goals / steps of this project were to identify lane lines which were visible on video files. 

- Make a pipeline that finds lane lines on the road
- Reflect on your work in a written report

The solution to this project is provided in the GitHub repository. Download instructions below.

The pipeline is even able to handle the "optional challenge" in an almost satisfying way!


[//]: # (Image References)

[image10]: ./writeup_images/1_mask_solidYellowCurve.jpg "Original image"
[image11]: ./writeup_images/2_HLS_image_solidYellowCurve.jpg "HLS image"
[image12]: ./writeup_images/3_grayscale_solidYellowCurve.jpg "Grayscale image"
[image13]: ./writeup_images/4_canny_hough_solidYellowCurve.jpg "Canny"
[image14]: ./writeup_images/5_final_lines_solidYellowCurve.jpg "Final image"


[image1]: ./writeup_images/grayscale.jpg "Grayscale" 
[image2]: ./writeup_images/mask.jpg "Mask"
[image3]: ./writeup_images/canny.jpg "Detected edges"
[image4]: ./writeup_images/final.png "Interpolated lines"
[image5]: ./writeup_images/tree3.jpg "Tree"

---

## How to run the code / Download instructions
First of, start with `git clone https://github.com/thelukasssheee/CarND-LaneLines-P1.git` to copy the repository to your computer.

The program is written in a jupyter notebook under the filename `P1.ipynb`. Dependencies can be seen inside the file.

The script depends on image files being stored in the directory `./examples` and on video files stored in `./test_videos`. Please make sure you have copied both. 

## Known issues
In case you run into a "MoviePy" error, this is likely caused by a problem with the movie output directory (either not available or a problem with write privileges). Please add the directory `./test_videos_output` to the same path where `P1.ipynb` is located and make sure that jupyter can write to this directory.

---
### 1. Pipeline description

At first, I started right away with conversion from RGB base image to gray. However, this process was not suited to handle the "optional challenge": there were too many lines identified wrongly... Therefore I added another step beforehand which helped to greatly improve the codes effectiveness. 

The current pipeline extracts white and yellow information on the image files beforehand. 

All color information outside the defined range is being discarded. The result can be seen below. 

![Original base image][image10]
![HLS filter process][image11]

As you can clearly see, the lane lines are distinguishable quite nicely.

The first image also shows a **mask**, which is being applied later. The mask is used to limit the information to the relevant area for lane lines only. It was formed by a 4-point polygon, as indicated in the first image already. Based on the training photos, the mask was ranging from the lower bottom of the image, at first! However, in the optional part of this project, the vehicle's hood was visible at the lower bottom and this was causing a lot of trouble. Therefore, the lower points had to be moved a little bit upwards.  

After this step, the image is **converted to grayscale** for easier processing. The lines are clearly visible and a great candidate for further processing. 

![Grayscale Version][image12]

To identify the edges, a **Canny edge detection algorithm** is being applied. The edges within the image are being identified based on the gradient of grayscale values around each pixel. With a `Lower Threshold` of **60** and an `Upper Threshold` of **120**, it was possible to obtain acceptable results

In order to actually identify the line information from the Canny'ed image, a **Hough transformation** was applied. This way, each of the detected edges within the mask was converted to polar coordinates and the line information could be extracted after the parameters were tweaked to complete the task sufficiently. The result was an image, where the lane lines were already pretty well identified. Some smaller lines are not being identified, but the overall result with the parameters used was sufficient. 

The image of the optional challange was made with higher resolution. Thus, constant values for the Hough transform parameters of the Hough transform function were a problem... Therefore, they were **normalized** and **scaled accordingly** to the vertical image dimensions.

![Canny Hough][image13]

**Final step: Extrapolation of lane lines**: Beyond detecting individual lines, the task was to extrapolate this information and generate two single, averaged lines. To accomplish this task, I sorted all detected lines into groups with a negative gradient and a positive gradient, thus dividing information for the left and the right line. By calculating simple averages for an equation like `y = mx + b`, it was possible to draw the two averaged lines. The actual result can be seen below. 

![Final image, extrapolated lane lines][image14]


### 2. Potential shortcomings with current pipeline


As soon as light conditions become less ideal, e.g. by a tree casting some shadows on the road, the pipeline can be distracted. 

However, by adding the yellow/white separation as additional step, this problem was solved. The "optional challenge" runs pretty smooth. For some frames, one of the lines cannot be detected.  


### 3. Suggest possible improvements to your pipeline

Already applied: 

~~ Increase image's contrast beforehand, thus possibly reducing the effect of shadows ~~
~~ Instead of a simple grayscale conversion, increase the trust-worthiness of light colours (white, yellow) instead of grayish dark colours ~~


There are several options to further improve the pipeline:

* Define limits for gradients based on what is likely to occur. Discard all detected lines with gradients outside of the trusted range.
* Take information from previous images into account. This could help reduce the slight unsteadiness / shaking of the lines.
* Define the tweak parameters dynamically, taking different light situations into account

