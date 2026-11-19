[简体中文](README.md) | **English**

# DS-CNN-S Keyword Spotting

Arm's DS-CNN-S keyword spotting model (12 classes); the input is 49 frames × 10-dimensional MFCC.

## Overview

| Item | Value |
|---|---|
| Task | Classification (12 classes: `silence`, `unknown`, `yes`, `no`, `up`, `down`, `left`, `right`, `on`, `off`, `stop`, `go`) |
| Model file | `DS_CNN_S.tflite` (95,480 B, float32) |
| SHA-256 | `e4e6af6a3fc7db4f7fdea3430da82f979ad9c342082d6afab13c6810ea85c907` |
| Input | float32 `[1,490]` (49 frames × 10-dimensional MFCC) |
| Output | float32 `[1,12]` softmax |
| Contract | `ai_spec.yaml`: `audio_mfcc` (16 kHz, 30 ms / 20 ms, 40 mel bands, 10 coefficients), `softmax_argmax` |

## Provenance

The weights come from `Pretrained_models/DS_CNN/DS_CNN_S.pb` in `ARM-software/ML-KWS-for-MCU` (TensorFlow frozen graph, Apache-2.0). The upstream repository has no `.tflite`; this file is a float32 TFLite converted by this project with TOCO. **The conversion steps and time were not recorded**, but the file is byte-for-byte identical to the copy in the N32 KWS reference project. Modification notes: TensorFlow graph → TFLite (TOCO format conversion); during conversion the BN parameters were folded into the preceding convolution weights/biases (deterministic folding, no retraining/quantization).

## Training Data

Google Speech Commands v0.01 / v0.02, licensed under **CC BY 4.0** (both Google's 2017 release announcement and Warden 2018, arXiv:1804.03209, Section 2 state this). Commercial use is permitted, provided attribution and a link to the license; there is no ShareAlike. **This repository does not contain the Speech Commands data.**

## Metrics

The source repository does not record the accuracy of this model; without a verifiable number, none is given here.

## License

See [`LICENSE.md`](LICENSE.md): the weights are Apache-2.0, the data is CC BY 4.0, and the attribution requirements are set out in that file.
