# 🧠 OpenCV Object Tracking

This project demonstrates object tracking using OpenCV and Python.  
It identifies and tracks **red-colored objects** in a video and outputs a GIF demo.

---

## 🎥 Demo

![Tracking Demo](output.gif)

---

## 📂 Project Structure

📁 opencv-object-tracking/
├── video.mp4 ← Input video file
├── object_tracking.py ← Python script for object tracking
├── output.gif ← Generated GIF of the result
└── README.md ← This file

---

## 💻 Code Overview

```python
import cv2
import imageio

cap = cv2.VideoCapture('video.mp4')
frames_for_gif = []

while True:
    ret, frame = cap.read()
    if not ret:
        break

    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower_red = (0, 100, 100)
    upper_red = (10, 255, 255)
    mask = cv2.inRange(hsv, lower_red, upper_red)

    contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    for cnt in contours:
        area = cv2.contourArea(cnt)
        if area > 500:
            x, y, w, h = cv2.boundingRect(cnt)
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
            cv2.putText(frame, "Red Object", (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 0), 2)

    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    frames_for_gif.append(rgb_frame)

    cv2.imshow("Object Tracking", frame)
    if cv2.waitKey(30) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

imageio.mimsave("output.gif", frames_for_gif, fps=10)
print("✅ Saved as output.gif")
```

## 📦 Requirements

Install dependencies using pip
```
pip install opencv-python imageio
```

## ▶️ Run the Script

```
python object_tracking.py
```
