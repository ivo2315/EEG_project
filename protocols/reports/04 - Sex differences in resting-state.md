# Sex Differences in Resting State — EEG Case Study Report

---

**Date.** _2026-06-27_
**Research question.** _At rest, or during low-cognitive-load tasks, are there reliable EEG differences between male and female participants?_
**Hardware.** _Muse 2_
**Data-collection** _LabRecorder XDF_
---

## Abstract

_We investigated whether resting-state EEG spectral features can classify biological sex (male vs. female). Eight participants (sub-P001–P010, excluding P008 and P009) performed eyes-open and eyes-closed resting-state tasks. Per-band Welch power features (16 features: 4 channels × 4 bands) were extracted from 2-second epochs and classified with XGBoost._

## 1. Introduction

_Prior EEG research reports sex differences in resting-state spectral power: females tend to show higher alpha power and different frontal theta patterns than males. These differences are attributed to hormonal influences, structural brain differences (e.g., corpus callosum), and socialization effects on baseline arousal._

_Whether these differences are detectable with a 4-channel consumer headset — without artifact-rejection pipelines available in clinical systems — is an open practical question with implications for BCI calibration and EEG normalization._

## 2. Methods

### 2.1 Participants
- 6 participants, 25-40 years, 3 female, 3 male
- Participants were recruited from attendees of the Summer School, where the study was introduced on a voluntary basis. All participants provided verbal informed consent prior to taking part in the experiment. Participants were excluded if they were unable to correctly wear or operate the experimental device, did not complete the experimental protocol, or withdrew their consent at any point during the study.

### 2.2 Apparatus
- **Headset:** Muse 2 (InteraXon), dry electrodes, 256 Hz
- **Channels:** TP9, AF7, AF8, TP10
- **Recording:** LSL + LabRecorder → XDF format
- **Software:** BlueMuse 2.4.0.0, LabRecorder 1.17.0

### 2.3 Procedure
Resting-state paradigm:
- **Eyes-Open (EO)** baseline — marker 1 (≥60 s)
- **Eyes-Closed (EC)** baseline — marker 2 (≥60 s)
- Low-activity task (counting breath intakes)

Order: EO ->  EC, consistent across participants.

### 2.4 Pre-processing

1. Sentinel -> NaN (Muse magic values: −1000.0, 999.511719)
2. KNN interpolation (k=5, inverse-distance weighting)
3. Bandpass 1–40 Hz (Butterworth order 4, zero-phase)
4. Notch 50 Hz (IIR, Q=30)
5. Amplitude threshold ±150 µV -> NaN -> KNN interpolation
6. Linked-mastoid re-reference: subtract mean(TP9, TP10)

Epoch rejection: 2-second windows with PTP > 420 µV discarded.

### 2.5 Analysis

**Features:** 16 per-band Welch power features (delta/theta/alpha/beta × TP9/AF7/AF8/TP10)

**Model:**
- Binary classification: Male vs. Female (chance = 50%)
- Algorithm: XGBoost (max_depth regularized, early stopping 50 rounds)
- Splits: 70/30 stratified + LOSO (8-fold, one subject held out per fold)
- Metrics: Accuracy, Balanced Accuracy, Macro F1, Cohen's κ, MCC

**Feature selection:** Gini 90% cumulative importance + permutation importance

## 3. Results

_`reports/run_report_case4.md/`_

## 4. Discussion

## 5. Reproducibility

- **Code:** `EEG_analysis/Case4.ipynb`
- **Data:** `EEG_analysis/data/raw/sub-*/ses-*/eeg/*task-Case4*.xdf`
- **Config:** `EEG_analysis/config.py`
- **To reproduce:** Run all cells in `Case4.ipynb`
