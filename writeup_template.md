# **Finding Lane Lines on the Road** 

## Gary Holness

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

I developed my pipleline using the six static images of roadway scenes.  I looped over each
image file in the test_images directory and performed 5 steps on each.  This included conversion
from color image to grayscale, blurring the grayscale image by convolition with a Gaussian
kernel to suppres noise and remove spurious artifacts, detecting edges using the Canny algorithm,
masking the edges within the region of interest describing the car's lane,  and finally using
the Hough transform to extract the dominant lines maskeded from the region of interest.  To better
visualize results from the static images, I displayed a subplot consisting of 3 entries for
each of the images.  The subplots contained the original image, the extracted lane lines, and
the extracted lane lines superimposed on the original image.   

Once I was convinced my pipleine worked on the static images, I tried it on the video and
tuned the parameters until I observed results with which I was happy.   The extention of
the draw_lines fuction took the approach of separating the edge map into two groups,
namely the lines corresponding to the the left lane line and the lines corresponding
to the right lane line.  This was done by checking for positive slope (right lane line)
and negative slope (left lane line).  Once the edges were partitioned, I performed a
linear fit (polynomial of degree 1) on each set to get the parameters of the line
describing the left lane and right lane.  Then, I took the domain for the x-values for
the left-lane and right-lane edges and used these to extrapolate using the equation for
the left and right lane lines returned from the linear fit.   When draw the lane lines,
I widen the line-width for the red visualization of the lane lines.

Applying my code to the video required a little bit of tinkering with parameters for the
Gaussian blur, the Canny Edge Detector, and the Hough Transform.

The majority of the issues I had with the project concerns setup of the programming
environment.  The instructions as written did not work on MacOS X 10.11 (El Capitan).
Miniconda version using Python 3 actually installs Python 3.6.  To get the environment
working with the test code, required downgrading from Pytin 3.6 to Python 3.5.2.  A number
of other pieces (moviepy, ffmpeg) did not install correctly.  Countless hours were spent
looking at sources on the web and tinkering in order to resolve the problem.  An example
of this is that the version of ffmpeg that you install with conda does not work and results
in a boken pipe error when running the test.  The version of ffmpeg that is inside of
the conda environmetn (carnd-term1) works.  For some reason the version inside and outside
the environment do not produce the same result.   In addition, there is an issue concerning
setup of SSL keys for use with GitHub.  It is specific to MacOS and it concerns the SSL
config directives for storing the SSL key passphrase in MacOS's Keychain.   In addition,
one must go into the notebook.json specification in the Jupyter installation directory
to physically change the path for the python version installed by default to point to
Python 3.5.2.  Otherwise, there is a kernel error when running the Jupyter notebook.
These issues consumed roughly 80% of the time I spent on this project and made
for an experience that was not much fun.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

I would like to have run some statistic on the greyscale image to
implement a more data-driven approach that informs good choices for
parameter for Canny and Hough transform rather than using trial and
error on the videos to select values.   In addition the programming
using the Jupyter notebook does not lend itself to creating robust
code.  I would have liked to have a debugger where I can set breakpoints
and examine variables.  I resorted to print statements, very primitive,
to debug my code. Print statements can only go so far.  In addition
reading stack traces lead me to write more primitive code.  With
a debugger, it would allow one to step into code versus decipering
stack traces when presented with run-time error.   



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to investigate a transform
that detects curved lines to deal with the extra challenge
problem.  I also wonder, based on the car's speed, or perhaps
optical flow field if we can tune how far ahead of the vehicle
the region of interest extends.   When the car moves slowly
there's no need to go to the horizon, but when the car moves
fast, proportional to it's speed, the region of interest could
perhaps extend farther towards the horizon.

