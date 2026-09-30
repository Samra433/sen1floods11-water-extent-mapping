# Water Extent Mapping in Flood-Event Scenes with Sentinel-1 and Sentinel-2

**A multi-seed comparison of SAR-only, optical-only, and fused U-Net models against an NDWI baseline on the Sen1Floods11 benchmark**

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-U--Net-red)
![Dataset](https://img.shields.io/badge/Dataset-Sen1Floods11-green)
![Task](https://img.shields.io/badge/Task-Flood%20Water%20Segmentation-informational)

## Overview

This project evaluates whether combining Sentinel-1 radar and Sentinel-2 optical imagery improves flood water mapping over either sensor alone and over a conventional NDWI threshold baseline.

Three U-Net models with a ResNet-34 encoder are trained and compared across three random seeds. Thresholds are selected on validation data only, and results are reported on both the standard test split and an out-of-region Bolivia split.

## Key Results

| Model | Test IoU | Bolivia IoU |
|---|---:|---:|
| SAR U-Net | 0.662 ± 0.013 | 0.609 ± 0.053 |
| Optical U-Net | 0.836 ± 0.006 | 0.797 ± 0.010 |
| Fused U-Net | 0.811 ± 0.012 | 0.743 ± 0.008 |
| NDWI baseline | 0.742 | 0.796 |

**Summary:** the optical-only U-Net was strongest on the standard test split, while the fused model did not outperform it. On the Bolivia split, NDWI was competitive and the learned models did not show a clear advantage.

## Dataset

This project uses the [Sen1Floods11](https://github.com/cloudtostreet/Sen1Floods11) dataset.

| Split | Chips | Notes |
|---|---:|---|
| Train | 252 | 10 regions |
| Validation | 89 | 10 regions |
| Test | 90 | Same regions as train |
| Bolivia | 15 | Single held-out region |

The labels represent water vs non-water in flood-event scenes. The dataset is not included in this repository; see the data download instructions in the project data folder.

## Methodology

- Inputs: Sentinel-1 VV/VH, Sentinel-2 B3/B8/B11/B12, and fused 6-band combinations
- Model: U-Net with ResNet-34 encoder and ImageNet-pretrained weights
- Loss: masked binary cross-entropy + Dice
- Optimizer: AdamW, learning rate 1e-4, weight decay 1e-4
- Augmentation: random 256 × 256 crops, flips, and rotations
- Seeds: 7, 21, 42
- Threshold selection: validation-only optimization over probability thresholds

## Repository Structure

```text
flood-event-water-mapping/
├── README.md
├── requirements.txt
├── notebooks/
│   └── flood_event_water_mapping.ipynb
├── data/
│   └── README.md
├── results/
└── .gitignore
```

## Getting Started

```bash
git clone https://github.com/YOUR-USERNAME/flood-event-water-mapping.git
cd flood-event-water-mapping
pip install -r requirements.txt
```

Download the dataset using the Google Cloud SDK:

```bash
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/S1Hand/*    data/S1Hand/
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/S2Hand/*    data/S2Hand/
gsutil -m cp -r gs://sen1floods11/v1.1/data/flood_events/HandLabeled/LabelHand/* data/LabelHand/
gsutil -m cp -r gs://sen1floods11/v1.1/splits/flood_handlabeled/*                data/splits/
```

Then run the notebook in `notebooks/flood_event_water_mapping.ipynb`.

## Limitations

- The standard test split is not geographically independent; train, validation, and test share regions.
- The Bolivia split is small and comes from a single region.
- The labels capture total surface water in flood-event scenes, not necessarily newly inundated water only.
- The project is focused on flood-event scenes, not reservoir transfer settings.

## Acknowledgements

- Bonafilia et al., *Sen1Floods11: A Georeferenced Dataset to Train and Test Deep Learning Flood Algorithms for Sentinel-1* (CVPR Workshops, 2020)
- Copernicus Sentinel-1 and Sentinel-2 missions

## License

Code is released under the MIT License. The Sen1Floods11 dataset has its own licensing terms.
