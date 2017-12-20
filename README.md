# Project: Advanced Lane Finding
## Overview   
   
This project is about writing a software pipeline to identify the lane boundaries in a video mainly using computer vision techniques via [Open CV](https://opencv.org/) library. Either individual images can be fed or a video clip can be used as an input. 

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

For video clips, same logic is applied by processing individual frames.

## Directory Structure
* **test_images:** Directory containing sample images for testing
* **test_images_output:** Directory containing processed images
* **test_videos:** Directory containing sample videos for testing
* **test_videos_output:** Directory containing processed videos
* **writeup_images:** Directory containing images for project writeup
* **P1.html:** Html output of the Python notebook
* **P1.ipynb:** Python notebook containing the source code
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
**Images**

*Raw Hough Lines*
```python
image_list = os.listdir("test_images/")
for image_name in image_list:
    full_image_path = 'test_images/' + image_name
    find_lane_lines_raw(full_image_path)
```

*Extrapolated Lane Lines*
```python
image_list = os.listdir("test_images/")
for image_name in image_list:
    full_image_path = 'test_images/' + image_name
    find_lane_lines(full_image_path)
```

**Video**

*Raw Hough Lines*
```python
white_output_raw = 'test_videos_output/raw-solidWhiteRight.mp4'
clip1_raw = VideoFileClip("test_videos/solidWhiteRight.mp4")
white_clip_raw = clip1_raw.fl_image(process_image_raw_lines) 
%time white_clip_raw.write_videofile(white_output_raw, audio=False)
```

*Extrapolated Lane Lines*
```python
white_output = 'test_videos_output/solidWhiteRight.mp4'
clip1 = VideoFileClip("test_videos/solidWhiteRight.mp4")
white_clip = clip1.fl_image(process_image) 
%time white_clip.write_videofile(white_output, audio=False)
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