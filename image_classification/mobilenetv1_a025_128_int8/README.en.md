[简体中文](README.md) | **English**

# MobileNetV1 a0.25 128 — ImageNet-1k

Keras Applications' MobileNetV1 (α=0.25, 128×128) on ImageNet-1k for 1000-class
classification, with full integer int8 quantization.

## Overview

| Item | Value |
|---|---|
| Task | Classification (1000 classes, Keras index order) |
| Model file | `mobilenetv1_a025_128_int8.tflite` (597,352 B, int8 input/output) |
| SHA-256 | `ec9cbb11b84f4f6d205d2faba9ffd134e122d0ca9b92f8d05b8bf53a2602f149` |
| Input | int8 `[1,128,128,3]` (scale 2/255, zero_point −1) |
| Output | int8 `[1,1000]` (scale 1/256, zero_point −128, the graph ends with SOFTMAX) |
| Contract | `ai_spec.yaml`: `image_rgb` 128×128, `scale {div: 127.5, sub: 1}`, `softmax_argmax` + 1000 class names |

## Metrics

On the imagenette2-160 validation set (3,925 images, a ten-class subset of ImageNet):

| Metric | int8 | float32 baseline |
|---|---|---|
| Top-1 (all 1000 classes) | 43.95% | 46.19% |
| Top-5 (all 1000 classes) | 67.24% | 69.43% |
| Top-1 restricted to the ten classes | 87.46% | 89.61% |

The Top-1 agreement rate between int8 and float32 is 77.35%. Calibration used only
500 training images from imagenette2-160; that data is not distributed with this
repository.

## Provenance

- Weights: `keras.applications.MobileNet(alpha=0.25, input_shape=(128,128,3),
  weights="imagenet")`; Keras 3 is released under Apache-2.0 (the legacy
  `keras-applications` package is MIT; the weights were originally translated from a
  TF-Slim checkpoint, Apache-2.0).
- Modification: this project converted them to int8 TFLite (full integer
  quantization); this must be stated upon redistribution.
- Training data: ImageNet / ILSVRC. Its access terms permit non-commercial research
  and education only ("Researcher shall use the Database only for non-commercial
  research and educational purposes."). Whether the weights are subject to the
  dataset terms is legally unsettled and remains a residual risk.

## License

See [`LICENSE.md`](LICENSE.md): weights are Apache-2.0; consult legal to confirm the
commercial-use interpretation of the ImageNet terms.
