Automated Malaria Parasite Detection and Stage Classification
Project Overview
This repository contains an end-to-end machine learning pipeline designed to detect and classify Plasmodium vivax malaria parasites in microscopic blood smear images. Built entirely using classical machine learning techniques, this project demonstrates advanced computer vision feature extraction, rigorous memory management of image arrays, and robust handling of extreme class imbalance.

By avoiding black-box deep learning models, this project provides a highly interpretable, resource-efficient solution suitable for deployment on edge devices in low-resource clinical settings.

Problem Statement
Malaria diagnosis traditionally relies on manual microscopic examination of blood smears. This process is time-consuming, prone to human error, and requires highly trained personnel. Automating this process using standard machine learning libraries presents multiple challenges:

Object Extraction: Parsing metadata to crop thousands of individual cells from large, raw microscopy fields.

Extreme Class Imbalance: Over 95% of the cells in the dataset are uninfected healthy cells.

Multi-Class Complexity: The system must differentiate between healthy cells and four distinct developmental stages of the parasite.

Staining Variance: Blood smears collected from different global clinics exhibit varying lighting and chemical staining profiles.

Dataset Details
This project utilizes the BBBC041v1 dataset from the Broad Bioimage Benchmark Collection.

Total Source Images: 1,364 high-resolution microscopic fields of view.

Total Extracted Cells: Approximately 80,000 individual cells.

Annotations: Bounding box coordinates and class labels provided by expert malaria researchers.

Classes:

Uninfected: Red Blood Cells (RBCs), Leukocytes.

Infected: Rings, Trophozoites, Schizonts, Gametocytes.

System Architecture and Workflow
The machine learning pipeline is structured into four distinct phases:

1. Data Ingestion and Object Extraction
Parses JSON/XML files containing bounding box coordinates.

Utilizes custom numpy.ndarray subclassing to slice and extract individual cell matrices from the source images efficiently.

Filters and routes cropped arrays into designated class directories.

2. Preprocessing and Normalization
Applies vectorized color normalization to standardize Giemsa stain variations across batches from Brazil and Southeast Asia.

Resizes all extracted cell matrices to a uniform dimension.

3. Feature Engineering
Extracts mathematical representations of the images using classical computer vision techniques:

Principal Component Analysis (PCA): Reduces the dimensionality of flattened image arrays while preserving structural variance.

Color Histograms: Captures the unique color distribution of the purple parasite stains against the pink cell background.

Morphological Features: Extracts localized intensity statistics and edge-detection metrics (e.g., Histogram of Oriented Gradients).

4. Modeling and Evaluation
Implements SMOTE (Synthetic Minority Over-sampling Technique) to balance the extreme 95-to-5 class distribution.

Trains ensemble models (Random Forest, Gradient Boosting) and Support Vector Machines.

Prioritizes Recall (Sensitivity) through custom decision threshold tuning to minimize false negatives.

Repository Structure
Plaintext
├── data/
│   ├── raw/               (Original BBBC041 images and coordinate files)
│   ├── processed/         (Extracted and normalized cell arrays)
├── notebooks/             
│   ├── 01_eda_and_extraction.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_training_and_evaluation.ipynb
├── src/                   
│   ├── extract_cells.py   (Script for bounding box parsing)
│   ├── preprocess.py      (Color normalization and scaling)
│   ├── features.py        (PCA and histogram extraction)
│   └── train.py           (Model training and hyperparameter tuning)
├── app/                   (Streamlit web interface code)
├── requirements.txt       (Project dependencies)
└── README.md              (Project documentation)
