import cv2
import numpy as np

def hsv(image):
    hsv_image=cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    h,s,v=cv2.split(hsv_image)
    max_value_coords=np.unravel_index(np.argmax(v),v.shape)
    max_value_hue=h[max_value_coords]
    return max_value_hue
