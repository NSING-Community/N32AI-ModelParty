[简体中文](README.md) | **English**

# N32AI Model Party

`n32ai-modelparty` is a TFLite model library for Nsing Technologies Inc. MCUs, organized by use case. Each model folder contains the model file, `ai_spec.yaml` (pre/post-processing contract), `README.md` (model card), and `LICENSE.md` (weight license and attribution; Apache-2.0 / CC BY 4.0 full texts under `licenses/`).

## Models

| Category | Model | Task | Input → Output |
|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | Handwritten digits, 10 classes | float32 `[1,28,28,1]` → `[1,10]` |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k, 1000 classes | int8 `[1,128,128,3]` → `[1,1000]` |
| `object_detection` | [person_detection](object_detection/person_detection/) | Person / no-person | int8 `[1,96,96,1]` → `[1,2]` |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12-class keyword spotting (MFCC) | float32 `[1,490]` → `[1,12]` |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | Tap / no-tap | float32 `[1,1,256,3]` → `[1,1]` |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | Imbalance / misalignment / normal | int8 `[1,4096,4]` → `[1,3]` |
| `regression` | [ccpp_mlp](regression/ccpp_mlp/) | Net power output PE (MW) | float32 `[1,4]` → `[1,1]` |
| `arc_fault_detection` | [arc_cnn_int8](arc_fault_detection/arc_cnn_int8/) | Arc / normal | int8 `[1,500,8]` → `[1,2]` |

## Directory layout

```
<category>/
└── <model>/
    ├── README.md                  Model card: use, I/O, dataset, metrics, provenance, license
    ├── LICENSE.md                 Weight license and attribution; Apache-2.0 / CC BY 4.0
    │                              full texts under licenses/
    ├── <model>.tflite             Model file
    └── ai_spec.yaml               Pre/post-processing contract (next to the model file)

licenses/
├── Apache-2.0.txt                Apache-2.0 full text
└── CC-BY-4.0.txt                 CC BY 4.0 full text
```

## Licensing and provenance

- Root `LICENSE`: BSD 3-Clause for the repository's own content (README, `models.yaml`,
  `ai_spec.yaml`, and the self-trained `mnist` / `tap_detector` / `kfupm_1dcnn` /
  `ccpp_mlp` / `arc_cnn_int8` weights, etc.).
- Root `NOTICE`: Nsing Technologies Inc. copyright/license scope, third-party models and
  datasets, and trademark notices.
- Per-model `LICENSE.md`: weight license declaration, modification notes, and dataset
  attribution; the Apache-2.0 / CC BY 4.0 full texts are in `licenses/`.
- `licenses/`: full Apache-2.0 and CC BY 4.0 texts referenced by the model licenses;
  they must be redistributed with the model.
- `models.yaml`: machine-readable `license` / `copyright` / `dataset_license` /
  `commercial_use` / `sha256` / `provenance`.
- Third-party models keep their own licenses; they are **not** covered by the root
  license.
- **This repository does not contain any training datasets**; it distributes trained
  weights only. Dataset license obligations (attribution, citation, redistribution
  conditions) are stated in each model card, model `LICENSE.md`, and the root `NOTICE`;
  CC BY 4.0 permits redistribution with attribution, a license link, and an indication
  of changes.
- **Model-file redistribution constraint**: every `.tflite` must be redistributed
  together with the `LICENSE.md` in the same folder (license declaration and
  attributions) and the `licenses/` texts it references. The `.tflite` files carry no
  embedded license metadata; extracting a `.tflite` alone does not remove the
  obligations in its `LICENSE.md` or `licenses/`. For Apache-2.0 models, observe
  Section 4 in particular (give recipients a copy of the license, state changes
  prominently, and retain all copyright and attribution notices).

### Demo / review models

The following models are provided as demos/research models only; they are not part of
the commercial release bundle. See each model's `LICENSE.md` and `licenses/` for the
full license text and attribution.

- `mnist`
- `mobilenetv1_a025_128_int8`
- `person_detection`

Commercial status (`allowed` / `review` / `prohibited`) is governed by the root
`NOTICE`, each model's `LICENSE.md`, and `commercial_use` in `models.yaml`. These are
engineering records, not legal advice.
