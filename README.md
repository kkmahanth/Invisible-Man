# Invisible-Man

#Import Invisible Modules
import numpy as np
import cv2
import time

#The primary idea is to replace the color pixels with background pixels to generate the invisibility feature
RGB color model is the most popular way to mix and create colors, but in dealing with commerical printers we use CMYK (Cyan, Magenta, Yellow, Key). Unlike RGB and CMYK which use primary colors, HSV is closer to how humans perceive color. It has three components: Hue, Saturation and Value. This color space describes colors (Hue) in terms of their shade (saturation or amount of gray) and their brightness value

#Capturing through webcam and camera to warm up
cap = cv2.VideoCapture(0)
time.sleep(2)
background=0

#capturing the background for 30 seconds so that it could easily save the back ground image
for i in range(30):
    return_val,background=cap.read()
    
 #Reading from web cam    
while(cap.isOpened()):
    
    return_val,img=cap.read()
    if not return_val:
        break;
 
 #Converting the image to HSV format
 hsv=cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
    
    #We are used in detecting red color and these ranges should be carefully choosen
    #Setting lowewr range and upper range for mask1
    lower_red=np.array([0,120,70])
    upper_red=np.array([10,255,255])
    mask1=cv2.inRange(hsv,lower_red,upper_red)
    
    #setting lower and upper range for mask2
    lower_red=np.array([170,120,70])
    upper_red=np.array([180,255,255])
    mask2=cv2.inRange(hsv,lower_red,upper_red)
    
    #The lower range and upper range in the above code block can be changed if we change the color of the cloth
    mask1=mask1+mask2
    
    #Refining the mask corresponding to the detected red color
    mask1=cv2.morphologyEx(mask1,cv2.MORPH_OPEN,np.ones((3,3),np.uint8))
    
    mask1=cv2.morphologyEx(mask1,cv2.MORPH_DILATE,np.ones((3,3),np.uint8))
    
    mask2=cv2.bitwise_not(mask1)
    
    #Generating the final Output
    res1=cv2.bitwise_and(background,background,mask=mask1)
    
    res2=cv2.bitwise_and(img,img,mask=mask2)
    
    final_output=cv2.addWeighted(res1,1,res2,1,0)
    
    cv2.imshow('Invisible Man',final_output)
    
    k=cv2.waitKey(10)
    
    if cv2.waitKey(1) & 0xFF == ord('a'): 
        break

#Release the webcam and destroy all windows
cap.release()
out.release()
cv2.destroyAllWindows()
    
