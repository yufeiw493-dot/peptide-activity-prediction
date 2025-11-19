# Peptide Activity Prediction

This repository contains a machine learning pipeline for predicting peptide-related properties
(e.g., activity or stability) using a combination of molecular, peptide and enzyme–cleavage
descriptors. It was developed as part of an AI-driven peptide drug design project.

## Overview

Given a dataset of peptides with:

- amino acid sequences
- SMILES representations
- experimental measurements (e.g., temperature, half-life or other activity-related endpoints),

this project:

1. Computes a rich set of **molecular descriptors** from SMILES.
2. Computes **peptide-level and protein-style descriptors** from sequences.
3. Optionally extracts **enzyme cleavage–related features** from online resources.
4. Builds and evaluates multiple regression models to predict the target property.

The pipeline is implemented in Python and relies on scikit-learn for model training and evaluation.

## Main Features

- Descriptor generation using:
  - RDKit-based molecular descriptors
  - PyBioMed molecular descriptors (e.g., Basak, E-state, MOE, CATS2D)
  - modlamp global peptide descriptors
  - PyProtein amino-acid composition and triad descriptors
  - (Optional) Expasy PeptideCutter–based enzyme cleavage features

- Flexible descriptor combinations:
  - `MoleculeDesc`
  - `MoleculeAndPeptideDesc`
  - `AllDesc` (molecule + peptide + enzyme descriptors)

- Multiple regression models:
  - Partial Least Squares Regression (PLS)
  - Support Vector Regression (SVR)
  - Random Forest Regressor
  - Gradient Boosting Regressor
  - Decision Tree Regressor

- Hyperparameter search and feature selection:
  - `SelectKBest` based on mutual information
  - `GridSearchCV` with either:
    - Repeated K-Fold CV (`kfold` mode)
    - Leave-One-Out CV (`loo` mode)

- Evaluation metrics:
  - Pearson correlation coefficient (r)
  - R²
  - MAE, MSE, RMSE
  - Cross-validation scores on the training set

## Project Structure

```text
peptide-activity-ml/
├── datasets.py      # Dataset class, SPXY split, scaling, and train/test preparation
├── descriptors.py   # Descriptor generation (molecular, peptide, and enzyme-based features)
├── myModel.py       # Model definitions and GridSearchCV wrappers for each regressor
├── run.py           # Main script: loops over tasks, descriptor sets, transforms and models
└── utils.py         # Utilities for data cleaning, SMILES/sequence checks and metric functions
