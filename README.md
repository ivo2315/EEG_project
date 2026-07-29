# Case: EEG analysis

# Research Questions:
1. **Brain activity while using AI** — does coding with an AI assistant produce a different EEG signature than coding alone, or watching a sports broadcast?
2. **Person identification** — can the same activity, recorded across people, be used to identify the individual? Optionally cross-reference Myers–Briggs type.
3. **Activity classification** — distinguish video gaming, online shopping, financial-market analysis, and an intelligence test from EEG alone.
4. **Sex differences in resting-state EEG** — at rest or during low-activity tasks, are there reliable male/female differences?
5. **Attention span** — short vs. long videos, instructional vs. entertainment.

# Hardware
The device used during the research is: Muse 2 Headband (Neuronal Tracking).

Muse-2889, Model: MU-06

**Specifications**:
| Spec              | Value                                |
|-------------------|--------------------------------------|
| EEG channels      | 4 (TP9, AF7, AF8, TP10)              |
| Brain Waves       | Delta, Theta, Alpha, Beta, Gamma     |
| Sample rate       | 256 Hz                               |
| Other sensors     | PPG (3 ch @ 64 Hz), Accel, Gyro      |
| Connection        | BLE                                  |
| Reference         | Fpz                                  |

**The Frequency Band**
| Band Frequency Brain State |
|----------------------------|
|Delta(δ) 0.5 - 4 Hz Dreamless sleep (deep meditation)|
|Theta(θ) 4 - 8 Hz Deeply relaxed, inward focused (drowsiness, vivid dreams)|
|Alpha(α) 7.5 - 13 Hz Very relaxed, passive attention (thoughtful times, reflective, restful)|
|Beta(β) 13 - 30 Hz Anxiety dominant, active, external attention, relaxed (busy mind)|
|Gamma(γ) 30 - 44 Hz Concentration (higher levels of consciousness, problem solving)|

# Software
Blue Muse version: 2.4.0.0

LabRecorder version: 1.17.0

Custom script included in the repo for pushing markers during the data collection.

# Data
The raw data is provided in the data/raw folder.
