import cv2
import numpy as np
def getLimits(color):
    color=np.uint8([[color]])
    hsv_color=cv2.cvtColor(color,cv2.COLOR_BGR2HSV)
    lowerLimit = hsv_color[0][0][0] - 10, 100, 100
    upperLimit = hsv_color[0][0][0] + 10, 255, 255
    ll=np.array(lowerLimit,dtype='uint8')
    ul=np.array(upperLimit,dtype='uint8')
    return ll,ul
def getContours(result,mask):
    gray=cv2.cvtColor(result,cv2.COLOR_BGR2GRAY)
    edge=cv2.Canny(gray,50,200)
    contour,heirarchy=cv2.findContours(edge,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    return contour
cam=cv2.VideoCapture(0)
ll_yellow,ul_yellow=getLimits((0,255,255))
ll_green,ul_green=getLimits((0,255,0))
#red wraps around hsv scale,difficult to calculate through formula
#Hardcoding limits for red
ll_red,ul_red=np.array([161,151,84]),np.array([179,255,255])
while True:
    #ret,act_frame=cam.read()
    _,act_frame=cam.read()
    frame=cv2.GaussianBlur(act_frame,(5,5),0)
    hsv=cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)

    
    mask_yellow=cv2.inRange(hsv,ll_yellow,ul_yellow)
    mask_green=cv2.inRange(hsv,ll_green,ul_green)
    mask_red=cv2.inRange(hsv,ll_red,ul_red)
    
    
    result_red=cv2.bitwise_and(frame,frame,mask=mask_red)
    result_green=cv2.bitwise_and(frame,frame,mask=mask_green)
    result_yellow=cv2.bitwise_and(frame,frame,mask=mask_yellow)

    
    contour_red=getContours(result_red,mask_red)
    contour_yellow=getContours(result_yellow,mask_yellow)
    contour_green=getContours(result_green,mask_green)
    
    cv2.drawContours(act_frame,contour_red,-1,(0,255,0),2)
    cv2.drawContours(act_frame,contour_green,-1,(0,255,0),2)
    cv2.drawContours(act_frame,contour_yellow,-1,(0,255,0),2)
    
    cv2.imshow('frame',act_frame)
    if cv2.waitKey(1)==ord('q'):
        break
cam.release()
cv2.destroyAllWindows()