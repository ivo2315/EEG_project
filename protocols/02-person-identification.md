# Protocol 02 — Person Identification (EEG Biometrics)

## Hypothesis

> _"A classifier trained on resting‑state EEG will identify individual subjects with above‑chance accuracy across recording sessions, and its performance will be higher when using multi‑band spectral features (delta–gamma) than when using any single band alone, reflecting stable, person‑specific patterns in the EEG power spectrum."_

## Design

Same activity recorded across **N ≥ 5** subjects, ideally **two sessions per
subject** at least 24 h apart so train/test sessions are independent.

| Block | Duration | Marker | Description |
|-------|----------|--------|-------------|
| BASELINE_EO | 60 s | 1 | Eyes open, fixation cross |
| BASELINE_EC | 60 s | 2 | Eyes closed |
| TASK        | 3 min | 10 | Scrolling on social media |
| BASELINE_EO_END | 60 s | 1 | Eyes open |

## Subject instructions

>- Participants will first be introduced to the purpose and procedures of the study and will provide informed consent before participation. They will then receive instructions on how to correctly position and use the experimental device to ensure accurate data collection. Finally, participants will be briefed on the study design, including the sequence of tasks they will complete and what they can expect throughout the experimental session.

## Analysis plan (pre-registered)

- Pre-processing: _Six-stage pipeline applied to each XDF recording:_
_1. Sentinel -> NaN (Muse magic values: −1000.0, 999.511719)_
_2. KNN interpolation (inverse-distance weighting, k = 5)_
_3. Bandpass filter: 1–40 Hz (Butterworth order 4, zero-phase)_
_4. Notch filter: 50 Hz (IIR notch, Q = 30)_
_5. Amplitude threshold: |signal| > 150 µV -> NaN -> KNN interpolation_
_6. Linked-mastoid re-reference: subtract mean(TP9, TP10)_
- Features: _e.g., band power per condition, frontal theta, parietal alpha_
- Statistics: _within-subject paired comparison, multiple-comparison correction_

## Notes
