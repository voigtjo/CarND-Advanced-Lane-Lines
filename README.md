## Writeup

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[img01]: ./output_images/camera_cal/calibration_undistorted.jpg "Undistorted Chessboard"
[img02]: ./output_images/image_processing/02_distortion_correction/test4.jpg "Undistorted"
[img03]: ./output_images/image_processing/03_binary_image/test4.jpg "Binary"
[img04]: ./output_images/image_processing/04_binary_warped/test4.jpg "Warp Example"
[img05]: ./output_images/image_processing/05_binary_warped_fit2/test4.jpg "Fit Example"
[img06]: ./output_images/image_processing/08_process_image/test4.jpg "Output"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the codes cell 2 through 4 of the IPython notebook located in "./LaneLine.ipynb"  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  
The "image points" are calculated using the OpenCV-function `findChessboardCorners`

The object and image points are passed to the OpenCV-function `calibrateCamera` which returns camera calibration and distortion coefficients. These can then be used by the OpenCV `undistort` function to undo the effects of distortion on any image produced by the same camera.

The image below shows the undistorted chessboard calibration image:

![alt text][img01]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

The image below shows the results of applying `undistort` in chapter "2. Apply a distortion correction to raw images" to the project dashcam image test4.jpg:
![alt text][img02]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a  combination of HLS  and gray image thresholds to generate a binary image in chapter "3. Use color transforms, gradients, etc., to create a thresholded binary image" (code cell 7) .  Here's an example of my output for this step.  

![alt text][img03]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, in chapter "4. Apply a perspective transform to rectify binary image ("birds-eye view")" (code cell 9). The `warp()` function takes as inputs the binary img from the previous step (`binaryImg`), as well as the undisted image to receive the heigth and width on witch the source (`src`) and destination (`dst`) points depend.  
```python
src = np.float32([(575,464),
                      (707,464), 
                      (258,682), 
                      (1049,682)])
    dst = np.float32([(450,0),
                      (w-450,0),
                      (450,h),
                      (w-450,h)])
```


Here is an example of my perspective transformation:
![alt text][img04]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I did this in chapter "5. Detect lane pixels and fit to find the lane boundary" and fit my lane lines with a 2nd order polynomial. You can see an example output below:

![alt text][img05]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I calculated the radius of curvature of the lane and the position of the vehicle in the chapter "6. Determine the curvature of the lane and vehicle position with respect to center." (code cell 18).

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this in chapter "7. Warp the detected lane boundaries back onto the original image" and chapter "8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position."  in the function `process_image` with several helper functions.  Here is an example of my result on the test image:

![alt text][img06]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The solution is just a starting point for real life conditions, which means that the lane is always limited by lane lines.
