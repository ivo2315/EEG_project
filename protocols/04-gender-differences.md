# Protocol 04 — Sex Differences in Resting-State EEG

## Hypothesis

> _"Female participants will show higher resting‑state alpha power over parietal and temporal regions (e.g., Pz, TP9, TP10) and a slightly higher individual alpha peak frequency than male participants, while male participants will show higher resting‑state beta power over frontal regions (e.g., Fz, F3, F4), reflecting sex‑linked differences in cortical arousal and attentional baseline."_

## Design

| Block | Duration | Marker | Description |
|-------|----------|--------|-------------|
| BASELINE_EO | 3 min | 1 | Eyes open, fixation cross |
| BASELINE_EC | 3 min | 2 | Eyes closed |
| LOW_LOAD    | 3 min | 10 | _low-cognitive-load task, breath counting_ |

## Important caveats

- Sex differences in EEG literature are small, inconsistent, and confounded
  with skull thickness, hair, time of cycle, and many other variables.
- With N < 30 per group, you are running an underpowered study. Frame results
  as exploratory, report effect size + 95 % CI, do not over-claim.

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
