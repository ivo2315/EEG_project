# Activity Classificication — EEG Case Study Report

---

**Date.** _2026-06-27_
**Research question.** _Can EEG distinguish between qualitatively different cognitive activities?_
**Hardware.** _Muse 2_
**Data-collection** _LabRecorder XDF_

---

## Abstract

_We investigated whether EEG spectral features can classify four cognitively distinct activities: video gaming, online shopping, financial market analysis, and an IQ test. Per-band Welch power features (16 features: 4 channels × 4 bands) were extracted from 2-second epochs and classified with XGBoost._

## 1. Introduction

_Different cognitive activities engage distinct neural circuits: gaming involves rapid visuomotor processing and reward circuitry; shopping engages value-based decision-making; financial analysis requires analytical reasoning and sustained attention; IQ tests demand fluid intelligence and working memory. If these differences manifest in scalp EEG spectra, a passive BCI could classify ongoing mental activity without explicit user input._

## 2. Methods

### 2.1 Participants
- 4 participants, 25-40 years, 1 female, 3 male
- Participants were recruited from attendees of the Summer School, where the study was introduced on a voluntary basis. All participants provided verbal informed consent prior to taking part in the experiment. Participants were excluded if they were unable to correctly wear or operate the experimental device, did not complete the experimental protocol, or withdrew their consent at any point during the study.

### 2.2 Apparatus
- **Headset:** Muse 2 (InteraXon), dry electrodes, 256 Hz
- **Channels:** TP9, AF7, AF8, TP10
- **Recording:** LSL + LabRecorder → XDF format
- **Software:** BlueMuse 2.4.0.0, LabRecorder 1.17.0

### 2.3 Procedure
Four activity blocks per session:
- **GAME** — video gaming
- **SHOP** — online shopping
- **MARKET** — financial market analysis
- **IQ_TEST** — standardized intelligence test items

Additional baseline/rest markers included in full 7-class model.
Markers sent via custom LSL script at block transitions.

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

**Models:**
- Full model: 7-class (all markers including baselines/rest), chance = 14.3%
- Challenger: 4-class (GAME/SHOP/MARKET/IQ_TEST), chance = 25%
- Algorithm: XGBoost with early stopping, regularization
- Splits: 70/30 stratified + LOSO cross-validation

## 3. Results

_`reports/run_report_case3.md/`_

## 4. Discussion

## 5. Reproducibility

- **Code:** `EEG_analysis/Case3.ipynb`
- **Data:** `EEG_analysis/data/raw/sub-*/ses-*/eeg/*task-Case3*.xdf`
- **Config:** `EEG_analysis/config.py`
- **To reproduce:** Run all cells in `Case3.ipynb`
