# SAM-IPC
This project implements an interactive image segmentation system using Meta’s Segment Anything Model (SAM). The system is designed to run SAM as a Python process, which communicates with a C++ parent process via inter-process communication (IPC) using shared memory (mmap) and event synchronization (Windows API).
