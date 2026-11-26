[简体中文](README.md) | **English**

# Accelerometer Tap Detection

Detect "finger tapping the desk" events with an MPU6050 resting on a desk: 256 points × three-axis acceleration → `tap_prob`.

## Overview

| Item | Value |
|---|---|
| Task | Binary classification (`no_tap` / `tap`, sigmoid) |
| Model file | `tap_detector.tflite` (22,240 B, float32) |
| SHA-256 | `497ede0883d8d7e6497e54ae22f0f05efb77d844a8fb07b4dc24192e3c59c591` |
| Input | float32 `[1,1,256,3]` (256 sample points × x/y/z) |
| Output | float32 `[1,1]` (sigmoid probability) |
| Contract | `ai_spec.yaml`: `timeseries_window` (256 points × 3 axes, `window_mean: true`), `binary_threshold` 0.5 |

## Provenance

Maintainer-trained: PyTorch training → ONNX export → `onnx2tf` conversion to TFLite. The training project used two segments of self-collected MPU6050 CSV (continuous finger tapping / desk at rest) to construct 40 windows, and recorded the SHA-256 of the raw CSVs, the labeling rules, and the constraint of "the same desk, the same device, the same collection session". No third-party weights or data.

## Metrics and Known Limitations

The training project's report gives: out-of-sample AUC mean 1.0000 under blocked 5-fold cross-validation, decision threshold 0.5315 (the model card's `ai_spec.yaml` uses 0.5; either works, adjust to the application). **Honest conclusion**: on clean data and under additive noise, this CNN does not outperform the "peak threshold" baseline; it leads clearly only under synthetic interference with amplitude comparable to a tap and differing only in waveform. Full evidence is in the training project's `METRICS_REPORT.md`.

## License

See [`LICENSE.md`](LICENSE.md): maintainer self-owned weights, released under BSD-3-Clause; the data was collected by the maintainer and training was completed in-house, with no third-party data or weights involved. `commercial_use: allowed`.
