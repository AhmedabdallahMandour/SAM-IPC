
# Image Segmentation Using SAM with IPC (Python & C++)  

# SAM-IPC 
This project implements an **interactive image segmentation system** using **Meta’s Segment Anything Model (SAM)**. The system runs **SAM as a Python process**, which communicates with a **C++ parent process** via **inter-process communication (IPC)** using **shared memory (mmap) and event synchronization (Windows API)**.  

## Key Features  
- **Image Segmentation with SAM**  
  - Uses **MobileSAM (lightweight SAM model)** with **CUDA support (if available)**.  
  - Accepts **user-defined points and labels** for segmentation.  

- **Inter-Process Communication (IPC) using Shared Memory**  
  - **mmap (shared memory)** for exchanging images, point coordinates, and labels.  
  - **Windows Events** for synchronization between C++ and Python.  

- **Base64 Image Encoding for Data Exchange**  
  - Converts images to **Base64 format** before storing in shared memory.  
  - Decodes images on the receiving side.  

- **Real-Time Mask Generation**  
  - SAM generates a **binary segmentation mask** (white = object, black = background).  
  - Mask is **Base64-encoded** and sent back via shared memory.  

- **Robust Error Handling**  
  - Detects **corrupt or empty images** in shared memory and resets it.  
  - Handles **process termination** safely by releasing system resources.  

## Workflow  
1. **C++ Process**  
   - Captures an image and encodes it as **Base64**.  
   - Stores **Base64-encoded image and user-defined points** in shared memory.  
   - Triggers an **event** to signal the Python process.  

2. **Python Process (SAM)**  
   - Waits for the event from C++.  
   - Reads **Base64 image** and **decodes** it.  
   - Reads **user-defined points & labels** from shared memory.  
   - Runs **SAM inference** to generate a segmentation mask.  
   - **Encodes the mask** as a **Base64 image** and writes it back to shared memory.  
   - Triggers an **event** to signal the C++ process.  

3. **C++ Process (Receiving Segmentation Mask)**  
   - Reads the **Base64-encoded segmentation mask** from shared memory.  
   - **Decodes and displays/saves** the segmented result.  

## Technologies Used  
- **Deep Learning Model:** MobileSAM (Segment Anything Model - SAM)  
- **Programming Languages:** Python (for SAM), C++ (for IPC & GUI)  
- **Inter-Process Communication (IPC):** mmap (shared memory), Windows Events  
- **Image Processing:** OpenCV, NumPy, Pillow  
- **Base64 Encoding:** Used for transmitting images through shared memory  

## Potential Applications  
- **Robotics & Automation:** Object detection & segmentation for robots.  
- **Medical Imaging:** Automated image segmentation for X-rays, MRIs, etc.  
- **Autonomous Vehicles:** Road and object segmentation.  
- **Security & Surveillance:** Object/person tracking in security footage.  

## Installation & Setup  
### **Prerequisites**  
- **Python 3.8+**  
- **C++ Compiler (MinGW/GCC/Visual Studio C++)**  
- **CUDA (Optional for GPU Acceleration)**  
- **Required Python Libraries:**  
  ```bash
  pip install torch torchvision torchaudio opencv-python numpy pillow
