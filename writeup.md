#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[gray]: ./gray.jpg "Grayscale"
[edges]: ./edges.jpg "Grayscale"
[line_image]: ./line_image.jpg
[result]: ./result.jpg

---

### Reflection

My pipeline consist of following steps.

####1. Convert the image to grayscale.

![alt text][gray]

####2. Apply gaussian blur to eliminate noise.

####3. Apply canny edge detector to extract edges for lane line candidate.

![alt text][edges]

####4. Apply hough transformation to extract lane line only from region of interest.

![alt text][line_image]

####5. Convert the line segments expression from cartesian to polar coordinate.
Here I relocate the polar center to bottom center of the image (where the car frontend should be), because esppecially the right lane line's theta parameter is varies quickly in the original polar center (top left corner).

####6. Apply k-means++ to clusterize line segments to 2 cluster. (Left lane and Right lane)
The line segments are clusterized in the polar space. Lane length was considered as weight to calculate cluster centroid.

####7. Extract the longest line in each cluster to create lines to be drawn.
I originally tried to create line from the cluster centroid but was highly sensitive to the outliers, so I just pick up the longest line in each cluster.

####8. Reconvert the lines from polar to cartesian coordiate.

####9. Apply helper function to draw lane lines on original image.

![alt text][result]


###2. Identify potential shortcomings with your current pipeline

One shortcomming is that the method of clusterization (k-means++) is very senstive to outliers. Often the hough line detects the horizontal segment of the lane line (outliers) from the image, and that really corrupted the cluster centroid.

Another shortcoming is that this method assumes that there is only two lane line for clusterization in the region of interest. If we were to execute lane change, we might have to detect more lane lines.


###3. Suggest possible improvements to your pipeline

The implementation of rejection of such outliers should be done in the future work, which is realted to the first shortcomming described earlier.

Another potential improvement could be to make is possible to detect optimum number of cluster before executing k-means++.
