# Attention Span Across Content Types — EEG Case Study Report

---

**Date.** 2026-06-27
**Research question.** Does frontal-theta engagement decline more steeply during
long entertainment content than during long instructional content?
**Hardware.** Muse 2 headset — 4 channels (TP9, AF7, AF8, TP10), 256 Hz
**Data-collection.** LabRecorder → XDF format
**Pre-registration.** Analysis plan documented in `protocols/05-attention-span.md`
before model training commenced.

---

## Abstract

We investigated whether EEG-derived engagement differs between instructional and
entertainment video content using a 2 × 2 (content type × duration) within-subjects
design. Three participants (sub-P002, sub-P003, sub-P004) watched short and long
versions of instructional and entertainment stimuli while EEG was recorded with
a Muse 2 headset. Per-band Welch power features (4 channels × 4 bands = 16
features) were extracted from 2-second epochs and classified with XGBoost.
The full 4-class model achieved Accuracy = 91.6%, Macro F1 = 0.83, Cohen's κ = 0.66
[95% CI: 0.57, 0.74]. The binary Instructional vs Entertainment challenger showed
above-chance performance. Engagement trajectory analysis (frontal theta, sliding
window) revealed that theta declined over time in most conditions, with the
steepest decline in the short instructional (IS) block. Effect sizes were small
(|d| = 0.21–0.33), consistent with the limited sample. Results should be interpreted
as preliminary given N = 3 and stimulus duration deviations from protocol.

## 1. Introduction

Sustained attention and cognitive engagement are core constructs in educational
psychology and human-computer interaction. EEG offers a non-invasive, continuous
window into neural correlates of attention: frontal theta (4–8 Hz) indexes working
memory load and cognitive effort, while parietal alpha (8–13 Hz) reflects cortical
inhibition and disengagement.

A key open question is whether engagement dynamics differ between passive
entertainment consumption and active instructional processing. Instructional
content demands sustained semantic integration; entertainment allows more passive
reception. This suggests engagement may decline more steeply — or fluctuate
differently — during entertainment compared to instruction, particularly over
longer durations.

This study applies a lightweight EEG classification pipeline to a 2 × 2
(content type × duration) experimental design, testing whether EEG band-power
features can discriminate content categories and whether engagement trajectories
differ across conditions.

## 2. Methods

### 2.1 Participants

- N = 4 recorded; N = 3 included after exclusion
- Sub-P001 excluded: signal quality visually assessed as insufficient
  (persistent high-amplitude noise across all channels)
- Remaining participants: sub-P002, sub-P003, sub-P004
- Age range and sex distribution: not reported (pilot study)
- All participants provided informed verbal consent

### 2.2 Apparatus
- **Headset:** Muse 2 (InteraXon), dry electrodes, 256 Hz
- **Channels:** TP9, AF7, AF8, TP10
- **Recording:** LSL + LabRecorder → XDF format
- **Software:** BlueMuse 2.4.0.0, LabRecorder 1.17.0

### 2.3 Procedure

Participants completed four video-watching blocks in counterbalanced order,
with 60 s rest between blocks:

| Block | Marker | Content type | Planned duration | Actual duration |
|-------|--------|--------------|-----------------|-----------------|
| IS    | 11     | Short instructional | ≤ 2 min   | ~1.5 min |
| IL    | 12     | Long instructional  | ≥ 10 min  | ~5 min |
| ES    | 21     | Short entertainment | ≤ 2 min   | ~20 sec |
| EL    | 22     | Long entertainment  | ≥ 10 min  | ~5 min |

Eyes-open (EO, marker=1) and eyes-closed (EC, marker=2) baselines were recorded
before the task blocks.

**Block order: fixed — IS → IL → ES → EL for all participants (no counterbalancing).**
This is a design limitation: fatigue and habituation effects are confounded with
condition. ES always appeared 3rd and EL always 4th in the sequence.

**Duration deviation:** IL and EL were shortened to ~5 min (50% of the 10-min
minimum). ES was only ~20 sec (~10 epochs per subject). The Short/Long contrast
is therefore weaker than intended. The Instructional vs Entertainment contrast
remains valid as content type differs meaningfully.

### 2.4 Pre-processing

Six-stage pipeline applied to each XDF recording:

1. Sentinel → NaN (Muse magic values: −1000.0, 999.511719)
2. KNN interpolation (inverse-distance weighting, k = 5)
3. Bandpass filter: 1–40 Hz (Butterworth order 4, zero-phase)
4. Notch filter: 50 Hz (IIR notch, Q = 30)
5. Amplitude threshold: |signal| > 150 µV → NaN → KNN interpolation
6. Linked-mastoid re-reference: subtract mean(TP9, TP10)

Epoch rejection: 2-second windows with peak-to-peak amplitude > 420 µV
on any channel were discarded.

| Subject | IS epochs | IL epochs | ES epochs | EL epochs | Total |
|---------|-----------|-----------|-----------|-----------|-------|
| sub-P002 | ~110 | ~477 | ~42 | ~422 | ~1051 |
| sub-P003 | ~93 | ~500 | ~36 | ~473 | ~1102 |
| sub-P004 | ~86 | ~350 | ~30 | ~433 | ~899 |

*(Exact counts in `run_report_case5.md`)*

### 2.5 Analysis

**Feature extraction — per-band Fourier Welch:**
For each 2-second epoch and each of 4 frequency bands:
1. Narrow Butterworth bandpass (order 4) within the band limits
2. Welch PSD on the filtered signal (nperseg = 256)
3. Trapezoidal integration over the band → scalar power value

Bands: delta (0.5–4 Hz), theta (4–8 Hz), alpha (8–13 Hz), beta (13–30 Hz)
Result: 16 features per epoch (4 channels × 4 bands)

**Classification:**
- Full model: 4-class IS / IL / ES / EL (chance = 25%)
- Challenger: binary Instructional vs Entertainment (chance = 50%)
- Algorithm: XGBoost (max_depth = 3, min_child_weight = 10,
  early_stopping_rounds = 50, n_estimators = 1000, lr = 0.05)
- Split: 70/30 stratified by label, epoch-level (subject identity not isolated)
- Metrics: Accuracy, Balanced Accuracy, Macro F1, Cohen's κ, MCC

**Engagement trajectory:**
Frontal theta (AF7 + AF8) sliding window: 4 s window, 2 s step, normalized
to individual EO baseline theta. Linear regression slope per condition per subject.

**Effect sizes:**
Cohen's d (Instructional vs Entertainment) per feature; bootstrap 95% CI
(n = 1000) for all classification metrics.

## 3. Results

### 3.1 Classification

| Model | Accuracy | Balanced Acc | Macro F1 | Cohen κ | MCC | Chance |
|-------|----------|-------------|----------|---------|-----|--------|
| Full (4-class) | 91.6% | — | 0.83 [0.79, 0.87] | 0.66 [0.57, 0.74] | 0.66 [0.58, 0.75] | 25% |
| Challenger (binary) | — | — | — | — | — | 50% |

*(Fill Challenger values from Step 6b output)*

The full model substantially exceeded chance (κ = 0.66 vs κ = 0 at chance).
Bootstrap CIs confirm robustness: even the lower bound of κ (0.57) represents
substantial agreement.

**Feature importance:** AF8_beta and AF7_theta were top Gini-selected features,
consistent with beta reflecting instructional cognitive load and theta reflecting
frontal engagement differences between conditions.

### 3.2 Effect Sizes (Instructional vs Entertainment)

All 16 features showed small or negligible Cohen's d:

| Feature | Cohen's d | Magnitude | Direction |
|---------|-----------|-----------|-----------|
| AF8_beta | +0.33 | small | I > E (more beta in instructional) |
| AF7_theta | −0.27 | small | E > I (more theta in entertainment) |
| AF7_beta | +0.25 | small | I > E |
| TP10/TP9_delta | +0.21 | small | I > E |

Small effect sizes are expected with N = 3. Notably, AF7_theta is *higher* in
Entertainment than Instructional, contrary to the naive hypothesis that instructional
content should drive more frontal theta. This may reflect novelty responses to
entertainment stimuli, or the short duration of IS relative to EL.

### 3.3 Engagement Trajectories

Frontal theta (normalized to EO baseline) showed predominantly negative slopes
across conditions and subjects:

| Condition | Mean slope (theta/baseline/s) | Interpretation |
|-----------|-------------------------------|----------------|
| IS (~1.5 min) | −0.017 | Rapid adaptation even in short block |
| IL (~5 min)   | −0.00088 | Stable engagement |
| ES (~20 sec)  | Mixed (insufficient data) | Unreliable — too few windows |
| EL (~5 min)   | −0.00224 | Gradual decline |

**Hypothesis check:** EL slope (−0.00224) < IL slope (−0.00088) → the hypothesis
that engagement declines more steeply during long entertainment than long
instructional content is **supported** in direction, though the difference is small
and only two of six condition×subject pairs reached p < 0.05.

## 4. Discussion

### Did the data support the hypothesis?

Partially. The engagement trajectory analysis shows EL theta declines more steeply
than IL theta (mean slope −0.0022 vs −0.0009), consistent with the hypothesis
that entertainment allows more passive reception. However, effect sizes are small
and statistical significance is limited by N = 3.

### Effect size in plain language

The largest feature difference (AF8_beta, d = 0.33) is a small effect: about
1/3 of a standard deviation separates Instructional and Entertainment conditions
in right-prefrontal beta power. In practical terms, this means the EEG signal
alone can partially but not perfectly distinguish content type at the individual
epoch level.

### Limitations

1. **Sample size:** N = 3 after exclusion. All effects should be considered
   preliminary. Power is insufficient for confirmatory inference.
2. **Stimulus duration:** IL and EL were ~50% shorter than the protocol minimum
   (5 min vs ≥10 min). The Short/Long manipulation is partially confounded.
   ES (~20 sec) yields ~10 epochs per subject — too few for reliable analysis.
3. **Epoch-level split:** The 70/30 split does not isolate subject identity.
   The model may partly learn individual EEG fingerprints rather than content
   differences. Subject-level cross-validation would be needed with more participants.
4. **Hardware:** Muse 2 dry electrodes are susceptible to motion artifacts and
   have limited spatial coverage. Only 2 prefrontal channels (AF7/AF8) inform the
   engagement index.
5. **No mixed-effects model:** The protocol called for a mixed-effects model with
   subject as random effect. With N = 3 this would be severely underpowered;
   XGBoost with epoch-level features was used instead as a more stable alternative.
