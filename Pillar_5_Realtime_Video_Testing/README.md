# 🎬 Pillar 5: Real-Time Video Testing & Evaluation Suite

This testing pillar allows you to drop **ANY video file** (dashcam footage, driving clips, phone recordings, synthetic clips) and immediately watch SAFAR act upon it in real time.

---

## 🚀 How to Test Any Video

### Method 1: Drag & Drop (Easiest)
Simply **drag and drop any `.mp4`, `.avi`, or `.mov` file** directly onto:
👉 `TEST_MY_VIDEO.bat`

SAFAR will launch an interactive playback window with the racing cockpit HUD, real-time threat scores, dynamic stopping envelopes, and automated braking intervention.

---

### Method 2: The `input_videos/` Folder
1. Copy or drop any video into the **`input_videos/`** folder.
2. Double-click **`TEST_MY_VIDEO.bat`** (or run `python test_any_video.py`).
3. SAFAR will automatically detect the video and begin processing.

*(A sample driving video `input_videos/demo_driving.mp4` is already pre-loaded for immediate testing!)*

---

### Method 3: Command Line
```cmd
python test_any_video.py "C:\path\to\my_dashcam.mp4"
```
Optional flags:
* `--speed 50.0`: Initial vehicle speed in km/h (default: 25.0)
* `--conf 0.35`: YOLO detection confidence threshold
* `--no-display`: Run headless (processes video as fast as possible without displaying a window)

---

## 🎮 Interactive Real-Time Controls During Playback

| Key | Action |
| :---: | :--- |
| **`[SPACE]`** | **Pause / Resume** video playback |
| **`[W]`** | **Speed Up** vehicle by $+5\text{ km/h}$ |
| **`[S]`** | **Slow Down** vehicle by $-5\text{ km/h}$ |
| **`[Q]`** or **`[ESC]`** | **Quit** and finalize output video |

---

## 📊 Where Are Results Saved?

All processed videos and logs are saved inside **`output_results/`**:
1. **`[video_name]_safar_hud.mp4`**: High-definition video with the full racing HUD, 3D target holograms, G-meter, dynamic radar minimap, and flashing AEB warning banners.
2. **`[video_name]_telemetry.csv`**: Frame-by-frame synchronized telemetry log containing:
   * Vehicle speed (km/h)
   * Throttle and Brake percentages ($0.0 - 1.0$)
   * Calculated Threat Score ($S_{risk}$)
   * Dynamic Time-to-Collision ($\text{TTC}$)
   * Distance to hazard (meters)
   * AEB intervention state & trigger reason

---

## ⚙️ What Engine is Running Behind the Scenes?

```
 ┌─────────────────────────────────────────────────────────┐
 │ 1. PYTHON PERCEPTION (Ultralytics YOLO11)               │
 │    Extracts 2D bounding boxes and road surface patches. │
 └────────────────────────────┬────────────────────────────┘
                              │
                              ▼
 ┌─────────────────────────────────────────────────────────┐
 │ 2. HIGH-SPEED C++ CALCULATION KERNEL (safar_core.dll)   │
 │    • Optical Pinhole Distance Projection (Z = fx*W/w)   │
 │    • Ego Driving Corridor Bounds (|X| <= 1.65m)         │
 │    • Kinematic Dynamic Stopping Distance:               │
 │      d_stop = v*t_react + v² / (2*mu*g)                 │
 │    • Relative Velocity & Time-To-Collision Calculus     │
 │    • Multi-frame Anti-Jitter Temporal Filter            │
 │    • Dual-Threshold Hysteresis State Machine            │
 └────────────────────────────┬────────────────────────────┘
                              │
                              ▼
 ┌─────────────────────────────────────────────────────────┐
 │ 3. CLOSED-LOOP ACTUATION & AAA COCKPIT HUD RENDERER     │
 │    • Graduated / Emergency Braking Command              │
 │    • Speedometer, Radar Minimap, and G-Force Circle     │
 └─────────────────────────────────────────────────────────┘
```
