# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---
### 1. Pipeline Description
There were  several transformations that had to be done on the original image to eventually find lane lines. OpenCV was used within Python, as OpenCV contains many high level and powerful image analysis functions. 
![alt text][./images4readme/image1]
The steps to move from an image to drawing lane lines are as follows:
#### Convert to Grayscale and sharpen the image
```
gray = grayscale(image)
blur_gray=gaussian_blur(gray,kernel_size)
```
![alt text][./images4readme/image2]
#### Detect edges in the image. Edges are found by finding the derivative in x and y using the canny() function
```
edges = canny(blur_gray, low_threshold, high_threshold)
```
![alt text][./images4readme/image3]
#### Identify the region of interest for lanes in the image
Lane lines should only be found in the triangular region in front of the car. To get rid of some noise, I chose to crop the region of interest to be adjusted with some offset.
```
x_offset=50
y_offset=75
vertices = np.array([[(0,imshape[0]),(imshape[1]/2-x_offset, imshape[0]/2+y_offset), (imshape[1]/2+x_offset, imshape[0]/2+y_offset), (imshape[1],imshape[0])]], dtype=np.int32)
image_masked=region_of_interest(edges,vertices)
```
![alt text][./images4readme/image4]
#### Ddentify the lines from edges. Lines are found by intersections in the Hough Transform
```
image_lines = hough_lines(image_masked, rho, theta, threshold, min_line_len, max_line_gap)
```

#### Detect lane lines.
At this point, lines are found in the region of interest. To find lane lines, `draw_lines2()` was created in an ad-hoc manner to find left lane lines and right lane lines. 
An overview of finding lane lines on a single image is briefly described below:
##### a. seperate the left and right lane lines by slope.
```
for line in lines:
    for x1,y1,x2,y2 in line:
        m = (y2-y1)/(x2-x1)
        b = y1-m*x1      
        if m < -0.5 and m > -1: #left lane
            m_lefts.append(m)
            b_lefts.append(b)
        if m > 0.5 and m < 1: #Right Lane
            m_rights.append(m)
            b_rights.append(b)
```
##### b. find the average slope and y-intercept for the left lane lines and right lane lines.
```
m_right = np.mean(m_rights)
b_right = np.mean(b_rights)
m_left = np.mean(m_lefts)
b_left = np.mean(b_lefts)
```
##### c. fit a line to slope and y-intercept. The lane lines should always be starting from the bottom of the image (top of the image as a matrix) and should extent to the midpoint minus some offset.
```
right_line_x1 = int((image.shape[0]-b_right)/m_right)
right_line_y1 = image.shape[0]
right_line_x2 = int((image.shape[0]/2+y_offset-b_right)/m_right)
right_line_y2 = int(image.shape[0]/2+y_offset)
left_line_x1 = int((image.shape[0]-b_left)/m_left)
left_line_y1 = image.shape[0]
left_line_x2 = int((image.shape[0]/2+y_offset-b_left)/m_left)
left_line_y2 = int(image.shape[0]/2+y_offset)
```




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
