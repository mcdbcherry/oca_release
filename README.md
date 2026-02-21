# OCA (Object-oriented Control Architecture)
<img src="oca_win.png">

OCA is a modern Software controller (not SoftPLC) / industrial automation runtime written in C++20.
It provides deterministic real-time control on Linux/Windows and ARM devices.
Targets include Siemens IOT2050, Raspberry Pi, and general-purpose x64 hardware.

### Closing the 40-Year Gap in Industrial Automation

<img src="oca_logo.png" width="200">

## 🚀 Toward the Democratization of Automation Control

**OCA challenges the 40-year stagnation of industrial control.**

For decades, the industry has been held hostage by proprietary hardware and outdated programming paradigms (IEC 61131-3). We believe that **General Purpose OS (Windows/Linux) + Modern C++** can outperform dedicated hardware PLCs when architected correctly.

OCA is not just a "SoftPLC". It is a **deterministic control kernel** built from scratch with **C++20**, designed to prove that software engineering can reclaim the factory floor.

---

## 🏆 Key Performance Metrics

The following performance has been verified on commercial off-the-shelf (COTS) hardware:

| Metric | Performance | Hardware | OS |
| :--- | :--- | :--- | :--- |
| **Cycle Time** | **3.6 µs** (stable) | Siemens SIMATIC IOT2050 | TI AM65x (RT Linux) |
| **Cycle Time** | **10.0 µs** (stable) | Raspberry Pi Zero 2 W | Yocto Linux |
| **Jitter** | < 1.0 µs | - | - |
| **Runtime Allocation** | **0 bytes** | All Platforms | No GC, No `new`/`malloc` |

> *"99% of 'PC-based control' projects fail due to Garbage Collection pauses. OCA solves this by enforcing a strict Zero-Allocation architecture during the runtime cycle."*


---

## 💎 Core Architecture

### 1. True Object-Oriented Logic
Breaking away from the "Copy & Paste" culture of Function Blocks (FB).
- **Inheritance & Polymorphism:** Create a base class `Conveyor` and derive `ConveyorWithSensor`. Modify the base, and all 100 instances update instantly.
- **Vendor Compatibility:** Includes wrapper libraries for **Siemens (P_TRIG, TON)** and **Mitsubishi (PLS, SET)** to bridge the gap for traditional OT engineers.

### 2. Modern DevOps Ecosystem
- **VS Code Integration:** Full-featured extension for Windows.
- **Cross-Compilation:** Docker-based build pipeline hidden behind a single "Build" button.
- **One-Click Deploy:** Automated SCP transfer, permission setting, and service restart via SSH.

---

## 🛠️ Supported Platforms

**Development Environment:**
- Windows 10/11 (Visual Studio Code)

**Runtime Targets:**
- **x64 Windows:** For development and simulation.
- **x64 Linux:** Ubuntu / Debian / RedHat.
- **ARM64 / ARMv7:** Siemens IOT2050, Raspberry Pi 3/4/5/Zero2W (via Yocto/Debian).

---

## 📊 Jitter Analysis in Real-World PLC Control

**Test Condition:**
In this test, OCA commands an external PLC over a general-purpose network (TCP/IP) to toggle an output ON and OFF at a strict 1-second (1000ms) interval. The target PLC logs the actual timing of these state changes, and the resulting data is summarized below.

**Technical Evaluation:**
While the OCA kernel itself operates stably at a cycle time of under **10µs**, the observed jitter (approx. ±50ms) is a physical constraint entirely originating from standard TCP/IP communication delays and the target PLC's own scan time fluctuations. 

Against the 1000ms expected interval, the average interval achieved was **999.347ms** (an astonishing average error of less than 1ms). There are absolutely no unpredictable delays, system freezes, or panic pauses caused by garbage collection or OS interrupts. This data clearly proves that OCA's architecture fundamentally eliminates sources of unpredictable latency, ensuring it operates in a strictly **deterministic** manner on a general-purpose OS.

<img src="jitter_analysis_results_20260221.png">

---

## 📦 Getting Started

Please download the VS Code extension and experience developing control logic.
<img src="VSCode.png">
---

## 💡 Tips for Creating User Logic

The actual control processing is executed within `OnExecute()`.

### 1. NetworkBase-derived class instances are persistent

The implementation classes of user logic are instantiated at startup, and their `OnExecute()` methods are called repeatedly. Unlike typical event-driven programming, understand that control logic is characterized by being called repeatedly at high frequency.

### 2. Avoid using `new`, `malloc`, and other memory allocation

Use buffer areas created in the constructor via member variables within `OnExecute()`, or use tags configured as Internal tags to reduce memory allocation overhead. When securing buffers with Internal tag settings, you can use BYTE array declarations (e.g., `BYTE[10]`; other types like Word cannot be used in array declarations).

### 3. Avoid executing high-overhead processing in `OnExecute()`

One scan consists of all `OnExecute()` methods implemented in user logic being called. Even for controls where real-time performance is not critical (such as conveyors), typical hardware controllers have scan cycle times in the range of several milliseconds. Systems with several thousand steps of ladder logic may consider it a control error if the total exceeds 150ms. Always strive to minimize processing time in your `OnExecute()` implementation.

---

## ⚠️ License & Proprietary Notice

**Copyright (c) 2016-2026 Satoshi Murakami. All Rights Reserved.**

This source code and related documentation are the **proprietary property** of Satoshi Murakami.
Unauthorized copying, distribution, modification, or use of this file, via any medium, is strictly prohibited.

* **Non-Commercial Use:** Free for personal evaluation and educational purposes.
* **Commercial Use:** Requires a commercial license.
* **Asset Acquisition:** For inquiries regarding technology transfer or IP acquisition, please contact the author directly.

**Strictly Confidential.**
