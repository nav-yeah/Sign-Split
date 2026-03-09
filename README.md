# SignSplit – Dual-Path ASL Recognition Framework with motion-based routing between specialized static and temporal models.

A real-time **American Sign Language (ASL) recognition system** that classifies **250 isolated signs** using MediaPipe hand landmarks and deep learning models.  
The system separates **static and dynamic gestures** using a motion-based routing mechanism and maintains a **lightweight model footprint (<25 MB)** for efficient deployment.

---

## Project Overview

Sign language recognition systems can improve accessibility and communication for the **Deaf and Hard-of-Hearing community**.

This project implements an **isolated ASL sign recognizer** trained on the **Google Isolated Sign Language Recognition dataset**, which contains **~95,000 sign videos** performed by **21 Deaf signers** across a **250-sign vocabulary**.

Static and dynamic gestures require different feature representations.  
To address this, SignSplit introduces a **dual-path architecture** that routes inputs to specialized models depending on motion patterns.

---

## Demo (Coming Soon)

This section will include:

- Real-time ASL recognition demo GIF  
- System architecture diagram  
- Example prediction outputs with confidence scores  

---

## Key Features

- Supports **250 ASL gestures**
- Handles both **static and dynamic signs**
- Uses **MediaPipe hand landmark features**
- **Motion-based routing** between specialized models
- Real-time webcam inference pipeline
- **Model size <25 MB** for lightweight deployment

---

## System Architecture

The recognition pipeline consists of the following stages:

1. **Input Capture**
   - Webcam frames captured using OpenCV.

2. **Feature Extraction**
   - MediaPipe extracts **hand landmark coordinates**.

3. **Motion Router**
   - Maintains a **30-frame rolling buffer** of landmarks.
   - Computes frame-to-frame displacement to estimate motion.

4. **Model Routing**
   - Low motion → Static Gesture Model  
   - High motion → Dynamic Gesture Model  

5. **Prediction Layer**
   - Outputs gesture predictions with confidence scores.
   - Prediction smoothing improves stability during real-time inference.

---

## Approach

Instead of training a single model for all gestures, the system follows a **mixture-of-experts style architecture**:

- A **motion-based router** determines whether a gesture is static or dynamic.
- The input is then dispatched to a specialized model for classification.

This approach improves efficiency and prevents a single model from needing to learn both **spatial and temporal gesture patterns simultaneously**.

---

## Models

### Static Gesture Model

Used for gestures with minimal motion.

Architecture experiments included:

- Multi-Layer Perceptron (MLP)
- Residual MLP
- 1D Convolutional Networks

**Performance**
- ~60% validation accuracy

---

### Dynamic Gesture Model

Handles gestures involving temporal motion.

Architecture:

Temporal CNN with stacked 1D convolutions:

512 → 256 → 128 channels

**Performance**

- ~40% validation accuracy  
- Significantly higher than the ~0.8% random baseline

---

## Dataset

This project uses the **Google Isolated Sign Language Recognition dataset** from Kaggle.

Dataset characteristics:

- ~95,000 sign videos
- 250 ASL sign vocabulary
- 21 Deaf signers
- Landmark features extracted using **MediaPipe Holistic**
- Each frame contains **543 landmarks (x, y, z)**

Dataset Source:  
https://www.kaggle.com/competitions/asl-signs

The dataset was created to support **PopSign**, an educational game designed to help parents of Deaf children learn ASL vocabulary.

---

## Tech Stack

- Python  
- PyTorch  
- OpenCV  
- MediaPipe  
- NumPy  

---

## Limitations

- Landmark-based models are sensitive to **tracking errors**
- Reduced accuracy under **poor lighting conditions**
- Dynamic gestures with subtle motion are harder to classify
- Currently uses **only hand landmarks**

---

## Future Improvements

- CNN-based **image models for static gestures**
- **Transformer-based temporal models**
- Multi-modal fusion (**hand + face landmarks**)
- Dataset expansion with varied lighting conditions
- Sentence-level **continuous sign recognition**

---

## Citation

If you use this dataset, please cite:

Ashley Chow et al.  
**Google – Isolated Sign Language Recognition**, Kaggle 2023  
https://kaggle.com/competitions/asl-signs
