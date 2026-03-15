# 🚦 Adaptive Traffic Control System for Dhaka City 🚦

*An end-to-end AI-driven approach to solving urban gridlock using Computer Vision (YOLOv9) and Reinforcement Learning (Deep Q-Networks).*

---

## 🛑 The Problem: Static Timers vs. Dynamic Traffic
Traffic gridlock in Dhaka city is a compounding crisis, largely exacerbated by an outdated, static traffic management system. 

Currently, intersections rely on **fixed-time traffic lights**. A light may stay green for 60 seconds for the North lane, even if that lane is completely empty, while 100 vehicles idle at a red light in the crossing street. This blind, mathematical inefficiency causes:
* 🚗 **Cascading Gridlock:** Artificial traffic jams created purely by bad timing rather than actual road capacity.
* 🚑 **Emergency Delays:** Ambulances and fire trucks are forced to wait in static queues, leading to potentially fatal delays.
* 📉 **Economic Loss:** Millions of working hours and fuel are burned daily due to vehicles idling at empty intersections.

---

## 💡 The Solution (Project Overview)
This project replaces static, blind timers with an intelligent, dynamic agent capable of "seeing" the intersection and optimizing traffic flow mathematically. 

The system is split into two primary modules:
1. 👁️ **The Perception Module (Eyes):** A YOLOv9 Convolutional Neural Network trained to detect and count local vehicles in real-time.
2. 🧠 **The Decision Module (Brain):** A PyTorch-based Deep Q-Network (DQN) that takes vehicle counts as a state vector and calculates the optimal traffic light switches to minimize wait times and prioritize emergency vehicles.

---

## 🛠️ Tech Stack & Libraries
* 📷 **Computer Vision:** YOLOv9, OpenCV, Bounding Box Mathematics
* 🤖 **Reinforcement Learning:** Deep Q-Networks (DQN), PyTorch, Gymnasium
* 💻 **Data Processing:** Python, NumPy, Matplotlib, Custom XML Parsing

---

## 🔬 Methodology

### 📸 Phase 1: Computer Vision (YOLOv9)
* 🗂️ **Dataset:** Utilized the Dhaka-AI dataset featuring 21 unique vehicle classes specific to Bangladesh (e.g., CNGs, human haulers, rickshaws).
* 🧹 **Data Engineering:** Developed a custom Python parser to clean corrupted Pascal VOC XML files and mathematically convert bounding boxes into normalized YOLO format.
* ⚖️ **Data Split:** Enforced a rigorous 80/20 Train/Validation split (1,819 training images, 455 validation images) to prevent data leakage.
* 🏋️‍♂️ **Training:** Fine-tuned the pre-trained YOLOv9 compact model for 20 epochs using Dual T4 GPUs.

### 🎯 Phase 2: Reinforcement Learning (DQN)
* 🏙️ **Custom Environment:** Engineered a custom mock 4-way intersection using the `Gymnasium` library to simulate Dhaka's traffic physics.
* 🔢 **State Space:** A continuous array containing vehicle counts for North, South, East, West lanes, and an Ambulance presence indicator.
* 🚥 **Action Space:** Discrete binary actions allowing the AI to choose between North/South Green or East/West Green.
* 🏆 **Reward Function:** * **+1 point** for every car that successfully passes the intersection.
  * **-0.5 point** penalty for every car forced to idle in the queue.
  * **-50 point** massive penalty if an ambulance is waiting, forcing the AI to develop an emergency priority override.
* 🔁 **Training:** Trained the DQN agent over 500 episodes using an Epsilon-Greedy exploration strategy, a replay memory buffer, and the Bellman Equation for target Q-value updates.

---

## 📊 Results & Performance

### 🎯 YOLOv9 Object Detection Metrics
Despite high class imbalances and low-light glare in the dataset, the model achieved strong detection accuracy on Dhaka's most common vehicles:
* 🌟 **Overall mAP50:** 0.474
* 🛺 **Three Wheelers (CNG):** 0.822 mAP50
* 🚌 **Bus:** 0.805 mAP50
* 🚗 **Car:** 0.788 mAP50

### ⏱️ RL Agent Performance
The trained RL agent dramatically outperformed a baseline static traffic timer during the inference testing phase:
* 🛣️ **Gridlock Prevention:** The agent successfully developed "lane-clearing" strategies, actively responding to dynamic traffic build-ups and consistently keeping total waiting cars near zero. 
* 📈 **Convergence:** The AI stabilized its learning curve significantly over the 500 episodes, finishing with an optimized average score that proved a **50%+ reduction** in overall intersection waiting times compared to random/static light switching.

---

## 🚀 Future Scope
* 🌌 **GAN Integration:** Implement Generative Adversarial Networks to denoise and enhance low-light CCTV camera feeds prior to YOLO inference.
* 🏙️ **Microscopic Simulation:** Bridge the Python RL agent directly into the **SUMO** (Simulation of Urban MObility) GUI via the TraCI API for physics-based, localized traffic modeling.
