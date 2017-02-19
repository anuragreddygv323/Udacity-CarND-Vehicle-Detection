# Udacity-CarND-Vehicle-Detection

# About

This project is part of Udacity's Self-Driven Car Nanodegree Program. The aim is to develop a pipeline which tracks the cars detected along the lanes.

# Rubric Points

## Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images. Explain how you settled on your final choice of HOG parameters.

  i) ***hog()*** function from ***skimage.feature*** is used for extracting hog features.
  
  ii) hog features are extracted individually for each of the color channels.

  iii) ***get_hog_features()*** function in **cell no. - 8 of vehicle_detection.ipynb** helps us to get the hog features of an image. Features extracted from each color channels are appended and  ***np.ravel()*** is used to return 1-D array of features of all the color channels.
  
  
   Original Image          |  HOG of Color Channel R               
:-------------------------:|:-------------------------:
![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/original.jpg)  |  ![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/hog_channel_r.jpg) 

HOG of Color Channel G     |  HOG of Color Channel B               
:-------------------------:|:-------------------------:
![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/hog_channel_g.jpg)  |  ![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/hog_channel_b.jpg) 


#### 2. Explain how you settled on your final choice of HOG parameters.

After several round of trial and error, the following are the parameters chosen for HOG, Spatial Bins and Color Histograms:-
      
      a) Color Channel - RGB
      b) Orientation bins - 8
      c) Pixels per cell - 8
      d) Cells per block - 2
      e) Histogram Bins - 48
      f) Histogram Bins Range - (0, 256)
      g) Spatial Image Size - (16, 16)
      
  These parameters are present in **cell no. - 5 of vehicle_detection.ipynb**.


#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

  i) I have trained my classifier using **SVC** function **sklearn.svm**. SVC was chosen instead of LinearSVC because it resulted in increased accuracy. The increase in accuracy was because of the probabilistic output of SVC.
  
  ii) The implementation is present in **cell no. - 15, 16 of vehicle_detection.ipynb**.
  
  iii) Accuracy Percentage - **99.53%**


## Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search. How did you decide what scales to search and how much to overlap windows?

  i) There are some helper function for sliding window search present in **cell no. - 17, 18, 20, 21 of vehicle_detection.ipynb**.
  
  ii) The ***image_pipeline()*** function present in **cell no. - 22 of vehicle_detection.ipynb** contains the whole implementation.
  
  iii) Coming up with parameters involved extensive trail and error. Also, there is no fixed single answer. Different configurations can be used to build the pipeline.
  
  iv) Three different window sizes were selected. 
      
  For small windows :-
     
          a) Window Size - (64, 64)
          b) X Start Stop - (640, 1280)
          c) Y Start Stop - (300, 450)
          d) Overlap - (0.6, 0.6)
          
  For medium windows :-
     
          a) Window Size - (96, 96)
          b) X Start Stop - (640, 1280)
          c) Y Start Stop - (300, 500)
          d) Overlap - (0.8, 0.8)
          
  For large windows :-
     
          a) Window Size - (128, 128)
          b) X Start Stop - (640, 1280)
          c) Y Start Stop - (400, 550)
          d) Overlap - (0.85, 0.85)
          
  v) All the windows obtained from above step are then passed to ***search_windows()*** present in **cell no. - 18 of vehicle_detection.ipynb** to get the actual hot boxes using prediction from SVC classifier. The classifier is used to predict the probability of a car being present in the image. If probability is greater than 0.5 then, car is present in the image.
  
  vi) Then a heatmap is created using the hot boxes from above step. ***add_heat()*** function present in **cell no. - 20 of vehicle_detection.ipynb** is used. +1 value is added to all the boxes obtained from above step. After that ***apply_threshold()*** function present in **cell no. - 20 of vehicle_detection.ipynb** is called to remove some of the false positives present.
  
  vii) ***label()*** function from ***scipy.ndimage.measurements*** helps in labelling the thresolding regions from the above step. After that ***draw_labeled_bboxes()*** function present in **cell no. - 21 of vehicle_detection.ipynb** is called to draw bouding box around the detected car.
          
  
#### 2. Show some examples of test images to demonstrate how your pipeline is working. What did you do to try to minimize false positives and reliably detect cars?

Image Pipeline for test image :-

   Original Image          |  Sliding Windows               
:-------------------------:|:-------------------------:
![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/original.jpg)  |  ![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/sliding_windows.jpg) 

Heatmaps                   |  Labeled Boxes               
:-------------------------:|:-------------------------:
![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/heatmap.jpg)  |  ![](https://github.com/imindrajit/Udacity-CarND-Vehicle-Detection/blob/master/output_images/test1/final_box.jpg)

To remove False positives :-

i) In ***draw_labeled_bboxes()*** function in **cell no. - 21 of vehicle_detection.ipynb**, I check whether the minimum y-coordinate values is in between 300 and 600. If not, then don't draw the box. Also, minimum x-coordinate value should be less than 1220. One more check of the area of the box has beeen used to remove small False positve boxes. If the area of the box is less than 2500, then don't draw it.

ii) Use of SVC with probabilitic prediction instead of normal LinearSVC also helped in removing many False positives. 

iii) As mentioned in the previous question, heatmaps and thresholding on those heatmaps also help us in removing False positives.


## Video Implementation

#### 1. Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

[Video Link](https://www.youtube.com/watch?v=T5zU5MmVTu4)

project_video_result.mp4 present in the github repository is the same video as that of youtube.


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

Method used for removing False positive has been answered in 2nd question of **Sliding Window Search** heading.

Bounding boxes obtained in image pipeline in each frame were flickering a lot. So, to reduce the flickering **Exponential Smoothening** has been used. It's implementation is present in ** cell no. - 22 of vehicle_detection.ipynb** inside ***image_pipeline()*** function. Heatmaps obtained from previous frame is given weightage of **0.2** and heatmaps from current frame are given weightage of **0.8**. These two values were obtained experimentally. 


## Discussion

#### Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

i) The pipeline used in the current project is very specific to the project video. All the different parameters have been tuned keeping that fact in mind. There is a need for generlistic approach.

ii) Removing False positives was the biggest challenge for me. There are still some False positives in the resultant video. the pipeline needs to be improved further.

iii) Weighted moving average filter can be used to further reduce flickering in the video.

iv) Also, Deep Learning approach could result in a more general solution. 
