# **Finding Lane Lines on the Road** 

## Writeup

* This is the fist project by udacity slef driving car nanodegree. This porject uses the basic computer vision technics to solve a very common topic by self driving car.

* The goal of the project is to find lane lines on the road in a photo as well as in a video.
---

### Reflection

### 1. My Pipeline

My pipeline consisted of several steps.
#### First, To seperate yellow color and white color in the picture.
This step was not added until the last challenge video. the challenge video is more like a actual situation, which have dirty road, shadows and all kinds of noises. I found that the laneline in the project are all yellow or white lines. To extract a specific color, i convert the image from RGB space to HLS space. And after several tunning, I sucessfully extraxt the important yellow and white color from the picture.

#### Second, To convert the color space to gray
After I extraxt the important information already, I will convert the picture to gray colorspace which is enough for edge detect in the followed steps.

#### Third, To blur the picture 
Normally, a picture will have some noise, we can try to block these noises using GaussianBlur function in OpenCV.

#### Fourth, To detect the Edge
A very commonly used edge detection function is Canny Edge Detection. Canny Function is several steps combined edge detection function.
Noise Reduction using Gaussian Blur.
Finding Intensity Gradient of the Image using Sobel Filter.
Non-maximum Suppression.
Hysteresis Thresholding.

#### Fifth, To find interested region
For this paticulay project, i found that the camera is in the middle of the car and the lanelines allways fall into the middle part. So we use the fillPoly Function in OpenCV to choose the interested area.
firstly, I just used the numbers picked out from the static pictures. but i found it didn't work in the challenge video. as the challenge video have a bigger size. So in order to make it work for both, i choose the scale of the width and hight as parameters.

#### Sixth, To  find the lines
We use the houghlinesp Function in the opencv. The hough transform is to convert the space to polar coordinates, the small lines can be detected in hough space and converte to x1,y1,x2,y2. The tunning of the paramter of the houghlinep is a tricky work.

#### Seventh, To form the long lines
The houghtrans only return the small seperate lines, we need a function to turn these lines to a complete line scipy.optimize.curvefit or scipy.stats.linregress can form a line form certain amount of points. 
Also, we can use the coordinates and direction of the lines to seperate the left and right line. 
The trial on the pictures are all fine. But in the videos we found the following problem: 
*Not enough input for caculate the lines. This is maybe no line detected in a special frame of the video. I try to solve this tunning the parameters to detecte as many lines as possible. But it still not work. I finally solve this using if there is a line i will form a line.
*The lines in the video can be horizontal. I solve this by select the slope of the lines.


### 2. Identify potential shortcomings with your current pipeline

1.I only use lines to form the laneline, but actual laneline are not only liner but also have curves. this will not work with big curves.
2.The pipeline seems to work on these limited special situations. when situation becomes more complicated, it will not work.


### 3. Suggest possible improvements to your pipeline

1. we can use not a liner line to form the laneline, we can also use higher curve to form the laneline.
2. In a video, we have the current frame and the history frame seconds earlier. we can combine the history to get a more roubst predict.
