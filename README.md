# FormFit AI

Real-time workout feedback in your browser. FormFit AI uses MediaPipe pose landmarks and exercise-specific rules to count reps, assess movement, and deliver optional AI-generated coaching cues with speech.

> **Project status:** Personal portfolio project. Feedback is experimental and is not medical advice or a substitute for a qualified trainer.

## Features

- Live webcam pose tracking with MediaPipe Pose Landmarker.
- Rep and set tracking for squats, push-ups, dumbbell biceps curls, shoulder press, and lunges.
- Exercise-specific movement metrics and form cues.
- Optional coaching text from Groq and speech output through Google Text-to-Speech.
- Per-user workout history stored in a local SQLite database.
- Separate static landing page in `LandingPage/`.

## Screenshots

<p align="center">
  <img src="Screenshot%202026-10-29%20192434.png" alt="FormFit AI workout setup screen" width="49%" />
  <img src="Screenshot%202026-10-29%20192453.png" alt="FormFit AI live workout tracking screen" width="49%" />
</p>

*Workout setup and live pose tracking.*

## How it works

The Streamlit interface sends webcam video to a `streamlit-webrtc` processor. MediaPipe detects pose landmarks; exercise detectors evaluate those landmarks and update repetition, set, and form metrics. Workout summaries are stored in SQLite. When a Groq API key is configured, the coaching service generates short feedback and gTTS turns it into speech.

## Run locally

### Requirements

- Python 3.10–3.12
- A webcam and a modern browser
- A Groq API key for AI-generated voice coaching (optional)

From the repository root, open a terminal and run:

```powershell
cd "Main App"
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
streamlit run main.py
```

On macOS or Linux, create and activate the environment with `python3 -m venv .venv` and `source .venv/bin/activate`, then install the same requirements and run `streamlit run main.py`.

Open the local URL printed by Streamlit (usually <http://localhost:8501>) and allow camera access. If PowerShell blocks environment activation, run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` in that terminal, then activate the environment again.

### Configure optional AI coaching

Create `Main App/.env` from `Main App/.env.example` and add your own key:

```dotenv
GROQ_API_KEY=your_groq_api_key
```

The app reads `.env` locally. Never commit your real key. Without a key, pose tracking and workout logging remain available, but AI-generated voice coaching may be unavailable.

## Deploy

The app entry point is `Main App/main.py`. For Streamlit Community Cloud, select this file as the app entry point and set `GROQ_API_KEY` under the app's Secrets settings if you want AI coaching. `Main App/packages.txt` lists the Linux system packages used by the deployment environment. Webcam access requires a secure browser context; localhost is treated as secure for local development.

## Repository layout

```text
LandingPage/                 Static portfolio landing page
Main App/
  main.py                    Streamlit app entry point
  detectors/                 Exercise-specific pose and rep logic
  ml_models/                 MediaPipe pose landmarker model
  services/
    auth/                    Username-based session setup
    coaching/                Groq feedback and text-to-speech
    config/                  Exercise options and coaching prompts
    persistence/             SQLite workout history
    tracking/                Rep, set, and metric synchronization
    vision/                  Webcam and pose processing
  static/                    App styles and font
  requirements.txt           Python dependencies
  packages.txt               Linux system packages
```

## Privacy and limitations

The application processes camera frames for live pose detection and does not save video. Workout history is stored in `Main App/data.db` on the machine running the app. The username prompt is a profile selector, not secure account authentication. AI form cues are rule-based/experimental and can be inaccurate; use safe loads and stop if you feel pain.

## Resume project summary

**FormFit AI — Real-Time AI Workout Coach** | Python, Streamlit, MediaPipe, OpenCV, WebRTC, SQLite, Groq

- Built a browser-based workout coach that applies MediaPipe pose landmarks and exercise-specific logic to track form, reps, and sets across five exercises.
- Integrated optional Groq-generated coaching cues and text-to-speech, with per-user workout history persisted in SQLite.
- Developed a Streamlit interface with live webcam processing and real-time workout metrics.

Adjust these bullets to reflect the parts you personally implemented and can explain in an interview.

## License

No license has been added yet. Add a license before inviting others to reuse or distribute the project.
