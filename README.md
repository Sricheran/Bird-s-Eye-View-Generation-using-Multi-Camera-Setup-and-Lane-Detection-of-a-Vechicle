# 🛰️ 360° Bird’s Eye View (BEV) Generation & Lane Detection using nuScenes Multi-Camera Setup

This project implements a real-time computer vision pipeline to generate **Bird’s Eye View (BEV)** representations and detect **lanes** using only monocular camera inputs from the [nuScenes dataset](https://www.nuscenes.org/nuscenes#download). No LiDAR or 3D point cloud data is used.

> Built using classical computer vision algorithms like **SIFT**, **Homography**, **Perspective Transform**, and **Hough Line Detection**.

---

## 🎯 Objective

Enable cost-effective surround view and lane detection by:
- Stitching camera feeds from 6 surround-view cameras
- Applying homography to warp into a BEV format
- Detecting lane lines using edge and line detection
- Overlaying the ego vehicle on the BEV image
- Running the entire pipeline in real-time

---

## 🎥 Camera Configuration

- Cameras Used: CAM_FRONT, CAM_FRONT_LEFT, CAM_FRONT_RIGHT, CAM_BACK, CAM_BACK_LEFT, CAM_BACK_RIGHT
- Image Format: 1600×900 JPEG
- Frame Rate: 12 Hz
- Dataset: [nuScenes v1.0-mini](https://www.nuscenes.org/nuscenes#download)

---

## 🔍 Methodology

### 🔹 1. Image Stitching
- Extract camera calibration and pose data from:
  - `sensor.json`, `calibrated_sensor.json`, `ego_pose.json`, `sample_data.json`
- Align and stitch images using:
  - `SIFT` for keypoint detection
  - `FLANN` for feature matching
  - `cv2.findHomography()` with RANSAC for alignment

### 🔹 2. Perspective Transformation
- Select a trapezoidal ROI on the road from the stitched image
- Use `cv2.getPerspectiveTransform()` to convert it to a top-down BEV view

### 🔹 3. Rear-View Integration
- Repeat stitching and warping for rear cameras
- Flip and vertically stack front and rear BEVs

### 🔹 4. Ego Vehicle Overlay
- Overlay a transparent PNG of the ego car at the center of BEV

### 🔹 5. Lane Detection
- Crop BEV region and apply:
  - Grayscale, Gaussian blur, Canny edges
  - Hough Line Transform with `cv2.HoughLinesP`
- Overlay detected lanes in green

---

## 🧪 Sample Output

> Add this line below if you have a sample image or GIF in the repo:

![BEV Output](Screenshot%202025-04-23%20103845.png)

## Key libraries used:

- **OpenCV**
- **NumPy**
- **json, os**
- (Optional) tqdm, matplotlib for visualization/debugging

## Running the Code
### Update paths in your script to match your local dataset structure:
- dataset_dir = r"C:\Users\ASUS\Labs\IVP\Dataset"
- ego_car_path = r"C:\Users\ASUS\Downloads\download-removebg-preview.png"

## Future Work
- **Integrate deep learning (e.g., U-Net) for semantic segmentation of lanes and parking zones**
- **Fuse BEV perception with LiDAR/RADAR for depth awareness**
- **Apply temporal smoothing across frames to reduce flickering**
- **Optimize for deployment on embedded devices**

## Contributors
- SriCheran CH – sricheran320@gmail.com
- Aditya V
- Bavathayini N

