## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

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
[image1]: ./output_images/processed_images.png "Processed Image Grid"
[image2]: ./output_images/dist-undist-image.png
[image3]: ./output_images/curvature_measurements.png
[image4]: ./output_images/corners.png "Chessboard Corners"
[image5]: ./output_images/fitpolynomial.jpg

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `ouput_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

### Camera calibration
The objpoints and imgpoints are then used to compute the camera calibration matrix and distortion coefficients using the cv2.calibrateCamera() function.

![alt text][image4]
<p align="center"><b>Chessboard corners</b></p>
<br>
### Distortion Correction
The cv2.undistort() functin was used for distortion correction on same chessboard images. The undistort_img() function serves the purpose of applying cv2.calibrateCamera() and cv2.undistort() to the input image.
![alt text][image2]
<p align="center"><b>Processed Image Grid</b></p>
<br>

### Color & Gradient threshhold
Used combination of color and gradient thresholds to generate binary image.

### Perspective transform
A perspective transform maps the points in a given image to different, desired, image points with a new perspective. The perspective transform in a bird’s-eye view transform. Check the wraped image in the below grid.
![alt text][image1]
<br>

### Locate the Lane Lines and Fit a Polynomial
After applying calibration, thresholding, and a perspective transform to a road image, you should have a binary image where the lane lines stand out clearly. However, you still need to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line. This is done by finding peaks in the histogram.
![alt text][image5]
<p align="center"><b>Fit polynomial</b></p>
<br>

### Sliding window search
We first check the histogram of the lower of image and find the two peaks for the left and right lines. Then we use the sliding window method to work our way upwards and find the relevant points in the image which mark the lane. Next, we use the np.polyfit() method to fit a second degree polynomial to these points. I did this in the fit_polynomials() function.

### Measuring Curvature
![alt text][image3]
<p align="center"><b>Radius and Offset Measurement</b></p>
<br>
Given the detected points, we determine a second degree polynomial that fits these points, and then we calculate the radius of curvature of this polynomial. Also, we convert from the pixel space to real space in meters. I did this in calculate_curvature_offset() function.

### Issues faced

* Performance seems to be an issue when I processed videos. The algorithm can't be applied on real-time videos using laptop.
* very strong bends like in the harder challenge video, are difficult to find using the sliding windows
* Thresholds can be further improved, especially for different lighting and track conditions
* I am yet to try all the techniques to be used for building a robust pipeline - sanity check, reset, smoothing etc., 