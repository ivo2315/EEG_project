# AI Coding vs. Solo Coding vs. Watching Sports — EEG Case Study Report

---

**Date.** _2026-06-27_
**Research question.** _Does the EEG signature of "programming with an AI assistant" differ from "programming without AI" and from a low-engagement baseline?_
**Hardware.** _Muse 2_
**Data-collection** _LabRecorder XDF_

---

## Abstract

_We investigated whether EEG spectral features can discriminate three cognitive states: AI-assisted coding, solo coding, and watching a sports broadcast. Per-band Welch power features (16 features: 4 channels × 4 bands) were extracted from 2-second epochs and classified with XGBoost._

## 1. Introduction

_The rise of AI coding assistants raises the question of whether they alter the cognitive state of the programmer. If AI assistance reduces active problem-solving load, we would expect changes in frontal theta (working memory) and beta (active cognition) power relative to solo coding. Watching sports serves as a non-cognitive baseline with high visual engagement but minimal executive demand._

_This study tests whether EEG band-power features can discriminate these three states using a lightweight consumer headset._

## 2. Methods

### 2.1 Participants
- 3 participants, 21-25 years, all males
- Participants were recruited from attendees of the Summer School, where the study was introduced on a voluntary basis. All participants provided verbal informed consent prior to taking part in the experiment. Participants were excluded if they were unable to correctly wear or operate the experimental device, did not complete the experimental protocol, or withdrew their consent at any point during the study.

### 2.2 Apparatus
- **Headset:** Muse 2 (InteraXon), dry electrodes, 256 Hz
- **Channels:** TP9, AF7, AF8, TP10
- **Recording:** LSL + LabRecorder → XDF format
- **Software:** BlueMuse 2.4.0.0, LabRecorder 1.17.0

### 2.3 Procedure
Three blocks per session:
- **WATCH** — watching a sports broadcast (passive, visual)
- **SOLO** — coding task without AI assistance
- **AI** — same/similar coding task with AI assistant
- Rests between each action
- Markers sent via custom LSL marker script at block transitions.

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
- Full model: 3-class WATCH / SOLO / AI (chance = 33.3%)
- Challenger: binary SOLO vs AI (chance = 50%)
- Algorithm: XGBoost with early stopping
- Splits: 70/30 stratified + LOSO cross-validation

## 3. Results

_`reports/run_report_case1.md/`_

### 3.1 Classification
| Model | 70/30 Accuracy | LOSO Mean Acc | Chance |
|-------|---------------|---------------|--------|
| Full (WATCH/SOLO/AI) | 75.7% | 63.3% | 33.3% |
| Challenger (SOLO vs AI) | 83.0% | 71.7% | 50.0% |

**70/30 split** shows strong above-chance performance (+42.4% for full, +33% for binary). However, epoch-level splitting risks subject-identity leakage.

**LOSO** is the more honest estimate: 63.3% for 3-class (+30% above chance) and 71.7% for binary (+21.7%) — both meaningful but notably lower than 70/30, confirming some leakage in the epoch-split.

### 3.2 Key observations

- The binary SOLO vs AI distinction is stronger than the 3-class problem, suggesting AI coding produces a reliably different EEG signature from solo coding.
- WATCH condition is likely the easiest to separate (visually-driven, minimal frontal engagement).


## 4. Discussion

## 5. Reproducibility

- **Code:** `EEG_analysis/Case1.ipynb`
- **Data:** `EEG_analysis/data/raw/sub-*/ses-*/eeg/*task-Case1*.xdf`
- **Config:** `EEG_analysis/config.py`
- **To reproduce:** Run all cells in `Case1.ipynb`
