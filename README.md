# openCV_Project FULL DESCRIPTION


import cv2
import winsound

#zero(0) is for single camera if you've Multiple camera you can change with 1,2,3,4..... etc 
cam = cv2.VideoCapture(0,cv2.CAP_DSHOW)
while cam.isOpened():
    ret , frame1 = cam.read()
    ret , frame2 = cam.read()

    # Check Diffrence b/w frame1 & frame2 With the help of (absdiff) and pass the parameter to (cv2.imhsow) 
    # absdiff give you colorfull image i.e this is not recommended
    diff = cv2.absdiff(frame1,frame2)
    
    # cvtColor is convert color in gray and pass gray as a  parameter to cv2.imshow('Umar cam' , gray) 
    gray = cv2.cvtColor(diff, cv2.COLOR_RGB2GRAY) 

    # convert color into Blur We use (GaussianBlur) and pass blur as a  parameter to cv2.imshow('Umar cam' , blur)
    blur =cv2.GaussianBlur(gray, (5,5) ,0 )
    
    # threshold is used to read unwanted thing
    # You've to define two varibles 
    #The first argument is the source image, which should be a grayscale image. The second argument is the threshold value which is used to classify the pixel values. The third argument is the maximum value which is assigned to pixel values exceeding the threshold.
    #fourth parameter of the function. Basic thresholding (cv.THRESH_BINARY,cv.THRESH_BINARY_INV,cv.THRESH_TRUNC,cv.THRESH_TOZERO,cv.THRESH_TOZERO_INV)
    #pass thresh as a  parameter to (cv2.imshow('Umar cam' , thresh))
    _, thresh = cv2.threshold(gray,20,255,cv2.THRESH_BINARY)

    #
    dilated = cv2.dilate(thresh, None, iterations=3)

    # findcontours take two variable 
    # Make border of the object which is moving
    contours, _ = cv2.findContours(dilated, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    # cv2.drawContours(frame1, contours, -1, (0, 255, 0), 2)

    for c in contours:
        # if block is use for ignoring the tiny  object i.w we use 5000
        if cv2.contourArea(c)<5000:
            continue
        # making rectangle over the object
        x,y,w,h = cv2.boundingRect(c)
        cv2.rectangle(frame1,(x,y),(x+w,y+h),(0,255,0),3)
        # winsound.Beep(500,300)
        winsound.PlaySound('alert.wav', winsound.SND_ASYNC)

     # To break the webcam wih the key "q"
    if cv2.waitKey(10) == ord('q'):
        break
    cv2.imshow('Umar cam' , frame1)
