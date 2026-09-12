# ESP32-CAM Telegram Intrusion Detection System

> **An IoT-based security system that detects motion using a PIR sensor, captures images with an ESP32-CAM, activates a local buzzer, and sends intrusion alerts to an authorized Telegram user.**

![Platform](https://img.shields.io/badge/Platform-ESP32--CAM-blue)
![IoT](https://img.shields.io/badge/IoT-Telegram%20Bot-green)
![Communication](https://img.shields.io/badge/Communication-Wi--Fi-orange)
![Language](https://img.shields.io/badge/Language-C%2FC%2B%2B-red)

---

## 📌 Overview

This project is an **IoT-based intrusion detection and remote surveillance system** built using an **ESP32-CAM, PIR motion sensor, buzzer, and Telegram Bot API**.

When motion is detected, the system:

* Detects movement using a PIR sensor
* Activates a local buzzer
* Captures an image using the ESP32-CAM
* Sends the image to Telegram
* Allows remote control through Telegram commands
* Restricts bot access using an authorized Chat ID

The ESP32-CAM communicates with the Telegram Bot API through Wi-Fi using a secure TLS connection.

---

## 🏗️ System Architecture

```text
             ┌─────────────────────┐
             │    PIR Sensor       │
             │      GPIO 13        │
             └──────────┬──────────┘
                        │
                  Motion Detected
                        │
                        ▼
             ┌─────────────────────┐
             │     ESP32-CAM       │
             │     Controller      │
             └───────┬─────┬───────┘
                     │     │
              ┌──────┘     └──────────┐
              ▼                       ▼
        ┌──────────┐            ┌───────────┐
        │  Buzzer  │            │  Camera   │
        │ GPIO 12  │            │  Capture  │
        └──────────┘            └─────┬─────┘
                                      │
                                  JPEG Image
                                      │
                                      ▼
                                Wi-Fi / TLS
                                      │
                                      ▼
                              ┌──────────────┐
                              │ Telegram Bot │
                              └──────┬───────┘
                                     │
                                     ▼
                              📱 User Alert
```

---

## ⚙️ How It Works

```text
System Start
     ↓
Connect to Wi-Fi
     ↓
Initialize ESP32-CAM
     ↓
Enable Motion Monitoring
     ↓
Monitor PIR Sensor
     ↓
Motion Detected?
   ┌───┴───┐
  No      Yes
  │        │
  │        ▼
  │    Buzzer ON
  │        │
  │        ▼
  │   Capture Image
  │        │
  │        ▼
  │  Send Image to
  │    Telegram
  │        │
  │        ▼
  │    Buzzer OFF
  │
  └── Continue
```

## When `/motionOn` is enabled, the ESP32 continuously checks the PIR sensor. A detected motion event triggers the buzzer and image transmission.

## 🔔 Telegram Commands

| Command           | Function                   |
| ----------------- | -------------------------- |
| `/start`          | Display available commands |
| `/photo`          | Capture and send a photo   |
| `/photoWithFlash` | Capture photo with flash   |
| `/motionOn`       | Enable motion detection    |
| `/motionOff`      | Disable motion detection   |

The bot command interface is implemented directly in the ESP32 firmware.

---

## 🔐 Security

The system implements basic **Telegram user authorization**.

Only the configured Chat ID is allowed to control the device. Messages from other Chat IDs receive an `Unauthorized user` response.


Use placeholders such as:

```cpp
const char* ssid = "YOUR_WIFI";
const char* password = "YOUR_PASSWORD";

String chatId = "YOUR_CHAT_ID";
String BOTtoken = "YOUR_BOT_TOKEN";
```

---

## 📷 Image Capture

The ESP32-CAM captures JPEG images using the `esp_camera` library.

Two capture modes are available:

```text
/photo
     ↓
Normal Image Capture

/photoWithFlash
     ↓
Flash ON
     ↓
Image Capture
     ↓
Flash OFF
```

The captured image is uploaded to Telegram using the Bot API `/sendPhoto` endpoint over port 443.

---

## 🔌 Hardware

| Component  | Purpose             |    GPIO |
| ---------- | ------------------- | ------: |
| ESP32-CAM  | Controller + Camera |       — |
| PIR Sensor | Motion Detection    | GPIO 13 |
| Buzzer     | Local Alert         | GPIO 12 |
| Flash LED  | Camera Illumination |  GPIO 4 |

The camera is configured for the **AI-Thinker ESP32-CAM** module.

---

## 💻 Technologies Used

**Hardware**

* ESP32-CAM AI-Thinker
* PIR Motion Sensor
* Buzzer
* Camera Flash LED

**Software**

* Embedded C/C++
* Arduino Framework
* ESP32 Camera Library
* Wi-Fi
* HTTPS/TLS
* Telegram Bot API

---

## 🚀 Getting Started

### 1. Install Arduino IDE

Install Arduino IDE with ESP32 board support.

### 2. Install Libraries

Install:

```text
UniversalTelegramBot
ArduinoJson
```

### 3. Configure Credentials

Update the Wi-Fi and Telegram credentials in the source code.

### 4. Upload Firmware

Connect the ESP32-CAM to your computer using a suitable programmer and upload the sketch.

### 5. Start the Telegram Bot

Send:

```text
/start
```

Then enable monitoring:

```text
/motionOn
```

The system is now ready to detect motion and send image alerts.

---

## 🧪 Testing

| Test                 | Expected Result            |
| -------------------- | -------------------------- |
| `/photo`             | Photo sent to Telegram     |
| `/photoWithFlash`    | Photo captured with flash  |
| `/motionOn`          | Motion monitoring enabled  |
| PIR detects motion   | Buzzer + photo alert       |
| `/motionOff`         | Motion monitoring disabled |
| Unauthorized Chat ID | Access denied              |

---

## 📊 Key Features

* 📷 **Remote Image Capture**
* 🚨 **PIR-Based Intrusion Detection**
* 🔔 **Local Buzzer Alert**
* 📱 **Telegram Security Notifications**
* 🔐 **Chat ID-Based Access Control**
* 📡 **Wi-Fi Connectivity**
* 🔒 **TLS Communication**
* 💡 **Flash-Assisted Image Capture**

---

## ⚠️ Important Note

This project uses **PIR-based motion detection**. It detects movement and treats it as a potential intrusion.

It does **not** currently implement:

* Face recognition
* AI-based object detection
* Machine-learning theft classification
* Real-time video streaming
* Cloud image storage

Therefore, the system is best described as an **IoT-based intrusion detection and remote surveillance prototype**.

---

## 🔮 Future Improvements

* AI-based person detection
* Face recognition
* SD-card image storage
* Timestamped intrusion logs
* Multiple authorized users
* Event cooldown/debouncing
* Real-time video streaming
* Cloud image storage
* Additional door/window sensors
* Battery monitoring

---

## 📁 Project Structure

```text
ESP32-CAM-Telegram-Intrusion-Detection/
│
├── ESP32-CAM-Telegram-Intrusion-Detection.ino
├── README.md
├── images/
│   ├── hardware.jpg
│   ├── wiring.jpg
│   └── telegram-alert.jpg
└── docs/
    └── system-diagram.png
```

---

## 🎯 Applications

* Home security
* Room monitoring
* Restricted-area monitoring
* Small office surveillance
* Laboratory/equipment protection
* Remote intrusion monitoring

---

## 👨‍💻 Author

**Ramachandru J**
SRM Institute of Science and Technology

---

## ⭐ Project Summary

> **ESP32-CAM + PIR + Buzzer + Telegram = Remote IoT Intrusion Detection**

A compact embedded security system that combines **motion sensing, camera-based evidence capture, local alarm generation, and remote Telegram notifications**.
