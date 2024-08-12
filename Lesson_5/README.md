
# Computer Vision in FPGA
  
## Intergrating the OpenCV UI into Ultra96

In Lesson 1, we finished a UI design using OpenCV and Python. In Lesson 4, we finished a Computer Vision program.
Please make a new notebook and copy these code into it.


```python

import cv2
import numpy as np
import datetime
import imageio
import os
os.environ["OPENCV_LOG_LEVEL"]="SILENT"
from PIL import ImageFont, ImageDraw, Image
from pynq.lib.video import *
from tqdm import tqdm
import time
```
```python
displayport = DisplayPort()

displayport.configure(VideoMode(1280, 720, 24), PIXEL_RGB)
```

```python
capture = cv2.VideoCapture(0)
capture.set(3, 1280)
capture.set(4, 720)
```
```python
# Set the lower and upper bounds for black color in HSV color space
# lower_black = np.array([0, 0, 0], dtype=np.uint8)
# upper_black = np.array([179, 255, 30], dtype=np.uint8)
# Set the lower and upper bounds for yellow color in HSV color space
# lower_yellow = np.array([20, 100, 100], dtype=np.uint8)
# upper_yellow = np.array([50, 255, 255], dtype=np.uint8)
# Set the lower and upper bounds for red color in HSV color space
lower_red = np.array([0, 100, 100], dtype=np.uint8)
upper_red = np.array([10, 255, 255], dtype=np.uint8)

# Set the minimum contour area threshold
min_contour_area = 200

# Set the kernel size for morphological operations
kernel_size = 5


number_frames = 100
start = time.time()

for i in range(number_frames):
    frame = displayport.newframe()
    capture.read(frame)
    # Convert the frame to HSV color space
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # Create a mask for black color detection
    mask = cv2.inRange(hsv, lower_red, upper_red)

    # Perform morphological closing to fill small gaps in black regions
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (kernel_size, kernel_size))
    mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)

    # Find contours in the mask
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    # Filter contours based on area threshold
    filtered_contours = [cnt for cnt in contours if cv2.contourArea(cnt) > min_contour_area]
    filtered_contours?
    
    # Draw bounding boxes around the filtered contours
    for cnt in filtered_contours:
        x, y, w, h = cv2.boundingRect(cnt)
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(frame, f"Red Object", (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (36, 255, 12), 2)
    
    cv2.putText(frame,f"Number of Object = "+str(len(filtered_contours)), (60,100), cv2.FONT_HERSHEY_SIMPLEX, 1.5, (36, 255, 12), 2)
    # Add a blue border around the frame
    border_width = 1000
    displayport.writeframe(frame)

end = time.time()
duration = end - start
print(f"Took {duration} seconds at {number_frames / duration} FPS")

```
```python
capture.release()
displayport.close()
```


  

Edit the above code to create a UI for your Image Processing result, using the knowledge you learn in Lesson 1 and Lesson 4. 

Also, try to implement multi-colour recognition algoirthm.


## DPU in Ultra96

DPU(Deep Processing Unit) is a PYNQ library, allow us to implement AI algoirthms in the Programming Logic of the Ultra96 MPSoC. 

To test it out, open the folder pynq-dpu and	 notebook: "dpu_tf_inceptionv1.ipynb" 