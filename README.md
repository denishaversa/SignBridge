# 🤟 SignBridge — ASL to Text in Your Browser

A real-time American Sign Language (ASL) to text translator that runs entirely in your browser. Uses your webcam with MediaPipe hand tracking and a lightweight gesture classifier to recognize signs and build a live transcript.

**No server required. No sign-up. No data leaves your device.**

## ✨ Features

- **Real-time hand tracking** — MediaPipe Hands draws a skeletal overlay on your hand at 30+ FPS
- **10 built-in ASL signs** — Hello, Thank You, Yes, No, Please, Sorry, Help, I Love You, Good, Stop
- **Train custom signs** — record any gesture from your webcam to teach new signs
- **Calibrate existing signs** — improve accuracy by recording your version of built-in signs
- **Export / Import training data** — share trained models between devices or collaborate with others
- **Text-to-speech** — speak your transcript aloud with one click
- **Copy to clipboard** — quickly paste your translated text anywhere
- **Two detection modes** — "Always Top Prediction" for responsiveness or "55% Threshold" for accuracy
- **Demo mode** — try the interface without a camera by clicking signs in the sidebar
- **Zero dependencies** — no TensorFlow, no Python, no server. One HTML file.

## 🚀 Quick Start

### Option 1: Live Demo (recommended)

Visit the deployed site on Netlify — webcam works over HTTPS with no setup:

👉 https://your-site.netlify.app

### Option 2: Local server

```bash
# Clone the repo
git clone https://github.com/denishaversa/signbridge.git
cd signbridge

# Serve locally (needed for webcam access)
python3 -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000) in Chrome or Edge.

### Option 3: Open directly

You can open `index.html` directly in your browser, but webcam access will be blocked. Use **Demo Mode** to explore the interface without a camera.

## 🖐️ Supported Signs

| Sign | Gesture |
|------|---------|
| Hello | Open hand wave near forehead |
| Thank You | Flat hand from chin outward |
| Yes | Closed fist (nod motion) |
| No | Index + middle finger snap to thumb |
| Please | Flat hand circles on chest |
| Sorry | Fist circles on chest |
| Help | Fist on flat palm, lift up |
| I Love You | 🤟 Thumb + index + pinky extended |
| Good | Thumbs up |
| Stop | Flat hand, palm facing out |

## 🎓 Training Custom Signs

1. Click **Train Sign** in the controls bar
2. Select **+ New Sign** tab and enter a name
3. Hold your gesture in front of the camera
4. Click **⏺ Start Recording** — hold for 3-5 seconds (aim for 30+ samples)
5. Click **⏹ Stop Recording** → **Save**
6. Your sign is immediately active in the classifier

## 🎯 Calibrating Existing Signs

If a built-in sign has low confidence for your hand/camera setup:

1. Click **Train Sign** → select **🎯 Calibrate Existing** tab
2. Pick the sign from the dropdown
3. Record your version of the gesture (30+ samples recommended)
4. Save — the classifier blends your data with the built-in model

Calibrated signs show a blue dot in the sidebar. Click **↺** in the Sign Studio to reset to defaults.

## 📤 Sharing Training Data

Training data can be exported and shared between devices:

1. **Export** — click 📤 to download a JSON file with all your custom signs and calibrations
2. **Import** — click 📥 on another device to load the file
3. Continue training on top of imported data — contributions stack

This enables collaborative model building: multiple people can each calibrate signs and merge their data for a model that works across different hands and environments.

## ⚙️ Detection Modes

The **Detection Rules** card in the sidebar lets you switch between:

| Mode | Confidence | Cooldown | Best for |
|------|-----------|----------|----------|
| ⚡ Top Prediction | Any (always accepts) | 2.0s | Responsiveness, casual use |
| 🎯 55% Threshold | ≥ 55% required | 1.8s | Accuracy, reducing false positives |

Both modes require the sign to be held for ~10 consecutive frames (~0.5s) to confirm.

## 🏗️ How It Works

```
Webcam → MediaPipe Hands → 21 3D Landmarks → Feature Extraction (41 features) → Classifier → Transcript
```

**Feature extraction** computes 41 geometric features from the hand landmarks:
- Fingertip-to-wrist distances (5)
- Fingertip-to-palm distances (5)
- Finger curl angles at PIP joints (5)
- Finger extension ratios (5)
- All inter-fingertip distances (10)
- Thumb-to-finger distances (4)
- Hand orientation — wrist Y, vertical span, horizontal span (3)
- Finger spread angles (4)

All distances are normalized by hand size for scale invariance.

**Classification** uses weighted Euclidean distance to pre-computed sign centroids with a Gaussian kernel for scoring. Custom signs and calibrations add nearest-neighbor matching for higher accuracy.

## 🌐 Browser Support

| Browser | Webcam | Demo Mode |
|---------|--------|-----------|
| Chrome 90+ | ✅ | ✅ |
| Edge 90+ | ✅ | ✅ |
| Firefox | ⚠️ Limited | ✅ |
| Safari | ⚠️ Limited | ✅ |

Chrome or Edge recommended for best MediaPipe performance.

## 📁 Project Structure

```
signbridge/
├── index.html    ← entire app (HTML + CSS + JS, self-contained)
├── README.md     ← this file
└── LICENSE        ← MIT
```

## ⚠️ Limitations

- **Static gestures only** — recognizes hand shapes, not motion-based signs (which require temporal sequence modeling)
- **Single hand primary** — classifier uses the first detected hand; two-hand signs are approximated
- **Synthetic training data** — built-in centroids are based on idealized gesture geometry, not real signer data. Calibration significantly improves accuracy for your specific setup
- **No fingerspelling** — individual letter recognition (A-Z) is not included in the current vocabulary

## 🙏 Acknowledgments

- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html) — Google's real-time hand tracking
- ASL gesture references from the Deaf community and educational resources

## 📄 License

MIT — use it however you like.
