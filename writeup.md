#**Traffic Sign Recognition** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

###Data Set Summary & Exploration

####1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32,32,3)
* The number of unique classes/labels in the data set is 43

####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...

![Histogram of training data](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/training_data_histogram.png)

![Histogram of all datasets](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/historgram-all-datasets.png)

The class lables (signs) are not uniformly distributed. This may lead to bias in learning from the over sampled signs.

We can also visualize full list of signs.

![All signs](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/class_images.png)


###Design and Test a Model Architecture

####1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I converted the images to grayscale by converting from RGB to YUV and only using the Y channel according to [Traffic Sign Recognition with Multi-Scale Convolutional Networks](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf) using grayscale images improves the performance of the network.

Then I normalized the image data (-0.5, 0.5) while improving the dynamic range of the image by spreading the histogram. 

![transformed data](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/transformed_data_sample_image.png)

Finally, I decided to generate additional data because as shown in the visualization the distribution of examples are not uniformly distributed. To add more data to the the data set, I used translate ([-2,2] pixels), scale ([.9,1.1] ratio) and rotation ([-15,+15] degrees) as suggested by [Traffic Sign Recognition with Multi-Scale Convolutional Networks](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf). 


![Historgram of all datasets after data augmentation](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/historgram-all-datasets-augmented.png)


####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

Another convolutional layer was added to the original LeNet architecture to grow the architecture deeper. My final model (this model was implemented by [Dmitry Kudinov](https://github.com/dmitrykudinov) instead of only using the Y-Channel of YUV-image he used the full RGB-image) consisted of the following layers:


The architecture of my model:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 Y-Channel of YUV-image   				| 
| Convolution 5x5      	| 1x1 stride, valid padding, outputs 28x28x9   	|
| RELU			        |	                               				|
| Convolution 10x10    	| 1x1 stride, valid padding, outputs 19x19x27  	|
| RELU			    	|	                                            |
| Convolution 10x10    	| 1x1 stride, valid padding, outputs 10x10x32  	|
| RELU                  |                                               |
| Max pooling	        | 2x2 stride,  outputs 5x5x32 (800)        		|
| Fully connected	    | 800 -> 120                           	    	|
| RELU			       	|	                                            |
| Fully connected	    | 120 -> 84                            			|
| RELU			    	|	                                            |
| Fully connected	    | 84 -> 43                             			|
| Softmax           	|	           									|


####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an Adamoptimizer with 100 epochs and batch size of 256 images.

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.984
* validation set accuracy of 0.984
* test set accuracy of 0.966

If an iterative approach was chosen:

* What was the first architecture that was tried and why was it chosen?

I started by using the normal LeNet5 architecture as discussed in lesson 8. Since it did not give me the desired validation-accuracy I implemented other similar architecture like at [Traffic Sign Recognition with Multi-Scale Convolutional Networks](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf) but setteled for the architecture described above by [Dmitry Kudinov](https://github.com/dmitrykudinov).

* What were some problems with the initial architecture?

Even after data augmentation and experimenting with more preprocessing the test-accuracy did not improve.

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.

Another convolutional layer was added to the original LeNet architecture to grow the architecture deeper.

* Which parameters were tuned? How were they adjusted and why?

The learning rate was dropped to 0.001, µ stayed at 0 and σ at 0.1.

* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

Convolution layer can improve image recognition as not only a single value is taken into consideration but we took the neighborhood values into our analysis. Using dropout layer is a way to prevent overfitting in NN.
  
  

###Test a Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are  German traffic signs that I found on the web:
![German traffic signs](https://github.com/thogiti/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup/germansignswebimages.png)


All 9 images are correctly classified.

####2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        					|     Prediction	        					| 
|:-----------------------------------------:|:---------------------------------------------:| 
| Priority road      						| Priority road   								| 
| Turn right ahead     						| Turn right ahead 								|
| Yield										| Yield											|
| No entry	      							| No entry						 				|
| 70 km/h	      							| 70 km/h						 				|
| 30 km/h	      							| 30 km/h						 				|
| Children crossing	   						| Children crossing								|
| Ahead only								| Ahead only     								|
| Vehicles over 3.5 metric tons prohibited	| Vehicles over 3.5 metric tons prohibited     	|

The model was able to correctly guess 9 out of 9 traffic signs, which gives an accuracy of 100%. This is a bit similar to the accuracy on the test dataset of  0.966. 


####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

For 9 out of 9 images, the model is correctly predicting the sign label.


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
####1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

It can be observed that the feature maps of the first convolution layer encode the shape of the traffic signs and the edges of the image details, in this example the edges around the letters.

The feature maps of the second convolution layer encode more complex information and other minute details present in the traffic sign. It is difficult to comprehend what exactly the convolution filters are capturing at this layer.



