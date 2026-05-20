# Automated Malaria Parasite Detection and Stage Classification

A classical machine learning pipeline to detect and classify *Plasmodium vivax* parasites in blood smear images. The project emphasizes interpretability, resource efficiency, and robustness to extreme class imbalance — suitable for low-resource clinical deployment.

## Table of Contents
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [System Architecture & Workflow](#system-architecture--workflow)
- [Repository Structure](#repository-structure)
- [Quickstart](#quickstart)

## Overview
This repository contains an end-to-end pipeline that uses classical computer vision and machine learning techniques to:

- Extract individual cell crops from high-resolution microscopy fields
- Normalize color/stain variations across sources
- Engineer interpretable features (PCA, color histograms, morphology)
- Train and evaluate models with techniques to handle severe class imbalance

## Problem Statement
Manual microscopy is slow, error-prone, and requires expert personnel. Challenges addressed by this project:

- Object extraction from large imaging fields
- Extreme class imbalance (≈95% uninfected cells)
- Multi-class classification of parasite developmental stages
- Variation in staining and imaging conditions across clinics

## Dataset
Uses the BBBC041v1 dataset (Broad Bioimage Benchmark Collection).

- Total source images: 1,364 high-resolution fields
- Total extracted cells: ~80,000
- Annotations: bounding boxes and class labels from experts
- Classes: Uninfected (RBCs, Leukocytes) and Infected (Rings, Trophozoites, Schizonts, Gametocytes)

## System Architecture & Workflow
Pipeline phases:

1. Data ingestion & object extraction
	- Parse JSON/XML coordinate files
	- Efficiently slice image arrays via a `numpy.ndarray` subclass
	- Save cropped cell images into class folders

2. Preprocessing & normalization
	- Vectorized color normalization to reduce stain/lighting variance
	- Resize all crops to a consistent shape

3. Feature engineering
	- PCA for dimensionality reduction
	- Color histograms for stain information
	- Morphological and texture descriptors (e.g., HOG)

4. Modeling & evaluation
	- Use SMOTE to mitigate class imbalance
	- Train ensembles (Random Forest, Gradient Boosting) and SVMs
	- Tune decision thresholds to prioritize recall (sensitivity)

## Repository Structure
```
├── data/
│   ├── raw/               (Original BBBC041 images and coordinate files)
│   └── processed/         (Extracted and normalized cell arrays)
├── notebooks/
│   ├── 01_eda_and_extraction.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_training_and_evaluation.ipynb
├── src/
│   ├── extract_cells.py   (bounding box parsing)
│   ├── preprocess.py      (color normalization & scaling)
│   ├── features.py        (PCA, histograms, descriptors)
│   └── train.py           (training and tuning)
├── app/                   (Streamlit web interface)
├── requirements.txt       (dependencies)
└── README.md              (this file)
```
