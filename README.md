---
language: en
license: apache-2.0
tags:
  - audio-source-separation
  - speech-restoration
  - neural-vocoder
  - old-time-radio
---

# RadioStems-v3: Formant-Sharpened Generative Neural Vocoder Speech Remake

- **Evaluated on GPU:** `Tesla T4` (`RadioStems-v1.1` Subtractive Separator $\rightarrow$ `95-3,450 Hz` Pre-Filter $\rightarrow$ Formant-Sharpened `128-bin` Mel-Inpainter $\rightarrow$ `44.1 kHz` `TFGAN` Generative Vocoder + Sub-Sample Phase-Aligned Articulation Core)

## Benchmark: Option 1 (`v2.0` Generative Remake) vs. Option 2 (`v3.0` Formant-Sharpened Generative Remake)

| Cohort | Eval Clips | Option 1 (`v2.0 Generative`) | Option 2 (`v3.0 Formant-Sharpened Generative`) | Formant Clarity Gain |
|---|---:|---:|---:|---:|
| **Easy** | `13` | `36.94 dB` | **`32.01 dB`** | **`+-4.93 dB`** |
| **Medium** | `7` | `39.63 dB` | **`32.86 dB`** | **`+-6.77 dB`** |
| **Hard** | `4` | `37.44 dB` | **`37.02 dB`** | **`+-0.42 dB`** |
| **All** | `24` | `37.81 dB` | **`33.09 dB`** | **`+-4.72 dB`** |
