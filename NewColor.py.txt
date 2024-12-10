import cv2
import numpy as np
def GetColor(img):
    hsv=cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
    ll_red,ul_red=np.array([161,151,84]),np.array([179,255,255])
    ll_green,ul_green=np.array([45,100,50]),np.array([75,255,255])
    ll_yellow = np.array([20, 100, 100]) 
    ul_yellow = np.array([30, 255, 255])
    mask_green=cv2.inRange(hsv,ll_green,ul_green)
    mask_red=cv2.inRange(hsv,ll_red,ul_red)
    mask_yellow=cv2.inRange(hsv,ll_yellow,ul_yellow)
    isGreen=np.argmax(mask_green)
    isRed=np.argmax(mask_red)
    isYellow=np.argmax(mask_yellow)
    if isGreen and not isRed:
        return "green"
    elif isRed and not isGreen:
        return "red"    
    elif isYellow and not isGreen and not isRed:
        return "yellow"
    else:
        return "Switched Off"