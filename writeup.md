**Advanced Lane Finding Project**

The goals / steps of this project are the following:

1. Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
2. Apply a distortion correction to raw images.
3. Use color transforms, gradients, etc., to create a thresholded binary image.
4. Apply a perspective transform to rectify binary image ("birds-eye view").
5. Detect lane pixels and fit to find the lane boundary.
6. Determine the curvature of the lane and vehicle position with respect to center.
7. Warp the detected lane boundaries back onto the original image.
8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion co-efficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the code cell *Step 1: Camera Calibration* of the [project's Jupyter notebook](./Project.ipynb).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the real world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners, using `cv2.findChessboardCorners()` function, in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. futher, I used the `cv2.cornerSubPix()` function to fine tune the detected corners from each chessboard image.

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function. Following images show the calibration process:

![camera calibration](/output_images/camera_calib.jpg)

Next, I applied the above distortion correction to the test image using the `cv2.undistort()` function, (code cell *Step 2: Image Undistort*), and obtained this result: 

![calibration undistort](/output_images/calib_undistort.jpg)

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I applied the `undistort()` function to the test image, bearing the following result:

![image undistort](/output_images/undistort.jpg)

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I started off by defining functions for color and gradient thresholds to generate a binary image (code cell *Step 3: Image Thresholding*). Gradient thresholding functions include `abs_sobel_thresh(), direction_threshold()` and `magnitude_threshold()`, all based on `cv2.Sobel()` function. Color thresholding functions include `color_threshold_hls(), color_threshold_rgb()` and `color_threshold_lab()` functions. These aforementioned functions are then tied together inside the `combine_grad_mag_dir_color_threshold()` function. At first I combined all both the gradient and color thresholds, however, after numerous tests, the best solution was to only use color thresholding based on *RGB* and *Lab* thresholding. The *HLS* thresholding was resulting in extraneous pixels 
 that create issues when line detection is run later on, hence it was not used. Here's an example of my output for this step.  

![thresholded](/output_images/thresholded.jpg)

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()` (code cell *Step 4: Image Warping*).  The `warp()` function takes as input an image `img` and then based on the `src_points` & `dest_points`, I used the `cv2.getPerspectiveTransform()` to obtain the *transform matrix* and the *inverse trasnform matrix*. Next, I used the `cv2.warpPerspective()` to perform the actual warping operation. The *inverse trasnform matrix* is used later to project the lane area on the original image. For warping, I chose to hardcode the source and destination points in the following manner:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 570, 470      | 400, 0        | 
| 720, 470      | 900, 0      |
| 1050, 690     | 900, 720      |
| 245, 690     | 400, 720       |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![warped](/output_images/warped.jpg)

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Having obtained the warped image, I proceeded towards identifying lane-line pixels by first getting a histogram of the warped image (3/4 of image height approx.) as below (code cell *Step 5: Image Histogram*):

![histogram](/output_images/histogram.jpg)

The positions of the peaks in the histogram are then used as the start points for identification of the lane-line pixels.

The actual process of line identification happens in functions `find_lane_lines_with_window_search()` & `find_lane_lines()` (code cell *Step 6: Finding Lane Lines (Window Search & Targeted Search)*). 

The `find_lane_lines_with_window_search()` function performs line search from scratch based on the histogram technique & using window-based strategy. First a histogram is built and then the peaks in the histogram are used as the base for starting the line search. The image is divided into `n` number of windows (`nwindows` parameter) vertically. The horizontal size of these windows is based on the +/- `margin` parameter. The search starts from the bottom & then moves up. For each window, if the specified pixels (`minpix` parameter)  are identified then the next window is re-centered based on the mean position of the found pixels. In the end, lines are fit based on the identified line pixels with a 2nd order polynomial.

```python
left_fit = np.polyfit(lefty, leftx, 2)
right_fit = np.polyfit(righty, rightx, 2)
``` 

Here's the output of the window search on the test image:

![window search](/output_images/window_search.jpg)

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
