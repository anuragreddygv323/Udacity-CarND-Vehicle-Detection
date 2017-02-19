# Udacity-CarND-Vehicle-Detection

# About

This project is part of Udacity's Self-Driven Car Nanodegree Program. The aim is to develop a pipeline which tracks the cars detected along the lanes.

# Rubric Points

## Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images. Explain how you settled on your final choice of HOG parameters.

  i) ***hog()*** function from ***skimage.feature*** is used for extracting hog features.
  
  ii) hog features are extracted individually for each of the color channels.

  iii) ***get_hog_features()*** function in **cell no. - 8 of vehicle_detection.ipynb** helps us to get the hog features of an image. Features extracted from each color channels are appended and  ***np.ravel()*** is used to return 1-D array of features of all the color channels.
  
  iv) After several round of trial and error, the following are the parameters chosen for HOG, Spatial Bins and Color Histograms:-
      
      **a) Color Channel - RGB**
      **b) Orientation bins - 8**  
      **c) Pixels per cell - 8**
      **d) Cells per block - 2**
      **e) Histogram Bins - 48**
      **f) Histogram Bins Range - (0, 256)**
      **g) Spatial Image Size - (16, 16)
      
     These parameters are present in **cell no. - 5 of vehicle_detection.ipynb**.
      
