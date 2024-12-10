import os
import Traffic_light_detection 
#import BetterColor
dir="Images"
for file in os.listdir(dir):
    f = os.path.join(dir, file)
    if os.path.isfile(f) and file.endswith('.jpg'):
        Traffic_light_detection.object_detection(f)
        #print(f+" "+BetterColor.getColor(f))