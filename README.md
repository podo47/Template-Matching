# Template-Matching
Using NCC and Pyramid strategy to deal with the template matching tasks

- [x] Calculate Normalized Correlation Coefficient (NCC)
- [x] Speed up with Pyramid strategy
- [x] Apply median filter to the image for better performance
- [x] Apply power law transform with gamma = 0.4 in special case 
- [x] Draw the matching position on the picture
- [x] Calculate the distance between the center position of border box and center pattern ( circle or cross ) in the image.
- [x] Record average time required
  
   <img src="https://github.com/podo47/Template-Matching/blob/main/distance.png" width="400" height="200" /><br/>

[Click to read the code](https://github.com/podo47/Template-Matching/blob/main/Template_matching.ipynb)
--------------

## 1.Import library
``` python
import os
import cv2
import math
import datetime
import numpy as np
import pandas as pd
import matplotlib.image as img
import matplotlib.pyplot as plt
from google.colab.patches import cv2_imshow
```

## 2.Read the images
*  Tenplate images are in file : [pattern](https://github.com/podo47/Template-Matching/tree/main/pattern)
    *  Template_circle
    *  Template_cross
    *  Template_BorderCircle
    *  Template_BorderCross
*  Source images are in file : [circle](https://github.com/podo47/Template-Matching/tree/main/circle) or [cross](https://github.com/podo47/Template-Matching/tree/main/cross)
    *  Panel{ 1-4 }_circle{ 1-4 }
    *  Panel{ 1-4 }_cross{ 1-4 }
  
## 3.Template matching
**Part 1 : Non-pyramid**

Case 1 : Circle
``` python
# Read source
circle_ori = cv2.imread('/content/drive/MyDrive/Matching/cross/Panel1_cross2.bmp')
# Apply a "median" blur to the image
blurred = cv2.medianBlur(circle_ori,9)
# Template matching
'''
cir_match(src,temp,temp_border,draw)
src : Source image
temp : Template image
temp_border : Border template image
draw : Image need to be drawn the matching position
'''
circle = cir_match(blurred,temp_cir,temp_border_cir,circle_ori)
```

Case 2 : Cross
``` python
# Read source
cross_ori = cv2.imread('/content/drive/MyDrive/Matching/cross/Panel1_cross2.bmp') 
# Apply a "median" blur to the image
blurred_cro = cv2.medianBlur(cross_ori,9)
# Template matching
'''
cro_match(src,temp,temp_border,draw)
src : Source image
temp : Template image
temp_border : Border template image
draw : Image need to be drawn the matching position
'''
cross = cro_match(blurred_cro,temp_cro,temp_border_cro,cross_ori)
```

   (1)   Scan and calculate NCC score through whole source image
   
   (2)   Find the highest NCC score and record the corresponding coordinate
   
   (3)   Draw the matching position on the image ( Both border box and center pattern )
   
   (4)   Calculate the distance between the center position of border box and center pattern ( circle or cross ) in the image
   
   (5)   Save all results
- Distance ( Group by different pattern : circle or cross )
  - [File(Circle)](https://github.com/podo47/Template-Matching/blob/main/match_circle/match_circle_dis.csv) : match_circle/match_circle_dis.csv
  - [File(Cross)](https://github.com/podo47/Template-Matching/blob/main/match_cross/match_cross_dis.csv) : match_cross/match_cross_dis.csv
- Result images 
  - [File(Circle)](https://github.com/podo47/Template-Matching/tree/main/match_circle) : match_circle/P{ 1-4 }_circle{ 1-4 }
  - [File(Cross)](https://github.com/podo47/Template-Matching/tree/main/match_cross) : match_cross/P{ 1-4 }_cross{ 1-4 }

**Part 2 : Pyramid**

Case 1 : Circle
``` python
# Read source
circle_ori = cv2.imread('/content/drive/MyDrive/Matching/circle/Panel1_circle1.bmp')

# Apply a "median" blur to the image 
blurred = cv2.medianBlur(circle_ori,9)

# Template matching
'''
cir_match_p(src,temp,temp_border,draw)
src : Source image
temp : Template image
temp_border : Border template image
draw : Image need to be drawn the matching position
'''
circle = cir_match_p(blurred,temp_cir,temp_border_cir,circle_ori)
```

Case 2 : Cross
``` python
# Read source
cross_ori = cv2.imread('/content/drive/MyDrive/Matching/cross/Panel1_cross1.bmp') 
# Apply a "median" blur to the image
blurred_cro = cv2.medianBlur(cross_ori,9)
# Template matching
'''
cro_match_p(src,temp,temp_border,draw)
src : Source image
temp : Template image
temp_border : Border template image
draw : Image need to be drawn the matching position
'''
cross = cro_match_p(blurred_cro,temp_cro,temp_border_cro,cross_ori)
```

   (1)   Build pyramid according to the max leval ( default = 10 )
   
   (2)   Scan and calculate NCC score through whole source image ( Through all elements in pyramid )
   
   (3)   Find the highest NCC score and record the corresponding coordinate
   
   (4)   Draw the matching position on the image ( Both border box and center pattern )
   
   (5)   Calculate the distance between the center position of border box and center pattern ( circle or cross ) in the image
   
   (6)   Save all results
- Distance ( Group by different pattern : circle or cross )
  - [File(Circle)](https://github.com/podo47/Template-Matching/blob/main/match_circle_p/circle_pyramid_dis.csv) : match_circle_p/circle_pyramid_dis.csv
  - [File(Cross)](https://github.com/podo47/Template-Matching/blob/main/match_cross_p/cross_pyramid_dis.csv) : match_cross_p/cross_pyramid_dis.csv
- Result images 
  - [File(Circle)](https://github.com/podo47/Template-Matching/tree/main/match_circle_p) : match_circle_p/P{ 1-4 }_circle{ 1-4 }
  - [File(Cross)](https://github.com/podo47/Template-Matching/tree/main/match_cross_p) : match_cross_p/P{ 1-4 }_cross{ 1-4 }

## 4. Result

* [All distance record](https://github.com/podo47/Template-Matching/blob/main/distance_all.csv)

**Part 1 : Non-pyramid**
1. Average consume time 
  * Circle - 0 days 00:24:43.240662375
  * Cross - 0 days 00:25:41.053639312
2. Distance
  * Circle
  
|Image      |Distance          |
|--------------|------------------|
|Panel1_circle1|6.324555320336759 |
|Panel1_circle2|5.0990195135927845|
|Panel1_circle3|3.1622776601683795|
|Panel1_circle4|13.341664064126334|
|Panel2_circle1|7.810249675906654 |
|Panel2_circle2|8.06225774829855  |
|Panel2_circle3|2.8284271247461903|
|Panel2_circle4|11.704699910719626|
|Panel3_circle1|8.54400374531753  |
|Panel3_circle2|14.142135623730953|
|Panel3_circle3|17.26267650163207 |
|Panel3_circle4|17.0              |
|Panel4_circle1|4.242640687119285 |
|Panel4_circle2|7.071067811865476 |
|Panel4_circle3|5.656854249492381 |
|Panel4_circle4|13.601470508735444|

  
  * Cross

|Image      |Distance          |
|-------------|------------------|
|Panel1_cross1|12.206555615733702|
|Panel1_cross2|17.08800749063506 |
|Panel1_cross3|16.278820596099706|
|Panel1_cross4|10.770329614269007|
|Panel2_cross1|4.47213595499958  |
|Panel2_cross2|10.295630140987   |
|Panel2_cross3|24.166091947189145|
|Panel2_cross4|11.40175425099138 |
|Panel3_cross1|14.560219778561036|
|Panel3_cross2|15.811388300841896|
|Panel3_cross3|3.1622776601683795|
|Panel3_cross4|15.231546211727817|
|Panel4_cross1|3.605551275463989 |
|Panel4_cross2|7.0710678118654755|
|Panel4_cross3|13.416407864998739|
|Panel4_cross4|5.385164807134504 |

3. Result image ( show only 1 image from each group )
  * Circle
  
  <img src="https://github.com/podo47/Template-Matching/blob/main/match_circle/P1_circle1.png" width="432" height="238" /><br/>
  
  * Cross
  
  <img src="https://github.com/podo47/Template-Matching/blob/main/match_cross/P1_cross1.png" width="432" height="238" /><br/>
  
**Part 2 : Pyramid**
1. Average consume time 
  * Circle - 0 days 00:00:00.749987812
  * Cross - 0 days 00:00:00.713385562
2. Distance
  * Circle
  
|Image       |Distance          |
|-------------|------------------|
|Panel1_circle1|6.324555320336759 |
|Panel1_circle2|5.0990195135927845|
|Panel1_circle3|3.1622776601683795|
|Panel1_circle4|13.341664064126334|
|Panel2_circle1|7.810249675906654 |
|Panel2_circle2|8.54400374531753  |
|Panel2_circle3|2.8284271247461903|
|Panel2_circle4|11.704699910719626|
|Panel3_circle1|8.54400374531753  |
|Panel3_circle2|14.142135623730951|
|Panel3_circle3|17.26267650163207 |
|Panel3_circle4|17.0              |
|Panel4_circle1|4.242640687119285 |
|Panel4_circle2|7.810249675906654 |
|Panel4_circle3|10.198039027185569|
|Panel4_circle4|13.601470508735444|

  * Cross

|Image      |Distance          |
|-------------|------------------|
|Panel1_cross1|11.40175425099138 |
|Panel1_cross2|16.15549442140351 |
|Panel1_cross3|16.278820596099706|
|Panel1_cross4|10.770329614269007|
|Panel2_cross1|4.123105625617661 |
|Panel2_cross2|10.295630140987   |
|Panel2_cross3|23.769728648009426|
|Panel2_cross4|10.44030650891055 |
|Panel3_cross1|14.560219778561036|
|Panel3_cross2|15.524174696260024|
|Panel3_cross3|4.123105625617661 |
|Panel3_cross4|15.231546211727817|
|Panel4_cross1|3.605551275463989 |
|Panel4_cross2|7.0710678118654755|
|Panel4_cross3|13.416407864998739|
|Panel4_cross4|5.385164807134504 |

3. Result image ( show only 1 image from each group )

  * Circle
  
  <img src="https://github.com/podo47/Template-Matching/blob/main/match_circle_p/P1_circle1.png" width="432" height="238" /><br/>
  
  * Cross
  
  <img src="https://github.com/podo47/Template-Matching/blob/main/match_cross_p/P1_cross1.png" width="432" height="238" /><br/>
