# NoMask_Detector_Casade

The face detector for No Masked People using OpenCV.

マスクをしていない人を検出するためにつくりました。

カメラからの入力画像で使う事を想定しています。

画像認識では厳しめです。

### Python Example

```python
import cv2
import sys
import os.path

def detect(filename, cascade_file = "./NoMask_Detector_Casade.xml"):
    if not os.path.isfile(cascade_file):
        raise RuntimeError("%s: not found" % cascade_file)

    cascade = cv2.CascadeClassifier(cascade_file)
    image = cv2.imread(filename, cv2.IMREAD_COLOR)
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    gray = cv2.equalizeHist(gray)
    
    faces = cascade.detectMultiScale(gray,
                                     # detector options
                                     scaleFactor = 1.1,
                                     minNeighbors = 5,
                                     minSize = (24, 24))
    for (x, y, w, h) in faces:
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), 2)

    cv2.imshow("NoMask_Detect", image)
    cv2.waitKey(0)
    cv2.imwrite("Result.png", image)

if len(sys.argv) != 2:
    sys.stderr.write("usage: detect.py <filename>\n")
    sys.exit(-1)
    
detect(sys.argv[1])
```
Run

    python NoMask_Detector_Sample.py image.jpg
