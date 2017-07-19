# **Finding Lane Lines on the Road** 

### 1. Pipeline description
To process the images the function __processing_image__ was implemented.

The pipeline consisted of following steps:
1. Evaluating image size in pixels
2. Grayscaling the image
3. Calling _canny_ function to determine edges. Threshold parameters are between 100 and 200
4. Defining a masked area like a triangle:
	* Left bottom point is approximate position of closest left lane marking line
	* Apex point is approximate position of intersection virtual left and right lane marking lines in perspective
	* Right bottom point is approximate position of closest right lane marking line
5. Defining the Hough transform parameters and applying them
	* First, define a coefficient between pixels and world units (centimeters) to process all other parameters in centimeters
	* Define minimum line length as 5 cm and maximum line gap as 1 cm
6. Drawing the lines on the source image

In order to draw a single line on the left and right lanes, I modified __hough_lines__ and call it __hough_merged_lines__.
For splitting all lines set into two subsets (positive oriented and negative oriented) I have implemented function __split_slope_lines__.
In this function:
1. For all lines is evaluated the slope
2. All lines are grouped into subset (positive or negative) if the slope is in configured threshold (by default 10 degrees)
3. For the subset is evaluated the minimal and maximal point from all subset lines
4. The minimal or maximal point (depended from orientation) is moved to the bottom border of image (line is prolongated)
5. The minimal and maximal points are return as candidate points for a aggregated line

After calling this function twice for positive and negative oriented lines subsets two aggregated lines are drawing using _draw_lines_ function with 10 cm tickness.

[//]: # (Image References)

[image1]: ./test_images/solidYellowCurve.jpg "Input"
[image2]: ./test_images_out/solidYellowCurve_out.jpg "Output"
[image3]: ./test_images_out/solidYellowCurve_out_merge.jpg "Output Merged"

### 2. Potential shortcomings

Potential shortcoming would be what would happen when not only two lines subsets are observed (line marking of another lanes, merging, splitting of line markings, etc.)

### 3. Suggest possible improvements to pipeline

A possible improvement would be to divide the lines set more strickly using not only slope.

Another potential improvement could be to dynamically define "visible envelope" of pictures or videos - exactly, what happen during processing _challenge_ video clip, when the
car cowl is visible in video and erroneously detected also as line markings.
