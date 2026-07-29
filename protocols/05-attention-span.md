# Protocol 05 — Attention Span Across Content Types

## Hypothesis

> _"Engagement—indexed by increased frontal theta and decreased parietal alpha—will decline more steeply over time during long entertainment videos than during long instructional videos, whereas mind‑wandering—indexed by increased posterior alpha and reduced frontal beta—will rise earlier and more strongly for entertainment content, reflecting deeper sustained cognitive involvement in instructional material."_

## Design — 2 × 2

|     Video      | Short (≤2 min) | Long (≥10 min) |
|----------------|----------------|----------------|
| Instructional  | block IS       | block IL       |
| Entertainment  | block ES       | block EL       |


| Block | Duration | Marker | Activity |
|-------|----------|--------|----------|
| BASELINE_EO | 60 s | 1 | Eyes open, fixation cross  | 
| BASELINE_EC | 60 s | 2 | Eyes closed |
| SHORT INSTRUCIONAL START     | 2 min | 11 | |
| REST       | 60 s | 99 | Rest |
| LONG INSTRUCTIONAL START       | 5 min | 12 |  |
| REST       | 60 s | 99 | Rest |
|   SHORT ENTERTAINMENT START | 2 min | 21 |  |
| REST       | 60 s | 99 | Rest |
| LONG ENTERTAINMENT START    | 5 min | 22 |  |
| BASELINE_EO_END | 60 s | 1 | Eyes open |

## Stimuli

- IS: _https://www.youtube.com/watch?v=ZoYsQQICPLY_
- IL: _https://www.youtube.com/watch?v=sL5hPovH1vU_
- ES: _https://www.youtube.com/shorts/ZCdjpC0yqF8_
- EL: _https://www.youtube.com/watch?v=Aijtp4hCb38&t=433s_

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
