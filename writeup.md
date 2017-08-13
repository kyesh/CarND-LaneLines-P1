# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image0]: ./test_images/solidWhiteCurve.jpg
[image1]: ./test_images_output/ROI_solidWhiteCurve.jpg
[image2]: ./test_images_output/filterRoadColor_solidWhiteCurve.jpg
[image3]: ./test_images_output/lanelinessolidWhiteCurve.jpg


---

## Reflection

### 1. Pipeline Description

My pipeline consisted of 4 steps.

First I set all points outside the region of intrest to black. Second I tried to emulate what sebation metioned in the the DARPA grand challenge video. I tried to determine what color the road was and filter out all pixels in that range. Third, I fed the remaining pixels which I surmised should only include lanes into the the OpenCV HoughLinesP function. I then consolidate all the lines output by the HoughLinesP into two lines.

#### Orginal Image:

![alt text][image0]

#### Step1 Example:

![alt text][image1]

#### Step2 Example:

![alt text][image2]

#### Step3 Example:

![alt text][image3]

#### Step3 Details:

So I think I went a bit down the rabbit hole for this part. I thought it would be cool to group the lines in hough space then do an average of their rho and theta to get the desired output line. I thought there would be existing equations to convert a line for cartasian space to hough space. I was unable to find any so I had to figure it out myself. I wrote **_line_to_hough(x1, y1, x2, y2)_** and **_getXforY(y,line)_** to help with converting between the two spaces.

##### White Board Math for line_to_hough(x1, y1, x2, y2)

![alt text](./supportingImages/IMG_20170807_192552.jpg)

##### White Board Math for getXforY(y,line)

![alt text](./supportingImages/IMG_20170813_131020.jpg)

I used **_line_to_hough(x1, y1, x2, y2)_** to convert the lines into hough space. Once I had the lines in hough space I sperated them by those with a theta of greater than pi/2 and less than pi/2. I then averaged each group to get the aveage left and right line corrisponding to the left and right lane. I then used **_getXforY(y,line)_** to get the corrisponding x value for the top and bottom of my area of interest.

### 2. Identify potential shortcomings with your current pipeline

The shortcommings of my current stratagey include cars and other obsticals will not be filtered out by road color. Changes in ligting and road color will throw off my algorithem. I assume there are only two lanes which could cause issues if more than two lanes are in view or a false third lane is detected. 

### 3. Suggest possible improvements to your pipeline

There are several improvments I would love to make but this assinment is way over due. First to deal with the changing road color I was thinking about using back projection or some self-made statical algorthem to determine whether or not a small square infront of the vehicle which we assume to be road matches the rest of the image and we filter out everythihg that fits within those bounds. This could also potentially be used to detect the edge of the road. Second is clustering the lines once they were hough space. This would make the system robust aganist more than lone line in the image as well as stray lines that are not lanes. Finally some type of previous line/lane location memory so that if the new lane location devaites to far from it's previous location. We can then dismiss the new location as noise and continue using the previous one until we get something more reasonable.
