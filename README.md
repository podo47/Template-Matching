# Template-Matching
Using NCC and Pyramid strategy to deal with the template matching homework
- [x] Calculate Normalized Correlation Coefficient (NCC)
- [x] Speed up with Pyramid strategy
- [x] Draw the matching position on the picture
- [x] Calculate the distance between the center position of border box and center pattern ( circle or cross ) in the image.
- [x] Record average time required
  
   <img src="https://github.com/podo47/Template-Matching/blob/main/distance.png" width="400" height="200" /><br/>


## 1.Import library
    import os
    import cv2
    import math
    import datetime
    import numpy as np
    import pandas as pd
    import matplotlib.image as img
    import matplotlib.pyplot as plt
    from google.colab.patches import cv2_imshow

## 2.Read the images
*  Tenplate images are in file : [pattern]()
  *  Template_circle
  *  Template_cross
  *  Template_BorderCircle
  *  Template_BorderCross
*  Source images are in file : [circle]() or [cross]()
  *  Panel{ 1-4 }_circle{ 1-4 }
  *  Panel{ 1-4 }_cross{ 1-4 }
  
## 3.Template matching
**Part 1 : Non-pyramid**

   (1)   Scan and calculate NCC score through whole source image
   
   (2)   Find the highest NCC score and record the corresponding coordinate
   
   (3)   Draw the matching position on the picture ( Both border box and center pattern )
   
   (4)   Calculate the distance between the center position of border box and center pattern ( circle or cross ) in the image
   
   (5)   Save all results
- Distance ( Group by different pattern : circle or cross )
  - [File](main/distance.png) : match_circle/match_circle_dis.csv
  - [File]() : match_cross/match_cross_dis.csv
- Result images 
  - [File]() : match_circle/P{ 1-4 }_circle{ 1-4 }
  - [File]() : match_cross/P{ 1-4 }_cross{ 1-4 }
