PAM Blueberry pH Pipeline

Terminal-first, reproducible Python pipeline for analyzing multi-frame Imaging-PAM TIFF files.

This project is structured to support:

Single-plant AOI feature extraction (current stage)

Multi-plant grid segmentation (future stage)

Genotype × pH statistical modeling

Scalable phenotyping workflows

Project Philosophy

Terminal runs everything

src/ contains reusable logic

scripts/ contains runnable entrypoints

No analysis locked inside notebooks

Every step is version controlled

This ensures reproducibility and scalability as experiments grow.

Current Capabilities
Single-Plant Multi-Frame TIFF Extraction

Reads multi-frame TIFF stack (frames, height, width)

Applies AOI masking (currently: nonzero mask)

Computes summary statistics per frame

Attaches frame labels (a–h placeholders)

Writes tidy CSV output

Folder Structure
pam_blueberry_ph/
│
├── data/
│   ├── raw_multiframe/        # Input TIFF stacks
│   ├── processed/             # Output feature tables
│   └── metadata/
│       └── frames_map.csv     # frame_index -> parameter mapping
│
├── scripts/
│   └── 03_features/
│       └── 03_extract_features_singleplant.py
│
├── src/
│   └── pamflow/
│       ├── io/
│       │   └── multiframe.py
│       └── features/
│           └── summary.py
│
└── README.md

Environment Setup

Create and activate a virtual environment:

python3 -m venv .venv
source .venv/bin/activate
pip install tifffile pandas numpy


For now, set:

export PYTHONPATH=$PWD


(Future improvement: editable install via pip install -e .)

Frame Mapping

data/metadata/frames_map.csv

Currently using placeholder labels:

frame_index,parameter
0,a
1,b
2,c
3,d
4,e
5,f
6,g
7,h


These will later be mapped to biological metrics (e.g., Fv/Fm, Y(II), Y(NO), etc.).

Running Single-Plant Extraction

Example:

python scripts/03_features/03_extract_features_singleplant.py \
  --tif data/raw_multiframe/PRACTICE_FILE_1.tif \
  --sample-id PRACTICE_1 \
  --mask nonzero


Output:

data/processed/features_singleplant.csv


Each row corresponds to one frame (a–h) with summary statistics:

n_pixels

min

max

mean

median

sd

p10

p90

Masking Strategy (Current)

--mask nonzero

Excludes background pixels with value 0

Improves summary statistics vs full-frame averaging

Future options:

Threshold-based masking

Single mask source frame applied across all metrics

True plant segmentation

Next Development Steps

Stable mask across frames

Batch processing of multiple TIFF files

Sample sheet integration (genotype, pH treatment, rep)

Multi-plant grid segmentation

Statistical modeling layer

Status

SSH-authenticated GitHub repository

Working venv

Modular codebase

First successful single-plant AOI extraction complete