#**Finding Lane Lines on the Road** 
---
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of several steps. First, I converted the images to grayscale, then I took gaussian_blur on the grayscale image with a kernel size of 5.
Then i applied canny function on the blurred image with low and high treshold of 50 and 150. Then since i didnt need the whole image and area of interest is only
the area with the roadsm i applied the function to mask_region and save the result as img_canny_mask.

Then i applied hough_lines on the resulted img_canny_mask like this:
    #Get Hough lines
    rho = 2
    theta = np.pi/180
    threshold = 10
    min_line_len = 40 #minimum number of pixels making up a line
    max_line_gap = 25 # maximum gap in pixels between connectable line segments
    img_hough_lines = hough_lines(img_canny_mask, rho, theta, threshold, min_line_len, max_line_gap)    

it internally calls the draw_lines method where i filter the lines as left / right based on their slope values and then i do least square polynomial fit like this:

   if  (rightLane[0] and rightLane[1]):
            coeffRight = np.polyfit(rightLane[0], rightLane[1], 1)
            p = np.poly1d(coeffRight)
            cv2.line(img, (520, int(p(520))), (910, int(p(910))), color, thickness)

and similarly for left lines.

Then i merge those lines with original images.

-------

I use the similar technique for videos as well.

Note: Most of the values / numbers choosen are with trail and error.
    
###2. Identify potential shortcomings with your current pipeline

Perhaps the values i choose are not the best optimal ones, i has also been pointed out in the review. That would explain why it doesnt work proerly in 
optional_challenge.

###3. Suggest possible improvements to your pipeline

Perhaps do more research on the values passed to different functions, but these are hard coded numbers, best scenario would be if the algorithm is able
to change the values dynamically based on the dataset provided.