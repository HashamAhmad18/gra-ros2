
# FSOCO YOLO Setup and Training Guide

## 1. Installs and Setup

### FSOCO:
Create a directory for all of your YOLO projects, e.g. `yolo_dir`.

Open cmd line, and use:
```
cd ...
mkdir yolo_dir
```
Or create the folder in File Explorer and use `cd` to find it in cmd, e.g.:
```
cd Desktop\FSAI\yolo_dir
```

Download the **FSOCO dataset** (bounding boxes for cones) here:  
🔗 https://fsoco.github.io/fsoco-dataset/download

This is in *Supervisely format*. Supervisely is a paid platform for model training.

Instead, use **Roboflow** to convert the annotated images from Supervisely to YOLOv11 format:  
🔗 https://roboflow.com/convert/supervisely-json-yolov11-pytorch-txt

Because of the dataset size, upload one or two folders at a time (the dataset is split into subsets). Once converted, move the new files into a destination folder (e.g. `FSOCO_converted`).

> 💡 Tip: Paid Roboflow allows larger uploads at once.

---

### Python Setup

Open cmd and navigate to your YOLO directory:
```
cd Desktop\yolo_dir
```
Do everything from here.

Ensure you’re using **Python 3.9–3.12 (maybe 3.13)**:
```
python --version
```

You can optionally create a Python environment (not required):
```
python -m venv env1
env1\Scriptsctivate.bat
```

#### Install required libraries:
```
pip install setuptools
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
pip install git+https://github.com/ultralytics/ultralytics.git@main
```

Download a **pre-trained YOLOv11 model** for object detection from:  
🔗 https://github.com/ultralytics/ultralytics

Scroll to the models table and download e.g. `yolo11n.pt`.  
Place this in your `yolo_dir`.

Now, you’re ready to run YOLO commands!

---

## 2. Testing YOLO with Pre-trained Model

Before training, test on a stock image:
```
yolo detect predict model=yolo11n.pt source='path_to_image.jpg'
```

Example: Place an image (e.g. `horse.jpg`) in `yolo_dir` and run the command.  
YOLO will create a `runs` folder containing results at:
```
runs/detect/predict/
```

---

## 3. Test with Last Year’s Model (Optional)

You can use last year’s model (`conev11n.pt`) from the Gryphon Racing AI GitHub.

Command:
```
yolo detect predict model=conev11n.pt source='path_to_cone_image.jpg'
```

---

## 4. Training

You’ll need a `.yaml` file that tells YOLO where your data is located.

Example `data.yaml`:
```yaml
# Dataset root directory
path: ../datasets/coco8

# Paths to training, validation, and testing images
train: images/train
val: images/val
test: images/test

# Number of classes and their names
nc: 5
names:
  0: blue_cone
  1: large_orange_cone
  2: orange_cone
  3: unknown_cone
  4: yellow_cone
```

Steps:
1. Create `data.txt` in your `yolo_dir`.
2. Paste the above YAML content.
3. Edit the directories to match your setup.
4. Save the file and rename the extension to `.yaml` (cannot edit afterward).

Start with a small subset of data for quicker testing.

---

### Training Command:
```
yolo detect train data=data.yaml model=yolov8n.pt epochs=100 imgsz=64
```

- **data**: path to your `.yaml` file  
- **model**: base model (e.g. your pretrained YOLO or empty model)  
- **epochs**: number of training cycles (start small: 5–10)  
- **imgsz**: image size (64 works fine)

---

## 5. Results

After training, results are stored in:
```
runs/train/
```
Your best trained model will be at:
```
runs/train/exp/weights/best.pt
```
Rename `best.pt` as needed — you can continue training from it later.

> There are additional ways to visualize metrics and compare models.  
> Feel free to explore YOLO’s built-in tools for this.

---

📞 For questions or troubleshooting:  
> “If you have any questions, feel free to ask team leads.”

---

✅ **Summary**
You’ve now:
- Installed the FSOCO dataset and YOLO environment  
- Tested YOLO with pre-trained models  
- Converted Supervisely data for YOLO  
- Trained and saved your own model

Good luck training!
