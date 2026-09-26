# 🌾 FarmGuard

## AI-Powered Smart Agricultural Protection System

FarmGuard is an AI-powered agricultural monitoring and protection system designed to detect animals approaching a protected farm area.

The system combines an **Arduino UNO**, **HC-SR04 ultrasonic distance sensor**, **1080P USB webcam**, **machine learning model**, **desktop application**, **LED indicators**, **desktop alert sound**, and **email notifications**.

When an object enters the configured detection range, the Arduino detects the object using the HC-SR04 sensor and communicates with the FarmGuard desktop application. The desktop application then activates the webcam, captures an image, and uses the AI model to classify the detected object.

The current AI model classifies three categories:

* 🐃 **Baffulo**
* 🐖 **Porc**
* 🌿 **Nature**

If the AI detects **Baffulo** or **Porc** with sufficient confidence, FarmGuard activates the alert system, plays a sound through the computer, turns on the red LED, and sends an email containing the captured image.


## 📌 Project Overview

FarmGuard was developed as a practical prototype for smart agricultural protection.

Traditional farm monitoring may require continuous human observation. FarmGuard aims to automate part of this process by combining distance sensing, computer vision, artificial intelligence, and communication technologies.

### Main objectives

* Detect objects approaching a protected area.
* Automatically trigger the camera when an object is detected.
* Capture an image of the detected object.
* Classify the captured image using an AI model.
* Distinguish between animals and non-animal objects.
* Provide a visual alert through the desktop application.
* Activate the red LED when an animal is detected.
* Play a custom alert sound through the computer.
* Send an email alert with the captured image.
* Maintain a history of detections.


# 🏗️ System Architecture

                    ┌──────────────────┐
                    │     HC-SR04      │
                    │ Distance Sensor  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    Arduino UNO   │
                    │                  │
                    │ D6  → ECHO       │
                    │ D7  → TRIG       │
                    │ D8  → Green LED  │
                    │ D9  → Red LED    │
                    │ D10 → Buzzer     │
                    └────────┬─────────┘
                             │
                          USB/Serial
                             │
                             ▼
                ┌──────────────────────────┐
                │     FarmGuard Desktop    │
                │        Application       │
                │                          │
                │  Arduino Communication   │
                │  Camera Control          │
                │  AI Classification       │
                │  Alert Sound             │
                │  Email Notifications     │
                │  Detection History       │
                └────────────┬─────────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │   1080P Webcam   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    AI Model      │
                    │                  │
                    │ Baffulo          │
                    │ Porc             │
                    │ Nature           │
                    └──────────────────┘

# 🔄 System Workflow

START
  │
  ▼
Arduino connected
  │
  ▼
HC-SR04 monitors distance
  │
  ├── Distance > 50 cm
  │       │
  │       ▼
  │     SAFE
  │
  └── Distance ≤ 50 cm
          │
          ▼
   Object detected
          │
          ▼
     Activate camera
          │
          ▼
      Capture image
          │
          ▼
      AI processing
          │
          ▼
     Classification
          │
      ┌───┴───────────┐
      │               │
      ▼               ▼
    Nature        Animal
      │           /      \
      ▼          ▼        ▼
    SAFE        Porc    Baffulo
                  │        │
                  └───┬────┘
                      │
                      ▼
                    ALERT
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
          Red LED   Sound    Email
                       │        │
                       ▼        ▼
                  Desktop    Image
                   Sound    Attachment

# 🔌 Hardware Requirements

## Required Hardware

| Component        | Purpose                                           |
| ---------------- | ------------------------------------------------- |
| Arduino UNO R3   | Controls the sensor and hardware indicators       |
| HC-SR04          | Measures distance and detects approaching objects |
| 1080P USB Webcam | Captures images for AI classification             |
| Green LED        | Indicates safe/normal state                       |
| Red LED          | Indicates animal alert                            |
| Buzzer           | Hardware alert/optional indicator                 |
| Breadboard       | Connects the electronic components                |
| Jumper wires     | Hardware connections                              |
| 220 Ω resistors  | LED current limiting                              |
| USB cable        | Arduino-to-computer communication                 |
| Computer/Laptop  | Runs the FarmGuard desktop application            |


# 🔧 Hardware Wiring

## Arduino UNO Pin Assignment

| Arduino Pin | Component              | Function                     |
| ----------- | ---------------------- | ---------------------------- |
| D6          | HC-SR04 ECHO           | Receives ultrasonic response |
| D7          | HC-SR04 TRIG           | Sends ultrasonic trigger     |
| D8          | Green LED              | Safe indicator               |
| D9          | Red LED                | Alert indicator              |
| D10         | Buzzer                 | Hardware alert               |
| 5V          | HC-SR04 / Breadboard + | Power                        |
| GND         | Common ground          | Ground                       |


## HC-SR04 Connection

HC-SR04
┌──────────────┐
│ VCC  ────────┼──── Arduino 5V
│ TRIG ────────┼──── Arduino D7
│ ECHO ────────┼──── Arduino D6
│ GND  ────────┼──── Arduino GND
└──────────────┘

## Green LED

The green LED uses a **220 Ω resistor**.

Arduino D8
    │
   220Ω
    │
    ▼
Green LED
    │
    ▼
   GND

## Red LED

The red LED uses a **220 Ω resistor**.

Arduino D9
    │
   220Ω
    │
    ▼
 Red LED
    │
    ▼
   GND

## Buzzer

Buzzer (+) ───────── Arduino D10

Buzzer (-) ───────── Arduino GND


The buzzer is retained as a hardware component and can be used as an optional hardware alert.

The primary FarmGuard animal alert is designed to use a **custom desktop sound** played through the computer.


# 💻 Computer Connections

The system uses two USB connections:


1080P Webcam
     │
     └──────── USB ────────► Computer


Arduino UNO
     │
     └──────── USB ────────► Computer

The Arduino communicates with the FarmGuard desktop application through the Arduino's serial/COM connection.


# 🧠 AI Classification

FarmGuard uses a TensorFlow Lite model for image classification.

### Current model classes

Baffulo
Nature
Porc

The model receives an image from the webcam and produces classification probabilities.

The application uses the configured confidence threshold before making an alert decision.



# 🚨 Detection Logic

The HC-SR04 is configured with a detection distance of: 50 cm


### Safe condition

When:

Distance > 50 cm

FarmGuard remains in the normal monitoring state.

The green LED indicates the safe state.

### Detection condition

When:

Distance ≤ 50 cm

FarmGuard considers an object detected and can activate the camera/AI detection process.

### Animal detection

If the AI classifies the object as:

Baffulo

or

Porc

with sufficient confidence, FarmGuard enters the alert state.

The alert includes:

* Red LED
* Desktop alert sound
* Email notification
* Captured image attachment
* Detection history entry

### Nature detection

If the AI classifies the captured image as:

Nature

the system returns to the safe monitoring state without sending an animal alert.


# 🔊 Desktop Alert Sound

FarmGuard supports a custom alert sound stored inside the project's `assets` directory.

Example:

FarmGuard/
│
├── desktop/
│   ├── assets/
│   │   └── alert.wav
│   │
│   ├── main.py
│   ├── config.py
│   ├── ai.py
│   ├── camera.py
│   ├── arduino.py
│   ├── email_alert.py
│   └── ...


The desktop application plays the sound when an animal alert is generated.

The sound is played through the computer's normal audio output rather than depending on the physical buzzer.


# 📧 Email Alerts

FarmGuard can send an email notification when an animal is detected.

The email can include:

* Detected animal
* AI confidence
* Captured image
  
Email sending is performed in a background thread so that SMTP operations do not block the graphical user interface.


# 🖥️ Desktop Application

The FarmGuard desktop application provides:

### System status

Displays the current overall state of the system.

### Arduino status

Shows whether the Arduino is connected.

### Webcam status

Shows whether the webcam is available.

### Distance

Displays the latest HC-SR04 measurement.

### AI classification

Displays the latest AI result and confidence.

### Live camera

Displays the webcam feed.

### Hardware status

Displays:

* Green LED status
* Red LED status
* Buzzer status

### Detection history

Stores recent detection events in the application interface.


# 🛠️ Software Technologies

The project uses:

* **Python**
* **PySide6**
* **OpenCV**
* **TensorFlow Lite**
* **NumPy**
* **PySerial**
* **Arduino C/C++**
* **SMTP/Gmail**
* **QSoundEffect / Qt audio**
* **VS Code**
* **Arduino IDE**


# 📁 Project Structure

A typical project structure is:

FarmGuard/
│
├── desktop/
│   │
│   ├── assets/
│   │   └── alert.wav
│   │
│   ├── model/
│   │   ├── trained.tflite
│   │   └── labels.txt
│   │
│   ├── captured_images/
│   │
│   ├── main.py
│   ├── config.py
│   ├── ai.py
│   ├── camera.py
│   ├── arduino.py
│   ├── email_alert.py
│   ├── database.py
│   └── ui.py
│
├── arduino/
│   └── FarmGuard.ino
│
├── README.md
└── requirements.txt

# ⚙️ Software Requirements

Recommended environment:

Python 3.10+
Arduino IDE
Arduino UNO
USB webcam
Windows PC

Install the required Python packages:

pip install PySide6
pip install opencv-python
pip install numpy
pip install pyserial
pip install tensorflow

Or install everything from:

pip install -r requirements.txt


# 🔧 Arduino Setup

1. Open the Arduino project in Arduino IDE.
2. Connect the Arduino UNO to the computer.
3. Select:

Tools → Board → Arduino UNO

4. Select the correct COM port.
5. Upload the FarmGuard Arduino sketch.
6. Keep the Arduino connected through USB.

The Arduino provides the serial connection required by the FarmGuard desktop application.


# ▶️ Running FarmGuard

Open the desktop project:

cd desktop

Then run:

python main.py

The FarmGuard desktop application should open.

The system will initialize:

1. Desktop interface
2. AI model
3. Email service
4. Arduino connection
5. Webcam
6. Monitoring system


# 🧪 Hardware Testing

Before running the complete FarmGuard application, test the Arduino hardware independently.

The hardware test should confirm:

### No nearby object

Green LED → ON
Red LED   → OFF
Buzzer    → OFF

### Object within 50 cm

Green LED → OFF
Red LED   → ON
Buzzer    → ON

The Serial Monitor should display distance measurements such as:

Distance: 80.2 cm | SAFE
Distance: 42.5 cm | OBJECT DETECTED
Distance: 25.1 cm | OBJECT DETECTED

# 🧪 AI Testing

The AI model should be tested using images representing the three supported classes:


Baffulo
Porc
Nature


The desktop application should display the predicted class and confidence.

Example:

AI RESULT

BAFFULO

Confidence: 99.61%

For an animal detection, the system should enter the alert state.


# 🔐 Configuration

Configuration values such as:

* Application name
* Application version
* Camera index
* Camera resolution
* Detection threshold
* Capture directory
* Maximum history items
* Arduino COM port
* AI confidence threshold

should be maintained in the configuration file rather than hard-coded throughout the application.


# 📸 Captured Images

FarmGuard stores captured images using date-based folders.

Example:

captured_images/
└── farmguard_20260926/
    ├── farmguard_190201_123456.jpg
    ├── farmguard_190214_789123.jpg
    └── farmguard_190228_456789.jpg

This makes detection images easier to organize and review.


# 🔄 Complete Detection Example

Suppose an animal approaches the protected area.

### Step 1

HC-SR04 measures: 42 cm


### Step 2

Arduino reports the detection to FarmGuard.

### Step 3

FarmGuard activates the webcam capture process.

### Step 4

The image is sent to the AI model.

### Step 5

AI returns:

Baffulo
99.61%


### Step 6

FarmGuard enters ALERT state.

Red LED       → ON
Green LED     → OFF
Desktop sound → PLAY

### Step 7

FarmGuard sends an email notification with the captured image.

### Step 8

The event is added to the detection history.


# 🎯 Project Goals

FarmGuard is intended to demonstrate how several technologies can work together in a practical agricultural application:

IoT
+
Computer Vision
+
Machine Learning
+
Desktop Application
+
Email Communication


The project demonstrates an approach where physical sensing is used to trigger intelligent image-based classification.


# 🚀 Future Improvements

Possible future improvements include:

* Solar-powered deployment
* Outdoor weatherproof enclosure
* Wireless Arduino communication
* Multiple ultrasonic sensors
* Multiple cameras
* Improved AI model accuracy
* Larger animal image datasets
* Mobile notifications
* Cloud-based detection history
* Remote monitoring
* GPS-enabled deployment
* Low-power operation
* Automated farm-zone monitoring


# 👨‍💻 Development

FarmGuard is developed as a practical AI + IoT agricultural protection prototype.

The project combines embedded hardware, computer vision, machine learning, desktop software, and communication services into a single integrated system.


# 📜 License

Add your preferred license here.

For example:

MIT License

if you decide to release the project under the MIT License.


# 🌱 FarmGuard

**AI-Powered Smart Agricultural Protection System**

Sense → Detect → Classify → Alert → Notify

Built to explore practical applications of **AI, IoT, and computer vision in agriculture**.
