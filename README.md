# Peanute(M&M) Segmentation

This project was developed as the first project for the Computer Vision course in the Master's in Electrical and Computer Engineering (MEEC) program at the Faculty of Engineering of the University of Porto (FEUP). The goal is to perform segmentation, using traditional techniques, of M&M chocolates of various colors and obtain some statistics such as the number of M&Ms in each photo, area, and standard deviation. In this case, this will be done for two different types of backgrounds: a white one, which would be the initial step, and a noisy one.

For more details about the project description, please access: https://drive.google.com/file/d/1pdEiIkSEW8Pbl_CM81_OZaXBAsyPmPQC/view?usp=sharing

# Data visualization

Below is an example of each background like M&M candies of various colors to understand the project's difficulty. For the white background, we will have up to 4 colors of M&M candies, namely blue, red, yellow, and green. As for the noisy background, we have the previous colors plus the brown color.

<p float="middle">
    <img src="images/White Background.png" alt="White background" width="350"/>
    <img src="images/Noisy Background.png" alt="Noisy background" width="350"/>
</p>

To access all the provided images, please visit: https://drive.google.com/file/d/1Q2mwA8k36r5RWhSbPjQ6dWrndn1jgpEH/view?usp=sharing

# Method

## Calibrating camera

The first step would be to perform camera calibration as the images are distorted. For this purpose, OpenCV (https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html) was used, utilizing images of a chessboard taken from various angles and perspectives. This allows obtaining the camera's intrinsic parameters and lens distortion coefficients. Finally, with these parameters, it is possible to undistort the image, calculate the error of this approximation, and effectively segment them.

## Obtaining conversion ratio between pixel to millimeter

In this step, a method was employed to obtain the size of the chessboard square in pixels by taking the average of all the squares on the chessboard and considering the Euclidean distance as the distance metric. Finally, the conversion ratio between pixels and millimeters was obtained.

## Define ROI

Initially, I manually defined the ROI (Region of Interest) only for the white background, as described in the project documentation, since it was initially almost static. However, later on, I automated the method to make it more general. In this way, I first used segmentation using the HSV color space to define only the area with a white background and remove other areas. Then, I applied the morphological operation of opening to improve the image quality. After that, I created a function to obtain the vertices of the quadrilateral obtained using approximations and subsequently performed a crop to extract only the region of the background.

## Segmentation

Indeed, this was the most complex step, in which I made several different attempts. Basically, both backgrounds employ the same idea, differing only in one aspect that I will discuss later.

Basically, we first convert the image from BGR to HSV and find suitable thresholds to segment each color. Then, we apply different filters for each color type using the closing morphological operation to fill holes in the image and obtain the requested metrics. For red color I need to use a different aproch to get a better result, where I created a mask to find all peanuts and after use another mask to remove the other colors.

For the noisy background, the only difference is that we first convert the image to the CMY color space and then apply a blur filter to smooth the image.

<p float="middle">
    <img src="images/White Background - Segmented.png" alt="White background segmented" width="350"/>
    <img src="images/Noisy Background - Segmented.png" alt="Noisy background segmented" width="350"/>
</p>


## Limitations

How we can see for white background the color segmentation using HSV got a good result for the majority colors. But, if was needed color segmentation for similar colors we had problem to find a good limiar of decision. Moreover, for other condictions of lighting or background color we would have more difficulty to segmentation.

And for noisy background one limitation is for some colors, how brown, the area is very imprecise duo the background color. Moreover, how can see in calib_img 5.png two M&M near are considered only one. Indeed, it is a more complex task to work with a background that does not follow a well-defined pattern.

## Conclusion

It is noticeable that for a well-behaved background, the segmentation occurs as expected, but for another background with more disorder, this does not happen, and new challenges arise.

For better results, I believe that it would be necessary to employ more robust techniques that go beyond the traditional segmentation approach.

Finally, despite the aforementioned problems, I achieved the highest grade in the class for this project, demonstrating that segmenting objects by color using traditional computer vision techniques is indeed not a simple task.

## Note

The .ipynb file is too large to be displayed on GitHub. If you would like a quick preview of the file without downloading it, please access: https://colab.research.google.com/drive/1r0lxHrhD9yri6ytPHyBZ4gCZW-S1BSA8?usp=sharing
