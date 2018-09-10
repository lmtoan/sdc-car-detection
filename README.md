# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

**Refer to `Vehicle-Detection-Pipeline-TL.ipynb` for code and full write-up.**

The report describes in detail the following steps and provides a final reflection for future development.

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Run the pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Draw and estimate a bounding box for vehicles detected.

Data
---

A pickled file is provided [here](https://www.dropbox.com/s/rfl7h55k6bb26xr/car_dataset.p?dl=0)

Here are links to the labeled data for [vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) and [non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) examples to train your classifier.  These example images come from a combination of the [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html), the [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/), and examples extracted from the project video itself.   You are welcome and encouraged to take advantage of the recently released [Udacity labeled dataset](https://github.com/udacity/self-driving-car/tree/master/annotations) to augment your training data.  

[Reflection](Vehicle-Detection-Pipeline-TL.ipynb)
---
The output for test video is [here](test_output_video.mp4) and the output for project video is [here](project_output_video.mp4)

A few problems and suggested solutions can be in the following areas:

1) Feature selection and extraction
* For this exercise, I only stack HOG features for all 3-channel RGB colorspace for the feature matrix X. Although HOG is a great abstractor of an image, its features might not be complex enough to detect cars in an image frame. An ensemble solution can be done by combining HOG, color histogram, SIFT features, raw pixels.
* For HOG, the hyperparameters search might not be comprehensive and lead to to sub-optimal performance.

2) Classifier
* Typically, `LinearSVC()` is a robust method for simple primitive features and might have yielded 98%ish test accuracy. But it is extremely easy to overfit. For a high-stake situation like vehicle detection in self-driving car, a linear model might not be a good candidate to generalize high-pixel image frames well. Neural-based methods or non-linear classifiers should be used to account for the complexity of image features.
* More sample is always needed. In this exercise, we only have low-definition crops and less than 10,000 examples for cars and non-cars categories. A reliable system might require close to half a million images.

3) Window search and accuracy
* For this exercise, I only set the window search for the middle section of an image frame. To achieve a reliable performance, a window search might be performed on a larger portion of the image frame, which will result in speed compromise for the system. A few real-time object detection algorithms such as [YOLO](https://pjreddie.com/darknet/yolo/) might be a good candidate to avoid brute force a large number of unnecessary windows.
* As shown in the project output video, the first few minutes, there are a few false positives which might be catastrophic for a self-driving car. This can be avoided by training a better classifier, or incorporating a more sophisticated history cache to account for cars on the same journey.