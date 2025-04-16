markdown_content = """
# 🖱️ Virtual Mouse Using OpenCV, MediaPipe & PyAutoGUI

## 📌 Project Overview
This Python script turns your webcam into a **virtual mouse** using hand gestures. It uses:
- **OpenCV** for capturing and processing video frames
- **MediaPipe** for detecting and tracking hand landmarks
- **PyAutoGUI** to move the system cursor and perform clicks

---

## 🧠 How It Works
- Captures video from the webcam
- Tracks hand landmarks using MediaPipe
- Detects the **index finger tip** to control the cursor
- If the **thumb and index finger** touch → performs a **mouse click**

---

## 📦 Required Libraries

Install required packages using:

```bash
pip install opencv-python mediapipe pyautogui
🧪 Code Breakdown
python
Copy code
import cv2
import mediapipe as mp
import pyautogui

cap = cv2.VideoCapture(0)
hand_detector = mp.solutions.hands.Hands()
drawing_utils = mp.solutions.drawing_utils
screen_width, screen_height = pyautogui.size()
index_y = 0

while True:
    _, frame = cap.read()
    frame = cv2.flip(frame, 1)
    frame_height, frame_width, _ = frame.shape
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output = hand_detector.process(rgb_frame)
    hands = output.multi_hand_landmarks

    if hands:
        for hand in hands:
            drawing_utils.draw_landmarks(frame, hand)
            landmarks = hand.landmark
            for id, landmark in enumerate(landmarks):
                x = int(landmark.x * frame_width)
                y = int(landmark.y * frame_height)

                if id == 8:
                    # Index finger
                    cv2.circle(frame, (x, y), 10, (0, 255, 255), -1)
                    index_x = screen_width / frame_width * x
                    index_y = screen_height / frame_height * y
                    pyautogui.moveTo(index_x, index_y)

                if id == 4:
                    # Thumb
                    cv2.circle(frame, (x, y), 10, (0, 255, 255), -1)
                    thumb_x = screen_width / frame_width * x
                    thumb_y = screen_height / frame_height * y

                    if abs(index_y - thumb_y) < 20:
                        pyautogui.click()
                        pyautogui.sleep(1)

    cv2.imshow('Virtual Mouse', frame)
    cv2.waitKey(1)
🖱️ Controls
Move your index finger to move the mouse

Bring thumb and index finger close to perform a click

🔚 To Exit
Press Ctrl + C in terminal or close the OpenCV window

📁 Use Cases
Hands-free control for presentations

Gesture-based navigation

Accessibility solutions for limited mobility users

📸 Preview
Add a screenshot or screen recording of the virtual mouse in action here!

🙌 Credits
MediaPipe by Google

OpenCV

PyAutoGUI """