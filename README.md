# Project: Advanced Lane Finding
## Overview   
   
This project is about writing a software pipeline to identify the lane boundaries in a video mainly using computer vision techniques via [Open CV](https://opencv.org/) library.

## How Does It Work?
The logic comprises an image processing pipeline with the following steps:

1. Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
2. Apply a distortion correction to raw images.
3. Use color transforms, gradients, etc., to create a thresholded binary image.
4. Apply a perspective transform to rectify binary image ("birds-eye view").
5. Detect lane pixels and fit to find the lane boundary.
6. Determine the curvature of the lane and vehicle position with respect to center.
7. Warp the detected lane boundaries back onto the original image.
8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

For a detailed description of this project, please view the [project writeup](./writeup.md).

## Directory Structure
* **test_images:** Directory containing sample images for testing
* **output_images:** Directory containing sample processed images
* **camera_cal:** Directory containing 17 chessboard images for camera calibration
* **project_video.mp4:** Video clip for testing the image processing pipeline
* **project_video_output.mp4:** Processed video clip via the image processing pipeline
* **Project.html:** Html output of the Jupyter notebook
* **Project.ipynb:** Jupyter notebook containing the Python source code
* **README.md:** Project readme file
* **writeup.md:** project writeup file containing detailed information about the inner workings of this project

## Requirements
* Python-3.5.2
* OpenCV-3.3.0
* numpy
* matplotlib
* moviepy
* Jupyter Notebook


## Usage/Examples

```python
log_file = open('./log.txt', 'w')                     
l_line = Line()
l_line.direction = 'Left'
r_line = Line()
r_line.direction = 'Right'
lane = Lane()
frame_counter = 0

video_output_raw = './project_video_output.mp4'
clip1_raw = VideoFileClip('./project_video.mp4')
video_clip_raw = clip1_raw.fl_image(process_frame) 
%time video_clip_raw.write_videofile(video_output_raw, audio=False)

log_file.close()
```

## Troubleshooting

**ffmpeg**

NOTE: If you don't have ffmpeg installed on your computer you'll have to install it for moviepy to work. If this is the case you'll be prompted by an error in the notebook. You can easily install ffmpeg by running the following in a code cell in the notebook.

```python
import imageio
imageio.plugins.ffmpeg.download()
```

Once it's installed, moviepy should work.

## License
The content of this project is licensed under the [Creative Commons Attribution 3.0 license](https://creativecommons.org/licenses/by/3.0/us/deed.en_US).