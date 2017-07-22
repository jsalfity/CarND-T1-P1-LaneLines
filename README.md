# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Pipeline Description
There were  several transformations that had to be done on the original image to eventually find lane lines. OpenCV was used within Python, as OpenCV contains many high level and powerful image analysis functions. 

The steps to move from an image to drawing lane lines are as follows:
#### Convert to Grayscale and sharpen the image
```gray = grayscale(image)
blur_gray=gaussian_blur(gray,kernel_size)```

#### Detect edges in the image. Edges are found by finding the derivative in x and y.
```edges = canny(blur_gray, low_threshold, high_threshold)```

#### Identify the region of interest for lanes in the image
Lane lines should only be found in the triangular region in front of the car. To get rid of some noise, I chose to crop the region of interest to be adjusted with some offset.
```x_offset=50
y_offset=75
vertices = np.array([[(0,imshape[0]),(imshape[1]/2-x_offset, imshape[0]/2+y_offset), (imshape[1]/2+x_offset, imshape[0]/2+y_offset), (imshape[1],imshape[0])]], dtype=np.int32)
image_masked=region_of_interest(edges,vertices)```


#### Identify the region of interest in the image
Lane lines should only be found in the triangular region in front of the car. To get rid of some noise, I chose to crop the region of interest to be adjusted with some offset.


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
