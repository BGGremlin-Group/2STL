Got it — below are updated README.md files for each zip, with the requirements fully included inside the README (so users don’t need to open requirements.txt to know what to install).

(And yes: you can just drop these README.md files into each zip before uploading.)


---

README.md for the Web App zip (FastAPI)

# 2STL — Image → 3D Exporter (Relief + Figurine) — Web App (Local-only)

A **local-only** web app that converts:

- **Single image → watertight bas-relief / lithophane**  
  (plaques, coins/medallions, keychains, lithophanes)
- **Multi-angle photos / ZIP / turntable video → 3D figurine mesh**  
  (photogrammetry pipeline with live job logs)

Exports: **STL / OBJ / PLY / GLB**

✅ No Open3D required (and no `pip install open3d` dependency).

---

## Requirements (Python)

This project requires **Python 3.10+**.

Install these packages:

```bash
python -m pip install --upgrade pip
python -m pip install \
  fastapi==0.115.0 \
  "uvicorn[standard]==0.30.6" \
  python-multipart==0.0.9 \
  pillow==10.4.0 \
  numpy==2.0.1 \
  trimesh==4.5.3 \
  pygltflib==1.16.2
```

---

Quick Start

1) Run the server

python img3d_app.py

2) Open the UI

Open in your browser:

http://127.0.0.1:8000


> This app binds to 127.0.0.1 by default (local machine only).




---

Features

Relief / Lithophane (single image)

Upload JPG/PNG/WebP

Adjustable:

width (mm), base thickness (mm), relief height (mm)

resolution (pixels wide)

blur/smoothing

invert

black/white points + gamma

crop (fractional: left,top,right,bottom)

auto-contrast


Generates a watertight mesh with a closed back + side walls

Export formats: STL / OBJ / PLY / GLB


Figurine (multi-angle photogrammetry)

Inputs:

many photos (recommended: 20–120)

ZIP of photos

turntable video (requires FFmpeg)


Job-based processing:

start job

live logs + progress

download result when done




---

Figurine Mode Requirements (External Tools)

Figurine mode uses external photogrammetry tools installed separately and available in PATH:

Engine A — COLMAP (CPU or CUDA GPU build)

Requires colmap executable on PATH

GPU support depends on your COLMAP build


Engine B — Meshroom

Requires meshroom_batch executable on PATH

Generally GPU-oriented


Optional — FFmpeg (Video Input)

Requires ffmpeg executable on PATH

Used only for extracting frames from turntable videos


You can check what the backend detects at:

http://127.0.0.1:8000/api/health



---

API Endpoints

GET / → UI

GET /api/health → tool detection (COLMAP/Meshroom/FFmpeg)

POST /api/relief/convert → relief/lithophane export

POST /api/figurine/start → start figurine job

GET /api/job/{job_id} → job status + logs

GET /api/job/{job_id}/download → download result



---

Troubleshooting

pip install open3d fails

This project does not use Open3D. Open3D does not publish wheels for every platform/architecture, so pip install open3d may fail on some systems. Relief mode works with the Python requirements listed above.

Figurine fails immediately

Visit /api/health

If it shows colmap❌ and meshroom❌, install one engine and ensure it is on PATH.

If using video and ffmpeg❌, install FFmpeg or upload images instead.



---

Security Notes

This is designed for local usage and binds to 127.0.0.1.
Do not expose it publicly without adding authentication and hardening.


---

License

MIT (or your preferred repo license).

---

## README.md for the Windows GUI zip (PySide6 / Qt)

# 2STL — Image → 3D Exporter (Relief + Figurine) — Windows GUI (No HTML)

A **Windows desktop GUI** (Qt/PySide6) that converts:

1) **Single image → watertight bas-relief / lithophane**  
2) **Multi-angle photos / ZIP / turntable video → 3D figurine mesh** (photogrammetry)

Exports: **STL / OBJ / PLY / GLB**

✅ No Open3D required (and no `pip install open3d` dependency).

---

## Requirements (Windows / Python)

This project requires **Python 3.10+** on Windows.

Install these packages:

```powershell
py -m pip install --upgrade pip
py -m pip install `
  PySide6==6.7.2 `
  pillow==10.4.0 `
  numpy==2.0.1 `
  trimesh==4.5.3 `
  pygltflib==1.16.2
```

---

Quick Start (Windows)

Run the GUI

py img3d_gui.py


---

Features

Relief / Lithophane (single image)

Pick an image file

Adjust:

width (mm), base thickness (mm), relief height (mm)

resolution (pixels wide)

blur/smoothing

invert

black/white points + gamma

crop (fractional: left,top,right,bottom)

auto-contrast


Generate mesh (watertight, closed)

Save to STL / OBJ / PLY / GLB


Figurine mode (multi-angle photogrammetry)

Input options:

Folder of images

ZIP of images

Turntable video (FFmpeg required)


The app:

runs the job in the background (non-blocking)

streams logs live

updates progress

exports to STL / OBJ / PLY / GLB



---

Figurine Mode Requirements (External Tools)

Figurine mode requires at least one engine installed and available in PATH:

Engine A — COLMAP

Ensure colmap.exe works in PowerShell:

colmap -h

GPU depends on whether your COLMAP build includes CUDA support.


Engine B — Meshroom

Ensure meshroom_batch.exe works:

meshroom_batch --help


Optional — FFmpeg (Video)

Ensure ffmpeg.exe works:

ffmpeg -version


If FFmpeg is missing, use images or ZIP instead.


---

Important: “Images from the same angle”

Photogrammetry needs viewpoint changes. If photos are truly the same angle, figurine reconstruction may fail or produce a flat/degenerate result.

For best results:

20–120 images around the subject

consistent lighting

minimal blur

good overlap between successive photos

multiple elevations if possible



---

Optional: Build a single EXE (PyInstaller)

py -m pip install pyinstaller
py -m PyInstaller --onefile --noconsole img3d_gui.py

Your EXE will be in dist/.


---

License

MIT (or your preferred repo license).

---
