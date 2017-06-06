
**Vehicle Detection Project**

[//]: # (Image References)
[image1]: ./output/car_not_car.png
[image2]: ./output/HOG_example.png
[image3]: ./output/sliding_windows.png
[image4]: ./output/sliding_window.png
[image5]: ./output/bboxes_and_heat.png
[image6]: ./output/labels_map.png
[image7]: ./output/output_bboxes.png
[video1]: ./output/project_video.mp4


####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook from line 15 to 26,I have called the sklearn's hog function with specific arugments to extract HOG features from YCrCb formate of image

I started by reading in all the `vehicle` and `non-vehicle` images. I found that the `vehicle` class had 1074 lesser images than that of `non-vehicle` class, I randomly reused 1074 of images in `vehicle` class ( cell 1 , line 10), to make sure that both the classes had same number of images. Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and various formate of color spaces and used one the best output.

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I have first normalized the features by using sklearn's StandardScaler() function (line 13 of cell 4) , later I have splited the features into traing and test set for validation using train_test_split() function, finally I have trained the train features by using Linear SVM of sklearn. We can see that accuarcy of trained model is 99.10% 

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

Sliding Window is implemented in cell 1 line 109. I have deterimind window size based on the number of 'Pixels for cell' and number of 'Cells for block'.As suggested in lecuture, I have calcuated the HOG feature of entie image for efficiance and estimated spatial and histogram features of each sub window induvidually.The combined feature is later used as an input for SVC's fit function to determine if it has a car. 

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output/project_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Also , I have restricted the search only to lower right part of images , since we are looking for cars only on that side in these example vedio.
![alt text][image3]
Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The challenging part was to come up with the right combination of fetures, I have tried using different color spaces and also tried different counts of bins for histograms.

The pipeline still detects false positivies and that can be observer in vedio. We can avoid these by using still more images for trainging.Provied we have high frame rate, we could efficiently determine the false positivies by checking if the same bouding box is found in both previous and next frame . 
