import gethsv
import colorsys
import cv2
import numpy as np
def hue_to_rgb(hue):
    hue_normalized=hue/360.0
    rgb_color = colorsys.hsv_to_rgb(hue_normalized, 1, 1)
    rgb_color_scaled = [int(val*255) for val in rgb_color]
    return rgb_color_scaled
def color(hue_value):
    rgb_color = hue_to_rgb(hue_value)
    print(rgb_color)
    # if not (rgb_color[0]%255):
    #     return "red"
    # elif not (rgb_color[1]%255):
    #     return "green"
    # elif not (rgb_color[0]%255) and not (rgb_color[1]%255):
    #     return "yellow"
    # else:
    #     return "SWITCHED OFF"
    ll_red,ul_red=np.array([161,151,84]),np.array([179,255,255])#In BGR
    mask=cv2.inRange(np.array(rgb_color),ll_red,ul_red)
    
    #rgb_numpy=np.array(rgb_color)
    #print(rgb_color)
    #index=(np.abs(rgb_numpy-255)).argmin()
    #print(index)
    #if index==0:    
    #    return "red"
    ##elif index==1:
     #   return "green"
    #else:
    #    return "yellow"
#color(89)
