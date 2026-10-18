# Real-Time Object Detection and Tracking System

A modern, responsive web application for real-time multi-object detection and tracking built using **React, Vite, Python, Flask, OpenCV, YOLOv8, and SORT (Simple Online and Realtime Tracking)**.

---

## 🌟 Key Features

- **Pre-trained YOLOv8 Model**: Uses pre-trained `yolov8n.pt` for real-time multi-class object detection across 80 COCO categories (pedestrians, vehicles, animals, accessories, etc.).
- **SORT & ByteTrack Object Tracking**:
  - **SORT (Simple Online and Realtime Tracking)**: Self-contained Kalman Filter state estimation combined with Hungarian matching (`scipy.optimize.linear_sum_assignment`) for persistent unique IDs across video frames.
  - **ByteTrack**: High-precision multi-object tracking with low-confidence association support.
- **Dual Stream Input Modes**:
  - **Live Webcam**: Real-time camera feed via browser MediaDevices WebRTC or direct server OpenCV VideoCapture.
  - **Video Upload**: Drag-and-drop or browse custom video files (`.mp4`, `.avi`, `.mov`, `.mkv`, `.webm`) with format validation.
  - **Built-in Sample Demo**: Instant multi-agent traffic and pedestrian test video with motion panning for zero-setup demonstration.
- **Modern Cyber Dark Dashboard**:
  - Live video preview with cyber reticle corners, HUD scanlines, and animated status badges.
  - Bounding boxes with distinctive color coding per tracking ID.
  - Confidence score percentages & class labels.
  - Motion trajectory trails tracking object centroid paths over time.
  - Real-time FPS, inference latency (ms), active objects count, and cumulative unique tracked IDs.
  - Object count breakdown by class with animated progress bars.
  - Searchable, filterable live telemetry log table with **CSV export**.
  - Frame snapshot capture to download annotated detection frames.

---

## 📁 System Architecture

```
AI DETESION/
├── backend/
│   ├── app.py                      # Flask REST & MJPEG streaming server
│   ├── streamer.py                 # Multi-source stream manager (Webcam, File, Browser)
│   ├── sample_generator.py         # Test video generator
│   ├── requirements.txt            # Python dependencies
│   ├── detector/
│   │   └── yolo_detector.py        # YOLOv8 inference, visual overlays & HUD
│   ├── tracker/
│   │   └── sort.py                 # Pure Python Kalman Filter + Hungarian SORT tracker
│   ├── sample_videos/              # Sample test video storage
│   └── uploads/                    # Directory for user-uploaded videos
├── frontend/
│   ├── index.html                  # HTML5 entry with Outfit & JetBrains Mono typography
│   ├── vite.config.js              # Vite config with backend API reverse proxy
│   ├── package.json                # React dependencies (Lucide icons, etc.)
│   └── src/
│       ├── App.jsx                 # Main dashboard layout & telemetry sync
│       ├── index.css               # Glassmorphic cyber dark design system
│       └── components/
│           ├── Navbar.jsx          # Brand header, online status & FPS badge
│           ├── MetricsCards.jsx    # Real-time metrics (FPS, Latency, Active, Total IDs)
│           ├── ControlPanel.jsx    # Start/Stop, Upload, Sliders, Overlays, Filters
│           ├── VideoPlayer.jsx     # MJPEG stream player & WebRTC webcam canvas
│           ├── CategoryBreakdown.jsx# Counts by class with visual progress bars
│           ├── DetectionLog.jsx    # Live tracking log with search & CSV export
│           └── UploadModal.jsx     # Video upload modal with drag-and-drop validation
├── run.bat                         # 1-Click launcher (Production build + Flask)
├── start_dev.bat                   # Development launcher (Vite HMR + Flask)
└── README.md                       # Complete documentation
```

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- **Python 3.10+** (Python 3.12 verified)
- **Node.js v18+** & **npm**

### 2. Fast Launch (Windows)
Double-click `run.bat` or execute in PowerShell:
```powershell
.\run.bat
```
This automatically verifies dependencies, starts the Flask server, and opens your default browser at `http://127.0.0.1:5000`.

---

### 3. Manual Step-by-Step Setup

#### A. Backend Setup
1. Open PowerShell / Command Prompt in the project folder:
   ```bash
   pip install -r backend/requirements.txt
   ```
2. Start the Flask server:
   ```bash
   python -m backend.app
   ```
   The backend API will run on `http://127.0.0.1:5000`.

#### B. Frontend Setup
1. Open a second terminal and navigate to the `frontend` folder:
   ```bash
   cd frontend
   npm install
   ```
2. Start the Vite development server:
   ```bash
   npm run dev
   ```
   The development frontend will run at `http://localhost:5173` with automatic reverse proxying to `http://127.0.0.1:5000/api`.

---

## 🔌 API Endpoints Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/status` | Returns backend health, model name, device (GPU/CPU), and 80 COCO classes |
| `POST` | `/api/start` | Starts video/webcam stream with tracker, confidence, and visual options |
| `POST` | `/api/stop` | Stops the active video stream |
| `GET` | `/api/video_feed` | MJPEG stream (`multipart/x-mixed-replace; boundary=frame`) |
| `POST` | `/api/process_frame` | Processes a single base64 frame from browser webcam and returns annotated image + tracking JSON |
| `GET` | `/api/stats` | Fetches live telemetry: FPS, inference time ms, counts by class, active IDs |
| `POST` | `/api/update_config` | Dynamically updates confidence threshold, IoU, and visual overlays on the fly |
| `POST` | `/api/upload` | Uploads and validates video files (`.mp4`, `.avi`, `.mov`, `.mkv`, `.webm`) |
| `POST` | `/api/reset_tracks` | Resets persistent tracking IDs and counter cache |

---

## 🛡️ Error Handling & Permissions

- **Webcam Permissions**: In **Browser Camera** mode, the application uses HTML5 `navigator.mediaDevices.getUserMedia`. If permissions are blocked or denied, a clear notification banner is shown guiding the user to grant camera permissions in the browser address bar.
- **No Webcam Available**: When no physical camera is detected, the app displays a clear prompt allowing users to seamlessly switch to an uploaded video or the built-in sample demo video.
- **Corrupt / Invalid Video Files**: Video uploads are validated for allowed file extensions (`mp4`, `avi`, `mov`, `mkv`, `webm`), file size limit (250MB), and frame decodability via OpenCV `cv2.VideoCapture` before being accepted.
"# AI-detection" 
