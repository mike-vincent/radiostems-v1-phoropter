# MOTHBALLED POST-MORTEM — RadioStems (`v1.0` – `v3.0`) Mono OTR Stem Separation & Vocoder Remake

- **Status:** **MOTHBALLED (Approach Rejected by Human Ear Test — Do Not Resume This Architecture)**
- **Date:** 2026-11-04
- **Active GPUs:** `0` (`$0.00/hr`, all 8 regional AML clusters deleted)

---

## 1. Verdict

Across four iterations (`v1.0` toy conv mask, `v1.0-HDemucs` + Wiener gate, `v1.1` 2048-point narrowband organ suppressor, and `v2.0`/`v3.0` `VoiceFixer` generative neural vocoder remakes), **human ear testing confirmed that this cascaded subtractive-separation + neural-vocoder approach degrades the audio and does not converge to usable archival quality.**

---

## 2. Root Causes Why This Approach Failed (Do Not Repeat)

1. **`MUSDB` Singing-Voice Separators (`HDemucs`) Fail on Mono OTR Instrumentation:**
   - Pre-trained music source separators (`HDEMUCS_HIGH_MUSDB_PLUS`) are trained on modern stereo pop stems (`drums`, `bass`, `other`, `vocals`).
   - In 1930s–1950s mono radio drama (*Suspense*, *X Minus One*, *The Shadow*, *Lights Out*), background cues consist of **mono brass horns, woodwinds, theremin, and Hammond organ** occupying the exact `250–2,500 Hz` fundamental and formant range as human speech.
   - `HDemucs` either passes those mid-band instruments straight into the vocal stem (`33.3%` music bleed in blind ear tests) or carves holes out of the actor's voice when trying to suppress them.
2. **Subtractive Spectral Masking Destroys Narrowband Mono Speech (`1.9–5.0 kHz`):**
   - Adding spectral Wiener filters, sliding percentile notches (`kthvalue`), or pause gates on top of `HDemucs` strips the remaining vocal harmonics and introduces severe STFT phase warble ("hollow / underwater / chewed-up" speech).
   - **Critical Metric Lesson:** Mathematical SNR / SI-SDR / pause-ratio metrics (`+8 dB` to `+16 dB` on paper) were completely anti-correlated with human perceptual quality—rewarding aggressive gating and bandwidth loss that sounded worse to the human ear.
3. **Generative Vocoder Resynthesis (`VoiceFixer` / `TFGAN`) Compounds Separation Damage:**
   - Feeding a subtractive-damaged vocal stem into a pre-trained Mel-inpainter + `TFGAN` vocoder (`v2.0` and `v3.0`) cannot recover vocal articulation that was already carved out in Stage 1; instead, it hallucinates synthetic vocoder buzz, lispy sibilance, and unnatural timbre shifts.
   - Attempting to post-process the vocoder output with Mel-peak sharpening and hybrid core blending (`v3.0`) made the artifacts worse rather than better.
