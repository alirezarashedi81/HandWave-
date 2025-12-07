# HandWave — Real-Time Hand Tracking Mouse Controller (MediaPipe + OpenCV)

HandWave is a real-time hand-tracking mouse controller built using **MediaPipe**, **OpenCV**, and **Windows system cursor APIs**.  
It allows you to control your mouse using natural hand gestures:

- 🖱️ **Left hand** → cursor control  
- 👆 Thumb + Index → **Double-click**  
- 🤙 Thumb + Pinky → **Hold & Drag**  
- ✋ Right hand → **Gesture distance tracking** (debug / future features)

This project uses smoothing, dead-zone filtering, velocity scaling, and stable MediaPipe landmark tracking to achieve **fast and accurate cursor movement**.

---

## ✨ Features

### 🎯 **Cursor Control (Left Hand)**
- Move your hand → moves the mouse cursor  
- Natural motion with:
  - EMA smoothing  
  - Dead-zone to remove jitter  
  - Velocity mapping for high-resolution screens  

### 👆 **Gestures**
| Gesture | Hand | Action |
|--------|------|--------|
| Thumb + Index | Left | Double-click |
| Thumb + Pinky | Left | Hold left mouse button (drag) |
| Thumb + Index | Right | Distance display (debug) |
| Thumb + Pinky | Right | Distance display (debug) |

---

## 📦 Installation

### 1. **Install Python 3.10**
MediaPipe **only supports Python ≤ 3.10**.

Download Python 3.10 from:  
https://www.python.org/downloads/release/python-3100/

---

## 2. **Create Virtual Environment**
python3.10 -m venv HandWave


Activate it:

### **Windows**

HandWave\Scripts\activate


---

## 3. **Install Dependencies**

Create a `requirements.txt` file:



opencv-python==4.9.0.80
mediapipe==0.10.9
pygame==2.5.2
numpy==1.26.4


Then install:



pip install -r requirements.txt


---

## ▶️ Running the Program

Once inside the environment:



python handwave.py


Exit the program by pressing:



q


---

## 📁 Project Structure



HandWave/
│
├── handwave.py # Main application code
├── requirements.txt # Project dependencies
└── README.md # Documentation


---

## 💡 How It Works

### 🔍 1. **Hand Tracking**
MediaPipe detects:
- 21 landmarks per hand  
- Left / right handedness  
- Thumb, index, middle, pinky tip coordinates  

### 🧮 2. **Smoothing**
Every frame uses **Exponential Moving Average (EMA)**:
```python
smoothed = alpha * current + (1 - alpha) * previous

🌀 3. Dead-Zone

Tiny movements around the previous point are ignored:

distance < DEAD_ZONE_THRESHOLD


Prevents shaking.

🚀 4. Velocity Mapping

Frame-space → Screen-space scaling + nonlinear motion:

vx = (dx ** gamma) * gain

🖱 5. Windows Cursor Control

Uses Win32 API:

SetCursorPos(x, y)
mouse_event(...)
