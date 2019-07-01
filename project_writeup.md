# **Traffic Sign Recognition** 

## Writeup

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./images/dataset_repartition.png "Data repartition"
[image2]: ./images/grayscale_normalization.png "Grayscaling&Normalization"
[image4]: ./images/0.png "Traffic Sign 1"
[image5]: ./images/17.png "Traffic Sign 2"
[image6]: ./images/38.png "Traffic Sign 3"
[image7]: ./images/39.png "Traffic Sign 4"
[image8]: ./images/40.png "Traffic Sign 5"

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/WbHappy/traffic-sign-classifier-project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic
signs data set in 2nd cell of the Ipython notebook:

* The size of training set is 34799.
* The size of the validation set is 4410.
* The size of test set is 12630.
* The shape of a traffic sign image is (32, 32, 3).
* The number of unique classes/labels in the data set is 43.

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data is distributed. There are as many bars as there are classes.
![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

After gooing through the LeCun and Semanet [paper](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf) (Traffic Sign Recognition with Multi-Scale Convolutional Networks) paper Grayscaling and normalization is clearly advised to help the classification and training time. Of course this was also presented during the training class preceding this project.


Here is an example of a traffic sign image after grayscaling. and normalization :

![alt text][image2]


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, Valid padding, outputs 28x28x6 	|
| RELU					| No change on shape							|
| Max pooling	      	| 2x2 stride, Valid padding, outputs 14x14x6 	|
| Convolution 5x5	    | 1x1 stride, Valid padding, outputs 10x10x16	|
| RELU					| No change on shape							|
| Max pooling	      	| 2x2 stride, Valid padding, outputs 5x5x16 	|
| Flatten(1)			| Output : 400									|
| Convolution 5x5	    | 1x1 stride, Valid padding, outputs 1x1x400	|
| RELU					| No change on shape							|
| Flatten(2)			| Output : 400									|
| Concat(1+2)			| Output : 800									|
| Dropout				| No change on shape							|
| Fully connected		| Input : 800 Output : 400						|
| RELU					| No change on shape							|
| Fully connected		| Input : 400 Output : 120						|
| RELU					| No change on shape							|
| Fully connected		| Input : 120 Output : 43						|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I did not use any GPU, only my CPU and.. some patience !
For the next project I will most likely make use of my 50h GPU workspace to do more testing.

* Learning rate : 0.0009 (tested with a lot of different values).
* Epochs : 60
* Batch_size : 128

I had to play around mostly with the learning rate and Epochs to get a good score on the training set and validation set. The test set accuracy was then good enough to validate the project.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:

* training set accuracy of 100% (1.0)
* validation set accuracy of 95.2% (0.952)
* test set accuracy of 93.8%(0.938)

I started from the original Lenet implementation made during a previous lesson. It was obviously choosen since it is a good starting point as multiple layers were implemented but also because it was doing greate with lines and curves detection which are obviously the main geometric shapes in traffic signs.

I tested some *division* of one of my layer (one was directly flattened while aplying a convolution on the other) right before concatenating them and then to reduce  overfitting I used dropout which had a really good impact on the validation set accuracy. 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] 
![alt text][image7] ![alt text][image8]

All images I took had the correct input size (32x32). They seem to be easily classifiable, the only visible challenge on those images is the variation of light.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Keep right      		| Keep right   									| 
| Speed limit (20 km/h) | Speed limit (20 km/h)							|
| Keep left				| Keep left										|
| No entry	      		| No entry					 					|
| Roundabout mandatory	| Roundabout mandatory							|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares more favorably to the accuracy on the test set of 93.8%. These new traffic signs were fairly easily detected. I am rather sure that taking some traffic signs from Australia or any other country that has some slight changes will reduce the accuracy.
The training set, and therefore the model, can greatly be improved using different traffic signs from all over the world.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 15th cell of the Ipython notebook.

For three out of five images, the model is 100% sure of his detection. Good news he is right. The top five soft max probabilities for the five signs are : 

First image, Keep right :

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Keep right 									| 
| .0     				| Road work 									|
| .0					| Wild animals crossing							|
| .0	      			| General caution					 			|
| .0				    | Pedestrians      								|


For the second image, Speed limit (20 km/h):

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Speed limit (20 km/h)							| 
| .0     				| Children crossing								|
| .0					| Righ-of-way at the next intersection			|
| .0	      			| Slippery Road 					 			|
| .0				    | Dangerous curve to the left					|

For the third image, Keep left:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Keep left										| 
| .0     				| Speed limit (50 km/h)							|
| .0					| Double curve									|
| .0	      			| Speed limit (100km/h)				 			|
| .0				    | Yield											|

For the fourth image, No entry:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.99674      			| No entry										| 
| .00326   				| Stop											|
| .0					| Turn right ahead								|
| .0	      			| Yield								 			|
| .0				    | Go straight or left							|

For the fifth image, roudabout mandatory:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.99674      			| Roudabout mandatory							| 
| .00326   				| Speed limit (100km/h)							|
| .0					| Vehicles over 3.5 metriic tons prohibited		|
| .0	      			| Righ-of-way at the next intersection			|
| .0				    | Priority road 								|