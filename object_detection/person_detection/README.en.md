[简体中文](README.md) | **English**

# Person Detection

A human-presence detection model for TensorFlow Lite Micro: 96×96 grayscale image → `no_person` / `person`.
It is a per-window binary classifier; the bounding-box/sliding-window logic lives on the application side (the reference firmware calls it once per window).

## Overview

| Item | Value |
|---|---|
| Task | Classification (2 classes: `no_person`, `person`) |
| Model file | `person_detect.tflite` (300,568 B, int8) |
| SHA-256 | `808cfdfc0cf3a6fa6f6fa26bfa379ea97c16d5db7334637766e39c3408502e9d` |
| Input | int8 `[1,96,96,1]` grayscale |
| Output | int8 `[1,2]` softmax |
| Contract | `ai_spec.yaml`: `image_grayscale` 96×96, `scale {div: 127.5, sub: 1}`, `softmax_argmax` |

## Provenance

**Byte-for-byte identical** to `tensorflow/lite/micro/models/person_detect.tflite`
in `tensorflow/tflite-micro`; the last upstream change is commit
`aeac6f39e5c7475cea20c54e86d41e3a38312546` (2021-08-12, Apache-2.0).
The header of the officially published `person_detect_model_data.cc` file states
"Copyright 2019 The TensorFlow Authors. All Rights Reserved. Licensed under the
Apache License, Version 2.0." This repository makes no modifications.

## Training data

Visual Wake Words (person / not-person), generated from the COCO 2014 train+val images and annotations.
COCO annotations are licensed under CC BY 4.0, and images follow their original per-image Flickr licenses; the COCO terms themselves impose no non-commercial restriction,
but some of the image licenses are NonCommercial (CC BY-NC series), and whether the weights are bound by the image licenses
is legally unsettled. **This repository does not contain any COCO/VWW images or annotations.**

Dataset references: COCO 2014 (COCO Consortium, CC BY 4.0, https://cocodataset.org);
Visual Wake Words (Chowdhery et al., 2019, arXiv:1906.05721).

## Metrics

The source repository does not record accuracy for this model; without a verifiable number, none is stated here.

## License

See [`LICENSE.md`](LICENSE.md): Apache-2.0.
