# AI Gym Coach 🏋️‍♂️

A real-time AI-powered fitness application that monitors your form, counts your repetitions, and provides interactive, proactive voice coaching cues using computer vision and LLM feedback.

---

## 🌟 Key Features

* **Real-time Pose Detection**: Leverages **MediaPipe Pose Landmarker** to extract and evaluate skeletal joint angles.
* **Form & Alignment Diagnostics**:
  * **Squats**: Detects depth (thighs parallel to the ground) and forward trunk lean.
  * **Push-ups**: Checks elbow flexion, overall body line alignment, and hip sag or pike.
  * **Biceps Curls**: Measures elbow angle and flags excessive torso swinging.
  * **Shoulder Press**: Tracks elbow extension and flags hyper-extension or lower back arching.
  * **Lunges**: Monitors knee angles and checks lateral offset for balance issues.
* **Interactive AI Voice Coaching**: Synthesizes custom, high-energy voice corrections in real-time utilizing **Groq (Llama 3.3)** and **gTTS** (Google Text-to-Speech).
* **Local Session Tracking**: Automatically logs completed reps, sets, and active workout times to a local **SQLite database** to map history.
* **Premium Dark Mode Interface**: Built with custom Streamlit styling and font rendering for a modern, high-contrast dashboard layout.
* **Product Landing Page**: Includes an elegant web introduction page (`LandingPage/index.html`) built with rich glassmorphism hover interactions, skeletal overlays, and latency statistics.

---

## 📁 Repository Structure

```
ai-gym-coach-main/
├── LandingPage/          # Frontend Web Presentation
│   ├── index.html        # Landing page structure
│   └── style.css         # Visual styles (grid overlays, scanlines)
├── MainApp/              # Core Streamlit Application
│   ├── main.py           # Streamlit orchestrator and UI view
│   ├── requirements.txt  # Python package dependencies
│   ├── packages.txt      # Debian package dependencies (for deployment)
│   ├── core/
│   │   └── base_exercise.py          # Geometric calculation helpers
│   ├── detectors/
│   │   ├── squat.py                  # Squat detector
│   │   ├── pushup.py                 # Push-up detector
│   │   ├── biceps_curl.py            # Biceps curl detector
│   │   ├── shoulder_press.py         # Shoulder press detector
│   │   └── lunges.py                 # Lunge detector
│   ├── ml_models/
│   │   └── pose_landmarker_full.task # MediaPipe model asset
│   └── services/
│       ├── auth/
│       │   └── login_wall.py         # Authentication layout
│       ├── coaching/
│       │   ├── llm.py                # Groq client controller
│       │   ├── tts.py                # Text-to-speech buffer helper
│       │   └── voice_pipeline.py     # Voice event orchestration
│       ├── config/
│       │   └── workout_config.py     # System instructions and boundaries
│       ├── persistence/
│       │   └── exercise_repository.py# SQLite database layer
│       ├── state/
│       │   └── session_defaults.py   # State managers
│       ├── tracking/
│       │   └── metrics.py            # Real-time WebRTC thread syncs
│       ├── ui/
│       │   └── style_loader.py       # Custom WebRTC iframe patcher
│       └── vision/
│           └── exercise_video_processor.py # MediaPipe camera frames processor
└── README.md
```

---

## 🚀 Setup & Installation

### 1. Prerequisites
Ensure you have **Python 3.10 or newer** installed on your system.

### 2. Clone and Install Dependencies
Navigate to the `MainApp` directory and install the necessary libraries:
```bash
cd MainApp
pip install -r requirements.txt
```

### 3. API Key Configuration
To enable the interactive voice feedback, obtain a Groq API Key and set it as an environment variable:
```bash
# Windows (cmd)
set GROQ_API_KEY="your-api-key-here"

# Windows (PowerShell)
$env:GROQ_API_KEY="your-api-key-here"

# Linux/macOS
export GROQ_API_KEY="your-api-key-here"
```
*Note: If no environment variable is provided, the application will fallback to Streamlit secrets or default to visual-only indicators.*

### 4. Running the Application
Launch the Streamlit dashboard:
```bash
streamlit run main.py
```
Open `http://localhost:8501` in your browser.

---

## 🏋️‍♂️ How to Use
1. **Enter Name**: Choose a unique username on the login wall to start your session.
2. **Configure Plan**: Use the sidebar options to select your exercise, targets sets, and reps per set.
3. **Start Session**: Click **Start Workout** to enable your webcam stream.
4. **Position Yourself**: Align your body fully within the camera bounds.
5. **Get Coaching**: Perform your reps; the AI will automatically count valid movements and speak corrections aloud if a form deviation is spotted.