# HarryPotter's Invisibility Cloak using Python and OpenCV
````python
import cv2
import numpy
import time

cap = cv2.VideoCapture(0)

time.sleep(3)
count = 0
background = 0

for i in range(60):
    ret,background = cap.read()
    
background = numpy.flip(background,axis=1)

while(cap.isOpened()):
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 2000)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 2000)
    ret,img = cap.read()
    if not ret:
        break
    count+=1
    img = numpy.flip(img, axis=1)
    
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    lower_red = numpy.array([100, 50, 50])
    upper_red = numpy.array([100, 255, 255])
    mask1 = cv2.inRange(hsv, lower_red, upper_red)
    
    lower_red = numpy.array([155, 50, 50])
    upper_red = numpy.array([180, 255, 255])
    mask2 = cv2.inRange(hsv, lower_red, upper_red)
    
    mask1 = mask1 + mask2
    
    mask1 = cv2.morphologyEx(mask1, cv2.MORPH_OPEN, numpy.ones((3,3), numpy.uint8), iterations = 2)
    mask1 = cv2.dilate(mask1, numpy.ones((3, 3), numpy.uint8), iterations = 1)
    mask2 = cv2.bitwise_not(mask1)
    
    res1 = cv2.bitwise_and(background, background, mask=mask1)
    res2 = cv2.bitwise_and(img, img, mask = mask2)
    final_output = cv2.addWeighted(res1, 1, res2, 1, 0)
    
    cv2.imshow("Harry Potter's invisibility Cloak", final_output)
    k = cv2.waitKey(10)
    
    if k == 27:
        break
    
cap.release()
cv2.destroyAllWindows()
    
````
