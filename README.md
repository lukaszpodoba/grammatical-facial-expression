# Grammatical Facial Expression Project

## Overview
The **Grammatical Facial Expression (GFE) Project** aims to develop a classification model that recognizes grammatical facial expressions used in Brazilian Sign Language (LIBRAS). These facial expressions are crucial for distinguishing sentence meanings (e.g., differentiating questions from statements) and are more subtle and dynamic than traditional hand gesture analysis.

**Polish Report**: I highly encourage you to read the full Polish report: [Raport Identyfikacja Gestu.pdf](https://github.com/user-attachments/files/18725648/Raport.Identyfikacja.Gestu.pdf)

**Dataset** This project utilizes the dataset available at https://archive.ics.uci.edu/dataset/317/grammatical+facial+expressions

## Extended Target Prediction
In the original dataset, the target value was provided as a binary indicator (0 or 1) that signaled only whether a facial expression was present or not. In our project, we went a step further by predicting which specific facial expression is being displayed, rather than simply indicating its presence. This enhanced approach allows for a more detailed understanding of facial dynamics in LIBRAS.

## Input Data
- **Data Collection**:  
  - Based on 18 recordings captured with a Microsoft Kinect sensor.
  - Each recording features a person performing five different sentences in LIBRAS.
- **Data Details**:  
  - 9 distinct facial expressions (GFEs) recorded for 2 subjects.
  - 100 facial landmarks (covering eyes, eyebrows, mouth, nose, and face contour) with (x, y, z) coordinates.
  - A total of 27,936 video frames analyzed.
- **Labeling**:  
  - Each frame is assigned to one of 9 GFE categories or to a "NaN" class if no specific facial expression is detected.

## Data Challenges
- **Missing Z Coordinates**:  
  - Some facial landmarks have missing z-values due to sensor limitations.
- **Imbalanced Classes**:  
  - The dominant "NaN" class constitutes 65% of the data.
- **High Dimensionality**:  
  - Each sample contains 300 features, which may lead to overfitting.

## Data Processing Methods

### Imputation of Missing Data
- **KNN-Based Imputation**:  
  Two methods were implemented to handle missing z-values:
  - **fill_missing_zeros** Uses KNN on a reference subset of complete frames.
  - **fill_missing_z** Processes each frame individually, filling in missing points based on the available data within the same frame.
- **Outcome:**  
  - The `fill_missing_z` method achieved a lower Mean Squared Error (MSE), enhancing the quality of the input data.
 
### Class Balancing
- **Stratified Data Split** Ensured even distribution of samples across training and test sets.
- **SMOTE (Synthetic Minority Oversampling Technique)** Tested for generating synthetic samples for minority classes.

### Feature Engineering
- **Additional Feature Computation**:  
  - **Facial Segment Lengths** Measurements of features such as eyebrows and mouth.
  - **Distances Between Key Points** For example, distances between the nose and eye, or nose and mouth.
  - **Angles Between Landmarks** Calculated to capture the dynamics of facial expressions.

## Classification Models

### XGBoost
- **Performance**:  
  - Achieved 93% accuracy with an F1-score > 0.86 for most classes.
- **Hyperparameter Tuning:**  
  - Tuned using Optuna over 20 trials.
- **Note**:  
  - SMOTE was not applied as the model performed better without synthetic data augmentation.

### Neural Network
- **Architecture**:  
  - Combines (x, y, z) coordinates for each facial landmark into a unified input.
  - Groups neurons based on facial segments (e.g., eyes, nose, mouth).
  - Includes two additional Dense layers.
- **Specifications**:  
  - Activation Function: Leaky ReLU  
  - Optimizer: AdamW  
  - Regularization: Dropout (0.1)
- **Performance**:  
  - Achieved 93% accuracy with an F1-score > 0.86.
- **Training**:  
  - Provided very stable results, albeit with longer training times.

### Random Forest
- **Approach**:  
  - Utilizes extended features (distances, angles, and lengths).
- **Hyperparameter Optimization**:  
  - Performed via Random Search.
- **Class Balancing**:  
  - SMOTE improved performance in this case.
- **Performance**:  
  - Achieved an overall accuracy of 93.49%, with the highest F1-scores (>0.94) for the 

## Key Technologies
- **Optuna** For automated hyperparameter optimization.
- **SMOTE** For synthetic oversampling of minority classes.
- **Classification Models** XGBoost, Neural Networks, and Random Forest.
- **StandardScaler** Used for feature standardization to improve model stability.
- **Feature Engineering** Incorporation of distances and angles as additional predictive features.
