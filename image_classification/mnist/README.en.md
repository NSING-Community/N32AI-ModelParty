[简体中文](README.md) | **English**

# MNIST Digit Recognition

A 10-class classification model for handwritten digits 0–9, suitable as an introductory example.

## Overview

| Item | Value |
|---|---|
| Task | Classification (10 classes: `0` … `9`) |
| Model file | `mnist.tflite` (110,916 B, float32) |
| SHA-256 | `1ba966ac590b50b2c695bab1cb95b6b22a78f386d07eeee2e9aa6d16a5c7d64e` |
| Input | float32 `[1,28,28,1]` grayscale |
| Output | float32 `[1,10]` softmax |
| Contract | `ai_spec.yaml`: `image_grayscale` 28×28, `scale.div = 255`, `softmax_argmax` |

## Provenance

Trained in-house by the maintainers on the official MNIST dataset: TensorFlow/Keras `mnist` loader → training → float32 TFLite. The weights are a self-owned product of Nsing Technologies Inc.; this repository does not include MNIST data, training scripts, or model artifacts.

The training data is MNIST (Yann LeCun, Corinna Cortes, Christopher J.C. Burges). The official MNIST page states no license or terms of use; TensorFlow/Keras documentation states it is provided under CC BY-SA 3.0, a claim traceable to a 2018 TensorFlow commit with no source cited. Whether the weights constitute an adaptation of MNIST, and whether the ShareAlike term of CC BY-SA 3.0 applies to the weights, is legally unsettled; consult legal counsel before commercial use.

## Metrics

This repository does not record the accuracy of this model; without a verifiable number, none is stated here.

## License

See [`LICENSE.md`](LICENSE.md): weights are BSD-3-Clause (Nsing Technologies Inc.); the license for the MNIST training data is undetermined (no statement on the official page; Keras documentation says CC BY-SA 3.0). `commercial_use: review`; consult legal counsel before commercial use.
