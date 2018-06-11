# **Vehicle Detection and Tracking**

The goals / steps of this project are the following:
* To make a pipeline that detects vehicles on the road

### Extrating Image Features

The key point to recognize objects on image is toextract usefull properties and compare them with known patterns. One of the most sucessful is [Histogram of Oriented Gradients](https://en.wikipedia.org/wiki/Histogram_of_oriented_gradients). This feature extractor tries to capture the line orientation of a given image, and then it is possible to compare it against car images. 

As an example, for the image:

![Car in Grayscale](test_images/i1.png)

The HOG for some chosen parameters is:

![HOG](test_images/i2.png)

In this project, We chose a very simple image feature extractor. First, convert the image to grayscale and compute HOG, then use an SVM Model to predict if HOG descriptor is from a Vehicle or not. Following parameters was chosen:

- image size: 64 x 64 pixels;
- orientation: 9;
- pixels per cell: 16 x 16;
- cell per block: 2

### Dataset

We have trained a SVM Model to detect Vehicles with dataset provided by Udacity. This dataset contains 18.000 (~) small color images classified as Vehicle or not. Each image is 64 x 64 pixels as:

![Car 1](test_images/c1.png)

![Car 2](test_images/c2.png)

![Car 3](test_images/c3.png)

And there are also images with no vehicles too:

![Not Car 1](test_images/nc1.png)

![Not Car 2](test_images/nc2.png)

### Model

After some tests, we chose as classifier an SVM Model with following parameters:

- Kernel: RBF;
- C: 0.200;
- gamma: 0.01.

Accuracy in train dataset is 0.98 and in test dataset is 0.96. Because a very simple image extractor and a model with small number of parameters were chosen, there is almost no overfit. 

A pre-trained model that could save us time is on file **model.pkl**. To force program to retrain model, it is necessary to remove this file previously.

### Model Performance

The model has performed well in the dataset. But this says nothing about its performance on images taken from another camera and conditions. As a sanity check, we extracted patches from known cars to verify if the model gives the corrected prediction:

![Original Image](test_images/test1.jpg)

![White Car](test_images/white_car.png)

Prediction: VEHICLE

![White Car](test_images/black_car.png)

Prediction: VEHICLE

![Far](test_images/far_car.png)

Prediction: NOT a VEHICLE

As We can see, the model cannot detect a car if it is far away (and image is small).

### Detection on Full Images

Our model is able to predict if a small image (64 x 64 pixels) is a vehicle, but real images are larger. To detect cars in full images, we use function **slide_window** that helps to crop original image in many small patchs and predict if each small patch is a vehicle. Here is an example:

![Window](test_images/slide_window.png)

The trained model does not perform  well on non-vehicles images and detect them as cars and, as result, the program obtains a lot of false positives. To avoid them, there are functions such as *add_heat*, *apply_threshold* and *draw_labeled_bboxes*. the ideia is to report as vehicles only the regions that are classified as vehicles in many windows. 

![Heat Map](test_images/heat_map_2.png)

Now, to report vehicles in a movie we use an additional method that stored detected vehicles in previous frames to compute an *average* and calculate the heat map.

### Pipeline

The pipeline to detect vehicles in movies is:

- initialize the history of detected windows with empty list;
- iterate to window size **L** with values: 70, 100, 130, 160, 190 and 220;
- for each window size, create a list of windows to search for cars;
- crop window from original image and resize it to 64x64 pixels;
- convert cropped image to grayscale;
- extract HOG features from image;
- determine if the image is a vehicle or not by using the SVM Model;
- update lists of detected windows and history;
- calculate the heat map;
- create result image as original frame plus the heat map.

We can see the pipeline being applied in the folling video:

[project_video.mp4](output/project_video.mp4).

### Shortcomings

I believe that the main shortcoming it is related to detecting small objects. The ideia of resize every image to 64 x 64 pixels results in poor image quality (resolution) for distant objects and compromises the classifier performance. 

Other shortcoming is the cost of running the feature extraction over and over. To reduce this cost and avoid overfitting, We have choosen a very simple extractor. But the idea of manually sellection the good features is not productive. 

We believe that using CNN (Convolutional Neural Network) could help us in these shortcomings in order to get a more robust solution, but we would need a larger dataset. 


