# CryoET Object Identification

This project implements a solution for the Kaggle competition "CryoET Object Identification" using YOLOv11 for detecting and localizing protein particles in cryo-electron tomography (CryoET) volumes.

## Overview

Cryo-electron tomography (CryoET) is a powerful technique for visualizing the 3D structure of biological samples at near-atomic resolution. This project focuses on identifying and localizing specific protein particles within CryoET volumes using deep learning-based object detection.

The solution uses Ultralytics YOLOv11 model trained on denoised CryoET data to detect particles across multiple slices, followed by a custom aggregation algorithm using Depth-First Search (DFS) to cluster detections into 3D particle locations.

## Target Particles

The model detects the following protein particles:

- Apo-ferritin (radius: 60 Å)
- Beta-amylase (radius: 65 Å)
- Beta-galactosidase (radius: 90 Å)
- Ribosome (radius: 150 Å)
- Thyroglobulin (radius: 130 Å)
- Virus-like-particle (radius: 135 Å)

## Requirements

- Python 3.8+
- Ultralytics YOLO
- Zarr
- NumPy
- Pandas
- OpenCV
- Matplotlib
- tqdm

## Installation

1. Clone or download this repository.

2. Install the required packages:

   ```bash
   pip install ultralytics zarr numpy pandas opencv-python matplotlib tqdm
   ```

3. For Kaggle environment, the notebook handles package installation automatically.

## Usage

### Kaggle Notebook

This project is designed to run as a Kaggle notebook. The main script is in `notebook1710298c3c.ipynb`.

1. Upload the notebook to Kaggle.
2. Ensure the following datasets are attached:

   - `czii-cryo-et-object-identification` (competition data)
   - `ultralitycs` (Ultralytics package)
   - `hengck-czii-cryo-et-01` (wheel files)
   - `czii-yolo11-training-baseline` (trained YOLO model)

3. Run all cells in the notebook to generate predictions and create `submission.csv`.

### Local Execution

To run locally (requires access to competition data):

1. Download the competition dataset and place it in the appropriate directory structure.
2. Update file paths in the notebook to match your local setup.
3. Run the notebook cells sequentially.

## Model Details

- **Model**: YOLOv11 trained on CryoET slices
- **Input**: 2D projections of 3D volumes (630x630 pixels, resized to 640x640)
- **Output**: Bounding boxes with confidence scores for each particle type
- **Aggregation**: Custom DFS-based clustering to merge 2D detections into 3D particle centers

### Prediction Pipeline

1. Load denoised zarr volumes from test data
2. Convert to 8-bit images
3. Run YOLO inference on each slice
4. Aggregate detections using DFS to find connected components
5. Calculate 3D centroids for each particle cluster
6. Filter based on confidence thresholds and cluster size

## Key Parameters

- `first_conf`: Initial YOLO confidence threshold (default: 0.15)
- `final_conf`: Final confidence threshold (default: 0.2)
- `conf_coef`: Confidence coefficient for cluster bonus (default: 0.5)
- Particle-specific confidence thresholds for filtering

## Output

The script generates a `submission.csv` file with the following columns:

- `id`: Unique identifier
- `experiment`: Experiment run identifier
- `particle_type`: Type of detected particle
- `x`, `y`, `z`: 3D coordinates of particle center

## Performance

The model achieves efficient inference with estimated prediction time scaling linearly with the number of test volumes.

## References

- Kaggle Competition: [CryoET Object Identification](https://www.kaggle.com/competitions/czii-cryo-et-object-identification)
- Ultralytics YOLO: [https://github.com/ultralytics/ultralytics](https://github.com/ultralytics/ultralytics)
- Zarr: [https://zarr.readthedocs.io/](https://zarr.readthedocs.io/)
