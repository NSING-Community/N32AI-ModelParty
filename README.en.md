[简体中文](README.md) | **English**

# N32AI Model Party

`n32ai-modelparty` is a TFLite model library for Nsing Technologies Inc. MCUs, organized by use case. Each model folder contains the model file, `ai_spec.yaml` (pre/post-processing contract), `README.md` (model card), and `LICENSE.md` (weight license and attribution, including the full license text).

## Models

| Category | Model | Task | Input → Output | Training data | License |
|---|---|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | Handwritten digits, 10 classes | float32 `[1,28,28,1]` → `[1,10]` | MNIST | BSD-3-Clause (self-owned weights) |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k, 1000 classes | int8 `[1,128,128,3]` → `[1,1000]` | ImageNet | Apache-2.0 (weights) |
| `object_detection` | [person_detection](object_detection/person_detection/) | Person / no-person | int8 `[1,96,96,1]` → `[1,2]` | Visual Wake Words (COCO 2014) | Apache-2.0 (weights) |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12-class keyword spotting (MFCC) | float32 `[1,490]` → `[1,12]` | Google Speech Commands | Apache-2.0 (weights) + CC BY 4.0 (data) |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | Tap / no-tap | float32 `[1,1,256,3]` → `[1,1]` | Self-collected MPU6050 data | BSD-3-Clause (self-owned), commercial `allowed` |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | Imbalance / misalignment / normal | int8 `[1,4096,4]` → `[1,3]` | KFUPM four-channel vibration dataset | BSD-3-Clause (weights) + CC BY 4.0 (data) |
| `regression` | [ccpp_mlp](regression/ccpp_mlp/) | Net power output PE (MW) | float32 `[1,4]` → `[1,1]` | UCI CCPP | BSD-3-Clause (weights) + CC BY 4.0 (data) |

## Directory layout

```
<category>/
└── <model>/
    ├── README.md                  Model card: use, I/O, dataset, metrics, provenance, license
    ├── LICENSE.md                 Weight license and attribution, including the full text
    │                              (third-party: Apache-2.0; self-owned: BSD-3-Clause;
    │                               CC BY 4.0 text is included when the dataset is CC BY 4.0)
    ├── <model>.tflite             Model file
    └── ai_spec.yaml               Pre/post-processing contract (next to the model file)
```

## Licensing and provenance

- Root `LICENSE`: BSD 3-Clause for the repository's own content (README, `models.yaml`,
  `ai_spec.yaml`, and the self-trained `mnist` / `tap_detector` / `kfupm_1dcnn` /
  `ccpp_mlp` weights, etc.).
- Root `NOTICE`: Nsing Technologies Inc. copyright/license scope, third-party models and
  datasets, and trademark notices.
- Per-model `LICENSE.md`: the full license text for the model weights (third-party models
  keep their upstream Apache-2.0; self-owned models are BSD-3-Clause), plus dataset
  license texts (e.g. CC BY 4.0).
- `models.yaml`: machine-readable `license` / `copyright` / `dataset_license` /
  `commercial_use` / `sha256` / `provenance`.
- Third-party models keep their own licenses; they are **not** covered by the root
  license.
- **This repository does not contain any training datasets**; it distributes trained
  weights only. Dataset license obligations (attribution, citation, redistribution
  conditions) are stated in each model card and model `LICENSE.md`; CC BY 4.0 permits
  redistribution with attribution, a license link, and an indication of changes.
- **Model-file redistribution constraint**: every `.tflite` must be redistributed
  together with the `LICENSE.md` in the same folder (full license text and
  attributions). The `.tflite` files carry no embedded license metadata; extracting a
  `.tflite` alone does not remove the obligations in its `LICENSE.md`. For Apache-2.0
  models, observe Section 4 in particular (give recipients a copy of the license, state
  changes prominently, and retain all copyright and attribution notices).

### Demo / review models

The following models are provided as demos/research models only; they are not part of
the commercial release bundle. See each model's `LICENSE.md` for the full license text
and attribution.

- `mnist`
- `mobilenetv1_a025_128_int8`
- `person_detection`

Commercial status (`allowed` / `review` / `prohibited`) is governed by the root
`NOTICE`, each model's `LICENSE.md`, and `commercial_use` in `models.yaml`. These are
engineering records, not legal advice.
