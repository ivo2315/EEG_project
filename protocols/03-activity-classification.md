# Protocol 03 — Activity Classification

## Hypothesis

> _"EEG features will distinguish four cognitively distinct activities (gaming, shopping, market analysis, IQ test) with above‑chance accuracy within‑subject, and classification performance will be highest when using combined spectral–temporal features (across delta–gamma bands) rather than spectral features alone, reflecting task‑specific patterns of cognitive load, attention, and decision‑making."_

## Design

| Block | Duration | Marker | Activity |
|-------|----------|--------|----------|
| BASELINE_EO | 60 s | 1 | Eyes open, fixation cross  | 
| BASELINE_EC | 60 s | 2 | Eyes closed |
| GAME       | 3 min | 10 | Mobile phone game - Water sort |
| REST       | 60 s | 99 | Rest |
| SHOP       | 3 min | 20 | Online shopping task - Temu website |
| REST       | 60 s | 99 | Rest |
| MARKET     | 3 min | 30 | Financial-market analysis - NASDAQ |
| REST       | 60 s | 99 | Rest |
| IQ_TEST    | 3 min | 40 | Reasoning test (Raven, etc.) |
| BASELINE_EO_END | 60 s | 1 | Eyes open |

## Stimuli

- Game: _Game Name: Water Sort part of the android game called "Offline Games" found on google play store, version 3.12.3._
- Shopping site: _https://www.temu.com_
- Market data tool: _https://www.nasdaq.com/_
- IQ test: _https://www.testiqs.com/_

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
