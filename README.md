
## Overview
This project explores the synthesis of **individual musical notes** using **Generative Adversarial Networks (GANs)**.  
The goal is to generate realistic audio conditioned on characteristics such as **pitch, timbre, and loudness**, leveraging the **[NSynth dataset](https://magenta.tensorflow.org/datasets/nsynth)** as the foundation for training and evaluation.

Our approach is inspired by **[GANsynth](https://openreview.net/forum?id=H1xQVn09FX)**, a technique for neural audio synthesis that achieves high-fidelity and fast parallel generation.

---

## Background

### NSynth
- NSynth (Neural Audio Synthesis) is a large-scale dataset of **300,000+ musical notes** played on thousands of instruments, annotated with features like pitch, velocity, and instrument family [[Engel et al., 2017]](https://arxiv.org/abs/1704.01279).
- It was originally introduced with an **autoencoder + WaveNet decoder** approach, enabling interpolation across timbres, but limited by slow autoregressive sampling.

### GANsynth
- GANsynth replaces autoregressive generation with a **progressive GAN architecture**, producing audio **spectrograms** that can be inverted to waveforms [[Engel et al., 2019]](https://arxiv.org/abs/1902.08710).
- Key innovations:
  - Generate **log-magnitude spectrograms** + **instantaneous frequency (IF)** instead of raw waveforms.
  - Condition the GAN on **pitch labels**, letting the latent vector capture timbre.
  - Use an **auxiliary pitch classification loss** in the discriminator.
  - Achieves **orders of magnitude faster generation** compared to WaveNet, with perceptually high audio quality.

---

## Method

1. **Data Preparation**
   - Extract spectrograms from NSynth waveforms.
   - Represent audio using **log-magnitude + IF**, normalized for training.
   - Condition data on pitch and instrument labels.

2. **Model Architecture**
   - **Generator**: latent vector `z` + pitch conditioning → upsampled spectrogram.
   - **Discriminator**: real/fake classification + auxiliary pitch prediction.
   - Training uses adversarial loss + auxiliary classification loss.

3. **Training**
   - Progressive GAN training for stability.
   - Evaluate with pitch classification accuracy, diversity metrics, and human listening tests.

4. **Audio Reconstruction**
   - Convert generated spectrograms back to waveforms with inverse STFT and phase reconstruction (e.g., Griffin-Lim).

---
