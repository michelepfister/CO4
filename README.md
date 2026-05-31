# CO4 BRACS Lumen Segmentation Challenge

## Overview

This project investigates automatic lumen segmentation in H&E-stained breast histopathology images derived from the BRACS dataset. The work compares classical image-processing approaches and a deep learning-based segmentation workflow for detecting glandular lumen regions.

The project was developed as part of the MSLS / CO4 course at ZHAW Zurich University of Applied Sciences.

Author: Michele Pfister
Course: MSLS / CO4
Semester: SS26
University: ZHAW Zurich University of Applied Sciences

⸻

## Project Goal

The aim of this project is to:

* Segment lumen structures in breast histopathology image patches
* Compare manual and automated segmentation methods
* Evaluate segmentation quality using overlap metrics
* Explore the strengths and limitations of classical and AI-based image segmentation approaches

Lumen segmentation is challenging because lumen boundaries are often unclear, partially collapsed, weakly stained, or visually similar to surrounding stromal tissue.

⸻

## Dataset

The dataset consists of:

* 15 H&E-stained BRACS histopathology image patches
* RGB PNG images
* Resolution: 299 × 299 pixels

Manual lumen annotations were created in QuPath and exported as GeoJSON files.

Structures of Interest

The project focuses on:

* Glandular lumen regions
* Hollow or weakly stained tissue structures
* Duct-like regions enclosed by epithelial tissue

⸻

## Methods

1. Preprocessing

The preprocessing pipeline includes:

* RGB image loading
* Grayscale conversion
* HED stain-space conversion
* Hematoxylin extraction
* Gaussian blurring
* Otsu thresholding
* Morphological operations

The preprocessing improves tissue contrast and supports downstream segmentation.

⸻

2. Manual Segmentation

Manual masks were generated using:

* QuPath polygon annotations
* GeoJSON export
* Conversion to binary masks using Python

The manual annotations serve as ground truth for evaluation.

⸻

3. Automated Segmentation Methods

Three segmentation methods were implemented and compared.

Method 1 — HED Morphology

Classical image-processing pipeline using:

* HED stain decomposition
* Thresholding
* Hole filling
* Morphological filtering

Method 2 — HED + Watershed

Improved classical pipeline using:

* Distance transforms
* Watershed segmentation
* Connected component separation

Method 3 — Cellpose + Lumen Filter

Deep learning-assisted segmentation using:

* Cellpose pretrained model
* Tissue mask filtering
* Morphological postprocessing

⸻

## Evaluation

The segmentation methods were evaluated against manual masks using:

* Dice coefficient
* Mean performance comparison
* Standard deviation analysis

Results Summary

Method	Mean Dice	Std
HED + Watershed	0.686	0.218
Cellpose + Lumen Filter	0.655	0.226
HED Morphology	0.641	0.221

The HED + Watershed method achieved the strongest overall performance.

⸻

## Project Structure

project/
│
├── CO4_Project_Michèle_Pfister.ipynb
├── requirements.txt
├── README.md
│
├── Data/
│   ├── Data_Original/
│   ├── Manual_Masks/
│   └── Processed/
│
├── Results/
│   ├── Figures/
│   ├── Overlays/
│   └── Metrics/
│
└── GeoJSON/

⸻

## Installation

Clone the repository and install the dependencies.

git clone <repository-url>
cd <repository-folder>
python -m pip install -r requirements.txt

⸻

## Requirements

Main Python packages used in this project:

* numpy
* pandas
* matplotlib
* opencv-python
* scikit-image
* scipy
* pillow
* cellpose
* jupyter

Install all dependencies with:

python -m pip install -r requirements.txt

⸻

## Running the Notebook

Open the notebook in Jupyter:

jupyter notebook

Then run:

CO4_Project_Michèle_Pfister.ipynb

The notebook performs:

1. Dataset loading
2. Preprocessing
3. Manual mask generation
4. Automated segmentation
5. Visualization
6. Dice score evaluation
7. Result comparison

⸻

Example Outputs

The project generates:

* Binary lumen masks
* Overlay visualizations
* Segmentation comparison figures
* Dice score tables
* Preprocessing visualizations

⸻

## Discussion

The results demonstrate that:

* Automatic lumen segmentation in BRACS images is feasible
* Classical image-processing methods can achieve moderate agreement with manual masks
* Watershed-based separation improves segmentation robustness
* Tissue heterogeneity and unclear lumen boundaries remain major challenges

Although manual segmentation remains biologically informed, automated approaches provide faster and more reproducible analysis.

⸻

## Future Improvements

Possible future extensions include:

* Larger annotated datasets
* Stain normalization
* Improved postprocessing
* Supervised deep learning models such as U-Net
* More robust lumen boundary detection
* Multi-class tissue segmentation

⸻

## References

* Bankhead P, et al. (2017). QuPath: Open source software for digital pathology image analysis.
* BRACS histopathology dataset
* Cellpose segmentation framework

⸻

## License

This project was created for academic and educational purposes.
