# IMU Sitting Postures Dataset

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

## Overview

This repository contains the **IMU Sitting Postures Dataset**, a publicly available dataset of inertial measurement unit (IMU) recordings collected from 46 healthy Egyptian university students, of whom 45 participants are included in the final released dataset following quality assurance. Participants performed six predefined static sitting posture conditions during a controlled, gamified experimental session.

Data were recorded using four **WITMotion WT901BLECL** IMU sensors placed at anatomically relevant spinal landmarks (C7, T4, T12, and L5), sampled at **50 Hz**. The dataset is intended to support machine learning research in wearable-based posture classification, ergonomics, and real-time postural monitoring systems.

---

## Dataset at a Glance

| Property | Value |
|---|---|
| Participants | 46 recruited; 45 included (25 F / 21 M; 1 excluded during quality assurance) |
| Age range | 19–24 years (mean: 21.89 ± 0.85) |
| Sensors | 4 × WITMotion WT901BLECL IMUs |
| Sensor locations | C7, T4, T12, L5 |
| Sampling rate | 50 Hz |
| Posture classes | 6 |
| Trials per participant | 18 (each posture × 3 repetitions) |
| Trial duration | 30 seconds per trial |
| Total recordings | 828 trials collected; 810 included (45 subjects × 18 trials) |
| Data formats | CSV, BIN, WPLAY, MAT |

---

## Posture Classes

| ID | Label | Anatomical Description |
|---|---|---|
| P1 | Backward Bending | Posterior trunk extension; torso leaning behind the vertical axis |
| P2 | Upright | Neutral seated position; head, shoulders, and hips vertically aligned |
| P3 | Slouching | Increased thoracic kyphosis; shoulders rolled forward, collapsed lumbar region |
| P4 | Forward Bending | Anterior trunk flexion from the hips; chest moving toward the desk |
| P5 | Right Bending | Lateral spinal flexion toward the right; right shoulder depressed |
| P6 | Left Bending | Lateral spinal flexion toward the left; left shoulder depressed |

---

## Repository Structure

```
IMU_Sitting_Postures_Dataset/
├── README.md                        ← This file
├── participants.tsv                 ← Demographic data and trial sequence mapping
├── sub_01/
│   ├── sub_01_trial1/               ← Trial 1 (chronologically first)
│   │   ├── data_0.csv               ← Primary IMU data file (4 sensors, time-synchronized)
│   │   ├── data_0.bin               ← Raw binary data (preserved for reproducibility)
│   │   ├── data.wplay               ← WitMotion playback file for visual review
│   │   └── Matlab/
│   │       ├── data                 ← MATLAB-formatted version of the same data
│   │       └── readMatData.m        ← MATLAB loader script
│   ├── sub_01_trial2/
│   │   └── ... (same structure)
│   └── ... (18 trial folders total, ordered chronologically)
├── sub_02/
│   └── ... (18 trial folders)
└── ... (sub_03 through sub_46)
```

> **Note:** Trial folders are named by chronological order of recording (`trial1` = first trial performed), **not** by posture class. To identify which posture class was performed in each trial, refer to the `participants.tsv` file, which maps each participant's chronological trial order to their posture sequence.

---

## Data Format

### Primary Data File: `data_0.csv`

Each `data_0.csv` file contains time-synchronized IMU readings from all four sensors. Each row corresponds to one sensor reading at one timestamp. Since four sensors were used simultaneously, each timestamp appears in **four consecutive rows**, identified by the `Device name` column (the sensor's MAC address).

| Column | Type | Units | Description |
|---|---|---|---|
| Time | string | HH:MM:SS.ms | Local timestamp of recording |
| Device name | string | — | MAC address identifying each sensor |
| Chip Time | datetime | YYYY-MM-DD HH:MM:SS.ms | Internal sensor timestamp |
| Acceleration X(g) | float | g | Acceleration along the superior–inferior axis |
| Acceleration Y(g) | float | g | Acceleration along the mediolateral axis |
| Acceleration Z(g) | float | g | Acceleration along the anteroposterior axis (normal to the sensor surface) |
| Angular velocity X(°/s) | float | °/s | Angular velocity about the superior–inferior axis |
| Angular velocity Y(°/s) | float | °/s | Angular velocity about the mediolateral axis |
| Angular velocity Z(°/s) | float | °/s | Angular velocity about the anteroposterior axis |
| Angle X(°) | float | degrees | Roll (rotation about the superior–inferior axis) |
| Angle Y(°) | float | degrees | Pitch (rotation about the mediolateral axis) |
| Angle Z(°) | float | degrees | Yaw (rotation about the anteroposterior axis) |
| Magnetic field X(µT) | float | µT | Magnetic field along the superior–inferior axis |
| Magnetic field Y(µT) | float | µT | Magnetic field along the mediolateral axis |
| Magnetic field Z(µT) | float | µT | Magnetic field along the anteroposterior axis |
| Temperature (°C) | float | °C | Sensor temperature |
| Quaternions 0 | float | — | Quaternion scalar (w) component |
| Quaternions 1 | float | — | Quaternion x component |
| Quaternions 2 | float | — | Quaternion y component |
| Quaternions 3 | float | — | Quaternion z component |

### Participant Metadata: `participants.tsv`

| Column | Type | Description |
|---|---|---|
| Person | string | Anonymized participant ID (sub_01 to sub_46) |
| Sequence | string | 18-character code mapping chronological trial order to posture classes |
| Labeled | string | Whether data has been verified and labeled |
| Consent Form | string | Consent status |
| Survey | string | Survey completion status |
| Weight | float | Weight in kilograms (self-reported) |
| Height | float | Height in centimeters (self-reported) |
| Age | integer | Age in years |
| Handedness | string | Right-handed or left-handed |

---

## Sensor Coordinate System and Orientation

The WT901BLECL IMUs record measurements in their own local coordinate system. During data collection, all four sensors (C7, T4, T12, and L5) were mounted in a fixed and standardized orientation within the custom wearable brace to establish a consistent relationship between the sensor-local axes and the participant's anatomical reference frame.

Unlike studies that perform post-processing coordinate-frame alignment, **no additional mathematical rotation matrix or coordinate transformation was applied during preprocessing**. Instead, anatomical consistency was achieved through standardized sensor placement during dataset acquisition.

The adopted sensor orientation is summarized below.

| Recorded Signal | Axis | Anatomical Direction | Positive Direction |
|-----------------|------|----------------------|--------------------|
| **Acceleration** | X | Superior–Inferior | Superior (toward the head) |
| | Y | Mediolateral | Left |
| | Z | Anteroposterior (normal to the sensor surface) | Posterior (away from the participant) |
| **Angular Velocity** | X | Rotation about the Superior–Inferior axis | Positive according to the WT901BLECL right-hand rule |
| | Y | Rotation about the Mediolateral axis | Positive according to the WT901BLECL right-hand rule |
| | Z | Rotation about the Anteroposterior axis | Positive according to the WT901BLECL right-hand rule |
| **Euler Angles** | X (Roll) | Rotation about the Superior–Inferior axis | Positive according to the WT901BLECL convention |
| | Y (Pitch) | Rotation about the Mediolateral axis | Positive according to the WT901BLECL convention |
| | Z (Yaw) | Rotation about the Anteroposterior axis | Positive according to the WT901BLECL convention |

All four sensors were mounted using this identical orientation, allowing measurements from corresponding axes to remain directly comparable across participants. The sensor orientation illustrated in **Figure 3** of the accompanying manuscript was maintained consistently throughout dataset acquisition.

The coordinate definitions described above apply consistently to the acceleration, angular velocity, Euler angle, magnetometer, and quaternion measurements reported by each sensor.

---

## Recommended Preprocessing

The following preprocessing pipeline is recommended for consistent and optimal use of this dataset:

1. **Noise attenuation:** Apply a 2nd-order Butterworth low-pass filter with a cutoff frequency of approximately 3 Hz to remove high-frequency noise from raw IMU signals.
2. **Segmentation:** Use a sliding window approach with a window length of **200 samples** and **50% overlap** to generate temporal segments from the continuous 50 Hz streams.
3. **Normalization:** Apply per-sensor normalization to standardize signal scales and account for inter-subject variability.
4. **Missing data:** Apply basic linear interpolation to address minor sample gaps caused by Bluetooth transmission losses.

---


## Machine and Deep Learning Models

### Machine Learning Model for Posture Classification

The machine learning pipeline uses a **Random Forest (RF) classifier** for six-class sitting posture classification. The pipeline consists of windowing, feature engineering, feature selection, model training, and evaluation.

#### ML Pipeline

1. **Windowing:** IMU data are segmented into windows of **100 samples** with **50% overlap** for the ML pipeline.
2. **Feature Engineering:** Handcrafted biomechanical features are extracted from the four IMU sensors.
3. **Feature Selection:** A wrapper-based feature selection approach evaluates feature blocks using LOSO performance as the selection criterion.
4. **Model Training:** A Random Forest classifier is trained using the selected features.
5. **Evaluation:** Performance is evaluated using both train/validation/test splits and **Leave-One-Subject-Out (LOSO)** cross-validation.

#### Handcrafted Feature Structure

The feature engineering stage is organized into **8 biomechanical feature blocks**, with **109 handcrafted features** extracted per window:

| Block | Feature Group | Description |
|---|---|---|
| B1 | Per-sensor statistics | Mean, standard deviation, and RMS of acceleration-related signals across the 4 sensors |
| B2 | C7 vs L5 angle difference | Mean and standard deviation of C7 − L5 roll and pitch differences |
| B3 | Pitch & roll statistics | Mean, standard deviation, and temporal slope per sensor |
| B4 | Acceleration magnitude | Mean, standard deviation, maximum, and minimum acceleration magnitude across the 4 sensors |
| B5 | Spinal curvature | Quadratic fit across the pitch values of the 4 spinal sensors |
| B6 | Linear spine slope | Spatial pitch gradient along the spine |
| B7 | Sign agreement | C7 × L5 tilt-direction agreement for S-curve detection |
| B8 | Slouch discriminators | L5−T12 and T4 gradients together with acceleration-Z cross-difference features |

Feature importance analysis across LOSO folds identified **acceleration Y, Z, and X** and **Euler-angle pitch and roll** among the most important parameters.

#### Random Forest Configuration

The selected Random Forest configuration is:

| Parameter | Value |
|---|---|
| `n_estimators` | 500 |
| `max_depth` | 5 |
| `min_samples_leaf` | 5 |
| `max_features` | sqrt(features) |
| `class_weight` | balanced |
| `bootstrap` | True |

The best validation configuration achieved a validation accuracy of **0.8380**.

#### Wrapper-Based Feature Selection

Feature selection was performed at the feature-block level using **LOSO accuracy** as the model-performance criterion. The process starts with all feature blocks, evaluates block subsets, and removes blocks that reduce generalization performance. The reported selected configuration retains **B1 (Per-sensor statistics)** and **B4 (Acceleration magnitude)**, resulting in **76 retained features** in the selected representation.

---

### Deep Learning Model for Posture Classification

The deep learning approach uses a **CNN-based model** designed to process the four IMU sensors separately before performing sensor attention, sensor fusion, temporal aggregation, and final posture classification.

#### CNN Pipeline

1. **Input:** A multi-sensor IMU sequence with shape **(B, T, 24)**, where B is batch size and T is the number of time steps. The 24 input channels correspond to 6 features from each of the 4 sensors.
2. **Sensor Split:** The input is separated into four sensor streams: **L5, T4, C7, and T12**, each with shape **(B, T, 6)**.
3. **Per-Sensor CNN Extraction:** Each sensor is processed by the same sensor block in parallel.
4. **Feature Attention:** Learned attention weights are applied to the extracted features within each sensor.
5. **Sensor Stacking:** The four sensor representations are stacked to form **(B, T, 4, 32)**.
6. **Sensor Attention:** Attention weights are learned across the four sensors to perform sensor weighting.
7. **Sensor Fusion:** The weighted sensor representations are summed to produce a fused representation of **(B, T, 32)**.
8. **Temporal Pooling:** Global Average Pooling 1D produces the temporal representation.
9. **Classifier:** Dense layers with ReLU activations, dropout, and a final 6-class softmax layer produce the posture prediction.

#### CNN Sensor Block

Each of the four sensors is processed in parallel using:

- `Conv1D(32, kernel_size=5, activation=ReLU)`
- Batch Normalization
- `Conv1D(32, kernel_size=3, activation=ReLU)`
- Batch Normalization
- Feature attention using `Dense(32, tanh)` → `Softmax` → feature weighting
- Output representation: **(B, T, 32)**

The model applies **sensor dropout of 0.25** at the input.

#### CNN Classifier

After sensor attention and temporal pooling, the classifier consists of:

- `Dense(128)` + ReLU
- `Dropout(0.4)`
- `Dense(64)` + ReLU
- `Dropout(0.3)`
- `Dense(6)` + Softmax

The model contains **101,735 trainable parameters**.

#### CNN Training Configuration

| Parameter | Value |
|---|---|
| Loss | Categorical Cross-Entropy |
| Label smoothing | 0.05 |
| Optimizer | Adam |
| Learning rate | 1e-4 |
| Batch size | 32 |
| Maximum epochs | 60 |
| Early stopping | Patience = 5 |
| Learning-rate scheduler | ReduceLROnPlateau |
| LR reduction factor | 0.5 |
| LR scheduler patience | 3 |
| Minimum learning rate | 1e-6 |
| Window length | 200 samples |
| Window overlap | 50% |
| Normalization | Per-sensor mean and standard deviation, computed on training data only |
| Validation strategy | LOSO, with train/validation split within the training subjects |

#### CNN Evaluation

The CNN was evaluated using both random subject splits and **Leave-One-Subject-Out (LOSO)** evaluation. Under LOSO evaluation, **45 subjects** were used as independent test runs, with the held-out subject completely unseen during training.

The reported LOSO results are:

| Metric | CNN |
|---|---:|
| Subject-Level Accuracy | 88.5% |
| Window-Level Accuracy | 86.5% |
| Macro F1-Score | 0.86 |

The per-class F1-scores under LOSO evaluation are:

| Posture | F1-Score |
|---|---:|
| Backward Bending | 0.98 |
| Upright | 0.95 |
| Slouching | 0.70 |
| Forward Bending | 0.70 |
| Right Bending | 0.89 |
| Left Bending | 0.92 |

The main confusion observed in the CNN results is between **Forward Bending and Slouching**, while the other posture classes show comparatively stronger discrimination.

## Baseline Classification Performance

The dataset was validated using a Convolutional Neural Network (CNN) trained with Leave-One-Subject-Out (LOSO) cross-validation.

| Metric | Value |
|---|---|
| Overall Window-Level Accuracy | 86.5% |
| Overall Subject-Level Accuracy | 88.5% |

| Posture | Precision | Recall | F1-Score |
|---|---|---|---|
| Backward Bending | 0.97 | 0.99 | 0.98 |
| Upright | 0.96 | 0.94 | 0.95 |
| Slouching | 0.68 | 0.73 | 0.70 |
| Forward Bending | 0.71 | 0.70 | 0.70 |
| Right Bending | 0.88 | 0.89 | 0.89 |
| Left Bending | 0.95 | 0.88 | 0.92 |

Full model implementation is available in the companion repository:

https://github.com/Ayat-Tarek/GP-DL-MODEL

---

## Known Limitations

- Data were collected under a **structured experimental setup** with predefined postures, which may not fully represent natural real-world sitting behavior.
- The dataset is limited to **healthy young adults aged 19–24** and does not include individuals with spinal deformities or musculoskeletal disorders.
- The sample size of **46 recruited participants (45 included)** may limit the robustness of models trained solely on this data.
- Recording sessions were of **limited duration** (30 seconds per trial) and do not capture long-term fatigue or posture drift over extended periods.
- One participant was excluded during quality assurance after being identified as a statistical outlier during participant-level screening. Independent review of the corresponding recordings revealed improper sensor placement caused by thick clothing, resulting in a protocol deviation that compromised data quality.

---

## Citation

If you use this dataset in your research, please cite the following:

```
Ali, A., Elsayed, N., Emad, M., Mohamed, S., Shehata, B., & Rehan, A. Y. (2026).
IMU Sitting Postures Dataset. GitHub / Zenodo.
https://github.com/nariman-ahmed/IMU-Sitting-Postures-Dataset
DOI: https://doi.org/10.5281/zenodo.20643339
```

---

## License

This dataset is distributed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.

You are free to share and adapt the material for any purpose, provided appropriate credit is given to the original authors.  
Full license text: [https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/)

---

## Acknowledgements

The authors thank all participants for their time and contribution. This work was supervised by **Dr. Aliaa Rehan** (Cairo University) and supported by the **Information Technology Academia Collaboration (ITAC)** program and **Cairo University**.
