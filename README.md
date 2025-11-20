# Car Counter

A small computer-vision project that detects vehicles and counts vehicles in a video using YOLOv8 (large) and SORT.

This repository contains a Jupyter notebook and a ready-to-run script for applying an optional image filter overlay (PNG), running inference with Ultralytics YOLOv8, tracking with SORT, and counting vehicles that cross a configurable line.

---

## Contents

- `car_counter.ipynb` — exploratory notebook with the detection/tracking loop and visualization.
- `sort.py` — SORT tracker implementation used to maintain object IDs.
- `run_counter.py` — standalone runner script for inference, tracking and counting (CLI-friendly).
- `yolov8l.pt` — YOLOv8 large model (not included by this README; already present in the repo root for your workspace).
- `filter.png` — optional overlay/filter image (place in the same folder if used).

---

## Requirements

- Python 3.8+ (tested on 3.10)
- OpenCV (cv2)
- numpy
- ultralytics (YOLOv8)
- filterpy (for Kalman filter used by SORT)
- scipy (fallback for the LAP solver)
- optionally: `lap` (faster assignment solver, available on conda-forge)

Install with pip (recommended for virtualenv) or conda (recommended for scientific stack):

pip (uses same interpreter as your shell/kernel):

```bash
python -m pip install -U pip
python -m pip install -U ultralytics opencv-python numpy filterpy scipy
# optional but recommended for coloring/plotting:
python -m pip install matplotlib
# optional faster LAP solver (may require build tools) - prefer conda:
python -m pip install lap
```

conda (recommended to avoid compilation issues):

```bash
conda create -n carcounter python=3.10 -y
conda activate carcounter
conda install -c conda-forge ultralytics opencv numpy filterpy scipy matplotlib lap -y
```

If you will run the notebook, install `scikit-image` if needed:

```bash
python -m pip install scikit-image
```

---

## Quick start — run the script

From the project folder run:

```bash
python run_counter.py --source cars.mp4 --filter filter.png --model yolov8l.pt --display
```

To save processed output to a file:

```bash
python run_counter.py --source cars.mp4 --filter filter.png --model yolov8l.pt --output out.mp4 --save
```

Key CLI flags:
- `--source` : input video file (default `cars.mp4`)
- `--filter` : optional PNG overlay (keeps alpha transparency)
- `--model`  : path to YOLO model (default `yolov8l.pt`)
- `--display`: show OpenCV display window
- `--save` and `--output`: save processed video

Notes:
- The script attempts to keep display playback close to the source FPS by subtracting processing time from the frame interval. If inference is slower than realtime, the script will not speed up frames (it uses minimal wait).

---

## Notebook

Open `car_counter.ipynb` if you prefer interactive development. The notebook contains step-by-step cells for:
- importing the model,
- preparing a `filter.png` overlay (resizing and alpha blending),
- running frame-by-frame inference,
- using `sort.py` to track and assign stable IDs, and
- counting when object centroids cross a line.

If you run in a headless environment (no display), the repository's `sort.py` already falls back to a non-interactive matplotlib backend to avoid import-time errors.

---

## Troubleshooting

- If you see warnings like `WARNING ⚠️ 'source' is missing. Using 'source=.../ultralytics/assets'`, pass an explicit source to the model call or use the provided `run_counter.py` which sets the source directly.
- If `lap` installation fails with build errors, prefer the conda-forge package or rely on SciPy's `linear_sum_assignment` (the `sort.py` implementation falls back automatically).
- If importing `sort.py` fails due to tkinter/TkAgg backend errors, install system `tk` (e.g., `sudo apt install python3-tk` or `conda install -c conda-forge tk`) or run in a notebook that sets an inline backend.

---

## License & attribution

This project uses the SORT implementation by Alex Bewley (GPL license noted in `sort.py`) and the Ultralytics YOLOv8 model. Make sure to follow their respective licenses when redistributing the model or code.

---

If you want, I can also:
- add a short CONTRIBUTING.md with commits/push instructions,
- create a small `requirements.txt` and a minimal `.gitignore` for GitHub, or
- convert `run_counter.py` into a pip-installable package scaffolding.

Happy counting — let me know which follow-up you'd like.
# Computer_Vision_Project_Car_Counter_Using_YOLOv8
