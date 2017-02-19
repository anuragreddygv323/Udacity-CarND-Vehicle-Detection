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
  
  iii) Accuracy Percentage - ** 99.53% **
