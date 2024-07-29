# PCB Etching Production Line User Manual

## Web_Server

The following section will explain how to set up a web server on ultra96v2 using flask to live stream the camera from ultra96

```python
from flask import Flask, render_template, Response
import cv2
import numpy as np

app = Flask(__name__)
camera = cv2.VideoCapture(0)

def generate_frames():
    while True:
        success, frame = camera.read()
        if not success:
            break
        else:
            # Convert the frame to HSV color space
            hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

            # Create a mask for black color detection
            mask = cv2.inRange(hsv, (0, 0, 0), (179, 255, 30))

            # Find contours in the mask
            contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

            # Filter contours based on area threshold
            filtered_contours = [cnt for cnt in contours if cv2.contourArea(cnt) > 200]

            # Draw bounding boxes around the filtered contours
            for cnt in filtered_contours:
                x, y, w, h = cv2.boundingRect(cnt)
                cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

            # Encode the frame as JPEG
            ret, buffer = cv2.imencode('.jpg', frame)
            frame = buffer.tobytes()

        yield (b'--frame\r\n'
                b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/video1')
def video1():
    return Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5000)
```
In the following code, there are three main sections to pay attention to

1) creating the application
 - as seen in Flask(__name__) is used to create the flask web server base

 2) Rendering the template
 - the template consists of a html file which is used to make the propertioes of our web server and define where to add the live streaming video and text. It can be modified for additional features as well using html.

 To create it in the app, we have to route it using `app.route`

 3) Add the live streaming video into the web server:

 - Here there are two main sections, the `generate_frames` function which encodes the frame data into a jpeg format
 - And the app.route section which calls the generate frames function to send the jpeg frames to the web server.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Video Streams</title>
    <style>
        .video-container {
            display: flex;
            flex-direction: row;
        }
        
        .video-container iframe {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <h1>Video Streams</h1>
    
    <div class="video-container">
        <div>
            <h2>Localhost Stream</h2>
            <iframe src="http://127.0.0.1:5000/video1" width="640" height="480" frameborder="0"></iframe>
        </div>
        
        <div>
            <h2>Ultra96v2 Stream</h2>
            <iframe src="http://192.168.50.121:5000/video1" width="640" height="480" frameborder="0"></iframe>
        </div>
    </div>
</body>
</html>
```

The html template utilizes iframe to load both the streaming addresses onto one ip address.

 The final output is the below:

 ![classes](./stream.gif)