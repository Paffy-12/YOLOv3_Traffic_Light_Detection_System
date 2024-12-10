import cv2
import numpy as np
import random
import NewColor
from ForClassNames import classes

def generateNames():
    return "CroppedImages/IMG-"+str(int(random.uniform(0,255)))+".jpg"
yolo = cv2.dnn.readNet('yolov3.cfg','yolov3.weights')
yolo.setPreferableBackend(cv2.dnn.DNN_BACKEND_OPENCV)
def object_detection(img):
    traffic_light=img#cv2.imread(file)
    height,width,_=traffic_light.shape
    blob=cv2.dnn.blobFromImage(traffic_light,1/255,(416,416),(0,0,0),swapRB=True,crop=False)
    yolo.setInput(blob)
    ln=yolo.getLayerNames()
    ln=[ln[i-1] for i in yolo.getUnconnectedOutLayers()]
    layeroutput=yolo.forward(ln)
    boxes=[]
    confidences=[]
    for output in layeroutput:
        for detection in output:
            score=detection[5:]
            class_id=np.argmax(score)
            confidence=score[class_id]
            if class_id==20:
                if confidence > 0.6:
                    box=detection[:4]
                    centerX=box[0]*width
                    centerY=box[1]*height
                    w=box[2]*width
                    h=box[3]*height
                    x = int(centerX-(w/2))
                    y = int(centerY-(h/2))
                    boxes.append((x,y,w,h))
                    confidences.append(float(confidence))
    indexes=np.array(cv2.dnn.NMSBoxes(boxes,confidences,0.5,0,4))
    font=cv2.FONT_HERSHEY_SIMPLEX
    for i in indexes.flatten():
        (x, y) = (int(boxes[i][0]), int(boxes[i][1]))
        (w, h) = (int(boxes[i][2]), int(boxes[i][3]))
        cropped_img=traffic_light[y:y+h,x:x+w]
        
        #cv2.imwrite(generateNames(),cropped_img)
        #h=gethsv.hsv(cropped_img)
        label=NewColor.GetColor(cropped_img)
        if label=="red":
            color=(0,0,255)
        elif label=="green": 
            color=(0,255,0)
        elif label=="yellow":
            color=(0,255,255)
        else:
            color=(255,255,255)
        cv2.rectangle(traffic_light, (x,y), (x+w,y+h), color, 3)
        cv2.putText(traffic_light,label,(x,y),font,2,(255,255,255),2)
    return traffic_light
