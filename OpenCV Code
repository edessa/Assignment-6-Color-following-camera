import numpy as np
import cv2
import cv, math
import time
import serial

cap = cv2.VideoCapture(0)
ser = serial.Serial('/dev/ttyACM0', 9600)

while(True):
    # Capture frame-by-frame
    ret, frame = cap.read()
    frame = cv2.flip(frame,1)
    # Display the resulting frame
    # Convert BGR to HSV
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # define range of blue color in HSV
    lower_blue = np.array([110, 50, 50], dtype=np.uint8)
    upper_blue = np.array([130,255,255], dtype=np.uint8)


    # Threshold the HSV image to get only blue colors
    mask = cv2.inRange(hsv, lower_blue, upper_blue)
    binary_blue = cv2.inRange(hsv, lower_blue, upper_blue)
    # Bitwise-AND mask and original image
#    res = cv2.bitwise_and(frame,frame, mask= mask)
    
    #Contours for tracking
    contours, hierarchy = cv2.findContours(mask, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
    max_area = 0
    largest_contour = None
    for idx, contour in enumerate(contours):
      area = cv2.contourArea(contour)
      if area > max_area:
        max_area = area
        largest_contour = contour
    if not largest_contour == None:
      moments = cv2.moments(mask) 
      #there can be noise in the video so ignore objects with small areas 
      if moments['m00']!=0:
	  if area == 0:
	     area = 1
          x = int(moments['m10']/moments['m00'])         # cx = M10/M00
          y = int(moments['m01']/moments['m00'])         # cy = M01/M00
	  rect = cv2.minAreaRect(largest_contour)        
	  rect = ((rect[0][0] , rect[0][1]), (rect[1][0], rect[1][1] ), rect[2])
	  xn = x/640.0 * 2 -1

	  xb = math.fabs(xn) * 127
	  if xn > 0 and xn < 256:
	    xb = xb + 128

	  xb = int(round(xb))
	  ser.write(chr(xb))
	  print xb

	  box = cv2.cv.BoxPoints(rect)
          box = np.int0(box)
          
	  #draw every frames 
	  cv2.drawContours(frame,[box], 0, (0, 0, 255), 2)	

    cv2.imshow('frame',frame)
    cv2.imshow('mask',binary_blue)
    
    cv2.imshow('frame',frame)
    #if cv2.waitKey(0) & 0xFF == ord('q'):
    if cv2.waitKey(10) >= 0:
       break
