# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[gray]: ./images4readme/gray.png "Grayscale"
[edges]: ./images4readme/edges.png "Edges"
[masked]: ./images4readme/masked.png "Masked"
[lines]: ./images4readme/lines.png "Lines"
[lanes]: ./images4readme/lanes.png "Lanes"

---
### 1. Pipeline Description
There were  several transformations that had to be done on the original image to eventually find lane lines. OpenCV was used within Python, as OpenCV contains many high level and powerful image analysis functions. 
The steps to move from an image to drawing lane lines are as follows:
#### Convert to Grayscale and sharpen the image
```
gray = grayscale(image)
blur_gray=gaussian_blur(gray,kernel_size)
```
![alt text][gray]
#### Detect edges in the image. Edges are found by finding the derivative in x and y using the canny() function
```
edges = canny(blur_gray, low_threshold, high_threshold)
```
![alt text][edges]
#### Identify the region of interest for lanes in the image
Lane lines should only be found in the triangular region in front of the car. To get rid of some noise, I chose to crop the region of interest to be adjusted with some offset.
```
x_offset=50
y_offset=75
vertices = np.array([[(0,imshape[0]),(imshape[1]/2-x_offset, imshape[0]/2+y_offset), (imshape[1]/2+x_offset, imshape[0]/2+y_offset), (imshape[1],imshape[0])]], dtype=np.int32)
image_masked=region_of_interest(edges,vertices)
```
![alt text][masked]
#### Ddentify the lines from edges. Lines are found by intersections in the Hough Transform
```
image_lines = hough_lines(image_masked, rho, theta, threshold, min_line_len, max_line_gap)
```
![alt text][lines]
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
![alt text][lanes]


### 2. Identify potential shortcomings with your current pipeline
One shortcoming is the small sample size of videos to process. Videos from different freeways, different time of days, and weather are all more data to process and tune the lane finding algorithm.


### 3. Suggest possible improvements to your pipeline
An improvement to add use the average, from all the frames, of the mean and y-intercept of each frame. Given the camera being in a fixed location, the lane lines look like they will be in generally the same x,y locations and contain the same slope.
Another improvement is to fit a spline or high order polynomial curve to the lanes.



