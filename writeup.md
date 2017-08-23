## Project: Search and Sample Return
---
**Problem statement**  
The goal of this task is to make an autonomous rover which automatically navigates on a lunatic surface.
The task for the rover is to locate yellow rocks from the ground and simultaneously update the map describing the
 layout of the surface.


[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 
[image4]: ./misc/image1.png
[image5]: ./misc/image2.png
[image6]: ./misc/image3.png
[image7]: ./misc/image4.png
[video]: ./output/test_mapping.mp4
## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  
![alt text][image1]

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
The image of a lunatic surface on a 1m x 1m grid (left) and a rock used for testing the rock detector:

![alt text][image4]

The left image after the perspective transformation:

![alt text][image5]


The below image shows color thresholded navigatable terrain (left), obstacles (middle) and the detected yellow rock (right).

![alt text][image6]

Finally the navigatable terrain image is tranformed to rover coordinate system and the navigation vector is plotted:

![alt text][image7]



#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

The `process_image()` function executes sequence of operations which results are shown above (perspective transform, 
color thresholding, coordinate transformation).

![alt text][video]

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

For most of the parts the `perception_step()` function was constructed as explained during the lectures and exercises.
Only differences were the obstacle and rock detector which use slightly different thresholding values for the images.
The `rock_detector()` transforms the image to HSV color space where the yellow color was easier to segment. In addition 
false alarms for rocks were reduced by counting the number of pixel which are left from the color segmentation.
Also to increase fidelity a conditional check was added to check whether the rover pitch and roll were reasonable.

`decision_step()` function is almost untouch: the rover moves forward if there is a open space and if not it will turn around.
While experimenting with the autonomous driving it was noted that the rover can get easily stuck on bigger rocks on the lunar ground.
Thus in `decision_step()` a functionality to recover from situation where the rover got stuck was implemented. 
In the function we check if the throttle is non-zero and velocity is zero. If these condition hold we assume that the rover
got stuck and simply try to turn the rover 15 degree.

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

The simulator was running at 27 FPS with 1920x1080 resolution and 'Fantastic' graphics quality.
 
My approach seems to work as expected and I can achieve 80-85% fidelity in most of the runs when 40% of the map is covered.

Drawbacks of my implementation are that it does not implement the rock picking and there is still lots of room for
optimization. For instance rover could check whether it has already visited some branches of the map and sometimes the rover
starts to shake at high speed due to variation of navigation angle between consecutive estimates even in straight sections. Also the functionality to check 
if the robot is stuck could be improved and made more sophisticated.

Hopefully in the future I am able to revisit the project and improve the current implementation.



