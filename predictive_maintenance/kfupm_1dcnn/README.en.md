[简体中文](README.md) | **English**

# KFUPM 1D-CNN — Rotating Machinery Fault Diagnosis

Four-channel vibration 1D-CNN: `imbalance` / `misalignment` / `normal`.

## Overview

| Item | Value |
|---|---|
| Task | Classification (3 classes: imbalance, misalignment, normal) |
| Model file | `kfupm_1dcnn_int8.tflite` (32,456 B, int8) |
| SHA-256 | `41b645cfc431f2b6ec4ad111d75d9287460cf5ad3347ff28c7fcf6b155046ba1` |
| Input | int8 `[1,4096,4]` (4,096 points × 4 channels, 160 ms at 25.6 kHz) |
| Output | int8 `[1,3]` softmax |
| Contract | `ai_spec.yaml`: `timeseries_window` (4096 × 4, `window_mean: true`, per-channel std), `softmax_argmax` |

Channel order V1 / H1 / V2 / H2 (orthogonal pairs at the two measurement points); the per-channel standard deviation follows the training global normalization mode (600/900 RPM training split).

## Metrics

Official cross-speed test (1,200 RPM unseen during training, 1,178 records): **record-level 100% (1,178/1,178)**. This is the hold-out split of the dataset publisher's protocol, not an independent third-party reproduction.

## Training data

Qasim, Yakoop; Almubarak, Hassan (2026). *Four-Channel Vibration Dataset for
Cross-Speed Rotating-Machinery Fault Diagnosis.* Mendeley Data, V1.
https://doi.org/10.17632/y8mv28hjwb.1

License **CC BY 4.0**: commercial use is permitted; attribution, a link to the license, and an indication of changes are required. The SRO-Net code is MIT; its CITATION.cff asks users of the software to cite the associated paper (not published as of 2026-11). The dataset page only lists SRO-Net as related software; this repository distributes neither the SRO-Net code nor the dataset.

## Provenance

The maintainer trained the 1D-CNN in Keras (conv1d + batch norm + max pooling + global average pooling + dense), then exported it to int8 TFLite. Change note: trained on this dataset and int8-quantized; the model weights are this project's own artifact.

## License

See [`LICENSE.md`](LICENSE.md): weights are BSD-3-Clause (Nsing Technologies Inc.); the attribution obligations of the CC BY 4.0 data must be retained with the product.
