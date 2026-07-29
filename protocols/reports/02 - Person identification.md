# Person Identification from EEG — Case Study Report

---

**Date.** _2026-06-27_
**Research question.** _Given recordings of the same activity across multiple people, can a model identify the individual from EEG alone?_
**Hardware.** _Muse 2_
**Data-collection** _LabRecorder XDF_
---

## Abstract

_We investigated whether individual participants can be identified from their EEG spectral features — a form of biometric identification. Per-band Welch power features (16 features: 4 channels × 4 bands) were extracted from 2-second epochs. Two models were trained: a Session model (trained on Session 1, validated on Session 2) and a Challenge model (stratified 70/30 split). XGBoost was used for both._

## 1. Introduction

_EEG signals carry individual-specific features — differences in skull thickness, cortical folding, and habitual brain states create a unique spectral signature per person. This has potential applications in passive biometric authentication and also raises privacy concerns for EEG-based interfaces._

_The key question is whether a model trained in one session generalizes to a second session of the same activity — a stricter test than within-session identification._

## 2. Methods

### 2.1 Participants
- 8 participants, 21-40 years, 3 female, 5 male
- Participants were recruited from attendees of the Summer School, where the study was introduced on a voluntary basis. All participants provided verbal informed consent prior to taking part in the experiment. Participants were excluded if they were unable to correctly wear or operate the experimental device, did not complete the experimental protocol, or withdrew their consent at any point during the study.

### 2.2 Apparatus
- **Headset:** Muse 2 (InteraXon), dry electrodes, 256 Hz
- **Channels:** TP9, AF7, AF8, TP10
- **Recording:** LSL + LabRecorder → XDF format
- **Software:** BlueMuse 2.4.0.0, LabRecorder 1.17.0

### 2.3 Procedure
Participants performed the same activity in two separate recording sessions (S1 and S2). The inter-session gap allows testing temporal stability of EEG fingerprints.

Markers used for condition segmentation within each session.

### 2.4 Pre-processing

1. Sentinel -> NaN (Muse magic values)
2. KNN interpolation (k=5, inverse-distance weighting)
3. Bandpass 1–40 Hz (Butterworth order 4, zero-phase)
4. Notch 50 Hz (IIR, Q=30)
5. Amplitude threshold ±150 µV -> NaN -> KNN interpolation
6. Linked-mastoid re-reference: subtract mean(TP9, TP10)

Epoch rejection: 2-second windows with PTP > 420 µV discarded.

### 2.5 Analysis
**Features:** 16 per-band Welch power features (delta/theta/alpha/beta × TP9/AF7/AF8/TP10)

**Two-model design:**
- **Session Model:** Trained on S1, validated on S2 — tests cross-session generalization
- **Challenge Model:** Stratified 70/30 split by subject × marker — within-session upper bound

**Algorithm:** XGBoost (multi-class, one class per subject)
**Chance level:** 1/N where N = number of subjects

**Feature selection:** Gini 90% cumulative importance + permutation importance ≥ 7% of max

## 3. Results

_`reports/run_report_case2.md/`_

## 4. Discussion

## 5. Reproducibility

- **Code:** `EEG_analysis/Case2.ipynb`, `EEG_analysis/Case2_simple.ipynb`
- **Data:** `EEG_analysis/data/raw/sub-*/ses-*/eeg/*task-Case2*.xdf`
- **Config:** `EEG_analysis/config.py`
- **To reproduce:** Run all cells in `Case2.ipynb`
