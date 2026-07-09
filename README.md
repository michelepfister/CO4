# CO4 BRACS Lumen Segmentation Challenge

Automatic lumen segmentation in H&E-stained breast histopathology image patches from the BRACS dataset.

This repository contains a notebook-based image analysis project developed for the MSLS / CO4 course at ZHAW Zurich University of Applied Sciences. The project compares classical image-processing pipelines with a Cellpose-assisted workflow for segmenting glandular lumen regions and evaluates each method against manually annotated masks.

**Author:** Michele Pfister  
**Course:** MSLS / CO4  
**Semester:** SS26  
**Institution:** ZHAW Zurich University of Applied Sciences

## Project Summary

Lumen segmentation in breast histopathology is challenging because lumen structures are often weakly stained, partially collapsed, irregularly shaped, or visually similar to surrounding stromal tissue. This project explores whether reproducible Python workflows can identify these structures in small H&E-stained BRACS image patches.

The analysis uses 15 RGB image patches with manual QuPath annotations as ground truth. Three automated segmentation strategies are implemented and evaluated with the Dice similarity coefficient.

## Objectives

- Segment glandular lumen structures in breast histopathology image patches.
- Convert manual QuPath GeoJSON annotations into binary reference masks.
- Compare classical and deep learning-assisted segmentation approaches.
- Quantify segmentation performance with overlap-based metrics.
- Identify strengths, limitations, and possible improvements for lumen detection.

## Dataset

The dataset included in this repository consists of:

| Property | Description |
| --- | --- |
| Source | BRACS breast histopathology image patches |
| Stain | Hematoxylin and eosin (H&E) |
| Number of images | 15 |
| Image format | RGB PNG |
| Resolution | 299 x 299 pixels |
| Manual annotations | QuPath polygon annotations exported as GeoJSON |

The structures of interest are glandular lumen regions, including hollow or weakly stained tissue spaces enclosed by epithelial tissue.

## Methodology

### 1. Preprocessing

The preprocessing workflow prepares the raw histology patches for segmentation:

- RGB image loading
- Grayscale conversion
- HED stain-space conversion
- Hematoxylin channel extraction
- Gaussian smoothing
- Otsu thresholding
- Morphological filtering

These steps improve contrast between tissue regions and candidate lumen structures.

### 2. Manual Mask Generation

Manual ground-truth masks are generated from QuPath annotations:

- Polygon annotations are exported from QuPath as GeoJSON files.
- GeoJSON coordinates are converted into binary masks.
- The resulting masks are used as reference labels for evaluation.

### 3. Automated Segmentation

Three segmentation methods are implemented and compared:

| Method | Description |
| --- | --- |
| HED morphology | Classical color-deconvolution pipeline using HED stain separation, thresholding, hole filling, and morphological filtering. |
| HED + watershed | Extension of the HED-based approach using distance transforms and watershed segmentation to separate connected lumen candidates. |
| Cellpose + lumen filter | Cellpose-based region proposal followed by intensity, saturation, tissue-mask, and morphology-based filtering for lumen-like structures. |

## Evaluation

Automated masks are compared against manual masks using the Dice similarity coefficient:

```text
Dice(A, B) = 2 |A intersect B| / (|A| + |B|)
```

A Dice score of `1.0` indicates perfect overlap, while `0.0` indicates no overlap.

### Results

| Method | Mean Dice | Standard Deviation |
| --- | ---: | ---: |
| HED + watershed | 0.686 | 0.218 |
| Cellpose + lumen filter | 0.655 | 0.226 |
| HED morphology | 0.641 | 0.221 |

The HED + watershed method achieved the strongest mean Dice score on this dataset. Its improvement over the HED morphology baseline was moderate, suggesting that watershed refinement helps separate connected lumen candidates but does not fully solve the underlying ambiguity of lumen boundaries.

## Repository Structure

```text
.
├── CO4_Project_Michèle_Pfister.ipynb   # Main analysis notebook
├── CO4_Project_Michèle_Pfister.html    # Exported notebook report
├── requirements.txt                    # Python dependencies
├── README.md                           # Project documentation
├── Data/
│   └── Data_Original/                  # Raw BRACS image patches
├── geojson/                            # QuPath annotation exports
├── BRACS lumen segmentation.pptx       # Project presentation
└── BRACS lumen segmentation.mp4        # Project video
```

When the notebook is executed, it creates additional output folders such as:

- `Data/Data_Processed`
- `manually masked data`
- `automated_masks`
- `watershed_masks`
- `cellpose_masks`
- `cellpose_model_cache`

The Cellpose model cache can be regenerated automatically and is not required as a permanent deliverable.

## Installation

Clone the repository and install the required Python packages:

```bash
git clone <repository-url>
cd <repository-folder>
python -m pip install -r requirements.txt
```

Using a virtual environment is recommended:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

On Windows, activate the virtual environment with:

```powershell
.venv\Scripts\activate
```

## Requirements

The main dependencies are:

- `numpy`
- `pandas`
- `matplotlib`
- `opencv-python-headless`
- `scikit-image`
- `scipy`
- `pillow`
- `cellpose`
- `jupyter`

The exact dependency list is provided in `requirements.txt`.

## Usage

Start Jupyter from the repository root:

```bash
jupyter notebook
```

Open and run:

```text
CO4_Project_Michèle_Pfister.ipynb
```

The notebook performs the full analysis workflow:

1. Load BRACS image patches and GeoJSON annotations.
2. Generate preprocessing outputs.
3. Convert manual annotations into binary masks.
4. Run the HED morphology segmentation method.
5. Run the HED + watershed segmentation method.
6. Run the Cellpose + lumen filter segmentation method.
7. Compare each automated method against the manual masks.
8. Visualize masks, overlays, and Dice score distributions.

To export an executed HTML report, run:

```bash
jupyter nbconvert --to html --execute "CO4_Project_Michèle_Pfister.ipynb"
```

## Outputs

The project generates:

- Preprocessed RGB, grayscale, and binary mask images.
- Manual binary masks from QuPath annotations.
- Automated masks for each segmentation method.
- Overlay visualizations for qualitative inspection.
- Dice score summaries and comparison plots.

## Key Findings

- Automatic lumen segmentation is feasible on the selected BRACS image patches.
- Classical image-processing methods provide a useful and interpretable baseline.
- Watershed refinement improves robustness by separating connected candidate regions.
- Cellpose can identify relevant structures, but additional filtering is needed to reduce false positives in pale non-lumen tissue regions.
- Ambiguous tissue morphology, weak staining, and incomplete lumen boundaries remain the main limitations.

## Future Work

Potential extensions include:

- Expanding the annotated dataset.
- Adding stain normalization.
- Improving postprocessing and lumen boundary filtering.
- Training a supervised segmentation model such as U-Net.
- Evaluating additional metrics beyond Dice.
- Extending the task toward multi-class tissue segmentation.

## Citation and Data Note

This project uses image patches derived from the BRACS dataset for educational coursework. When reusing or extending this work, please follow the applicable BRACS dataset citation and licensing requirements.

## References

- Bankhead, P. et al. (2017). QuPath: Open source software for digital pathology image analysis.
- BRACS breast cancer histopathology dataset.
- Stringer, C. et al. Cellpose: a generalist algorithm for cellular segmentation.

## License

This repository was created for academic and educational purposes as part of the MSLS / CO4 coursework.
