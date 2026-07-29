# Protocol 01 — AI vs. Human Coding (vs. Neutral Baseline)

## Hypothesis

> _"Frontal theta and gamma power will be higher during AI‑assisted coding than during solo human coding due to increased meta‑cognitive monitoring, integration of external suggestions, and rapid task‑switching demands. In contrast, beta power will be highest during solo coding, reflecting sustained internal problem‑solving effort. Alpha power will be highest during the neutral baseline, reflecting relaxed wakefulness with minimal cognitive load."_

## Design

| Block | Duration | Marker code | Description |
|-------|----------|-------------|-------------|
| BASELINE_EO | 60 s   | 1   | Eyes open, fixation cross |
| BASELINE_EC | 60 s   | 2   | Eyes closed |
| WATCH_NEUTRAL | 3 min | 10 | Sports broadcast, neutral teams |
| REST_1      | 60 s   | 99  | Rest |
| CODE_SOLO   | 5 min  | 20  | Solve programming task without AI |
| REST_2      | 60 s   | 99  | Rest |
| CODE_AI     | 5 min  | 30  | Same task class, with AI assistant |
| BASELINE_EO_END | 60 s | 1 | Eyes open closing baseline |

Counterbalance order of `WATCH_NEUTRAL`, `CODE_SOLO`, `CODE_AI` across subjects.

## Stimuli & task material

- Sports broadcast clip: _https://www.youtube.com/watch?v=nuHEZUEayJU; https://www.youtube.com/watch?v=EGjiKT12JR8_
- Tasks: _https://www.hackerrank.com/challenges/inherited-code/problem?isFullScreen=true; https://www.sql-practice.online/; https://www.hackerrank.com/challenges/exceptional-server/problem?isFullScreen=true;_
- AI tools used: _ChatGPT / Claude / Gemini_

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
