[简体中文](README.md) | **English**

# CCPP — Combined Cycle Power Plant Output Regression

Predict the net output power PE (MW) of a combined cycle power plant from four ambient quantities:

`AT` ambient temperature (°C), `V` exhaust vacuum (cm Hg), `AP` atmospheric pressure (mbar), `RH` relative humidity (%).

## Overview

| Item | Value |
|---|---|
| Task | Regression (output PE, unit MW) |
| Model file | `ccpp_mlp_float32.tflite` (3,444 B, float32) |
| SHA-256 | `4d1436b5940391a56572e390968c4b9dc472c39870873a225e050a57f467f38f` |
| Input | float32 `[1,4]` (AT, V, AP, RH) |
| Output | float32 `[1,1]` (PE, MW) |
| Contract | `ai_spec.yaml`: `timeseries_window` (`window_len: 1`, `channels: 4`, four-feature standardization), `regression_threshold` (451.42 MW) |

The four features are standardized using the mean / standard deviation estimated from the training set (written under `normalize` in `ai_spec.yaml`):
mean `[19.6915, 54.3504, 1013.2044, 73.3364]`, standard deviation
`[7.4319, 12.7244, 5.9579, 14.5091]`. The target was also standardized during training, but after
convergence the target mean/std were **folded into the final layer weights**, so the exported model outputs PE (MW) directly and
no on-device denormalization is needed.

`window_len: 1` expresses "a single four-feature record" as one window with multiple channels; the inputs are laid out as `t * channels + c` (`t` is the sample index within the window, `c` is the channel).

## Structure

```
Input(4) -> Dense(16, ReLU) -> Dense(16, ReLU) -> Dense(1)   # 369 parameters
```

## Metrics

957 held-out test samples, in raw MW units:

| Model | RMSE (MW) | MAE (MW) | R² | max \|err\| (MW) |
|---|---:|---:|---:|---:|
| TFLite float32 | 3.8406 | 2.9859 | 0.95054 | 25.4142 |
| TFLite int8 (the other file in the training project) | 3.9166 | 3.0569 | 0.94857 | 25.2287 |
| Baseline (training-set mean) | 17.2770 | 15.0950 | -0.0008 | 40.1558 |

The data split is a deterministic random split with `seed=42` (7,654 / 957 / 957); CCPP is a per-instant snapshot,
with no official temporal or generating-unit split, so the R² of a random split does not represent cross-time or cross-unit generalization.

## Post-processing Note

The `regression_threshold` in `ai_spec.yaml` labels outputs above 451.42 MW as `high_power` and
those below as `low_power`; this is a binary display for demonstration purposes. The model's own output is still a continuous MW value;
read the float output (`ai_output_to_float`) when the raw value is needed.

## Provenance and License

The maintainer trained and exported the float32 TFLite model with Keras 3 / TensorFlow 2.21; it is a self-owned artifact of this project.
The training data is **UCI Combined Cycle Power Plant** (Tüfekci & Kaya, 2014), released under
**CC BY 4.0**. This repository does not include the dataset.

CC BY 4.0 attribution (retain when redistributing the model):

> Tüfekci, P. & Kaya, H. (2014). *Combined Cycle Power Plant* [Dataset].
> UCI Machine Learning Repository. https://doi.org/10.24432/C5002N

Weight license: see [`LICENSE.md`](LICENSE.md): BSD-3-Clause (Nsing Technologies Inc.); dataset attribution and link must be retained.
