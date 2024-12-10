import Traffic_light_detection 
import cv2
import random
import os

directory = 'ForReview'
def getName():
    return "Output3/O-"+str(int(random.randrange(0,200)))+".jpg"

for filename in os.listdir(directory):
    f=os.path.join(directory, filename)
    if os.path.isfile(f) and f.endswith('.jpeg') or f.endswith('.jpg') or f.endswith('.webp'):
        img=cv2.imread(f)
        img=Traffic_light_detection.object_detection(img)
        cv2.imshow('frame',img)
        cv2.waitKey()
        cv2.destroyAllWindows() 
        #cv2.imwrite(getName(),img)

'''img=cv2.imread('image4.jpg')
img=Traffic_light_detection.object_detection(img)
cv2.imshow('frame',img)
cv2.waitKey()
cv2.destroyAllWindows()
'''