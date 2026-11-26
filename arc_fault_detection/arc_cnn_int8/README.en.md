[简体中文](README.md) | **English**

# arc_cnn_int8 Model

## Application: Arc Fault Detection (AFD)

## Model Description

arc_cnn_int8 is a binary arc-fault detection model for photovoltaic DC systems, classifying
`normal` / `arc`. It takes three 4 kHz signals (`i_arc` / `i_dc` / `v_dut`), runs them through
an AGC feature front end to produce a 500-point × 8-channel causal feature window, and uses a
compact two-branch 1D CNN to output two logits:

- Main branch: three Conv1D layers (16 / 32 / 48 channels, kernel 8 / 8 / 4, stride 4 / 4 / 2);
- High-frequency branch: two Conv1D layers (8 / 8 channels, kernel 16 / 8, stride 8 / 8);
- Each branch computes GAP and GMP, concatenates them, passes through a Dense head, and the
  two 2-class logits are added.

This folder provides the fully-integer int8 TFLite model
[`arc_cnn_int8.tflite`](arc_cnn_int8.tflite) (30,288 B). Deployment uses this int8 model. The
preprocessing and postprocessing contract is in [`ai_spec.yaml`](ai_spec.yaml).

This is a reference / DEMO model, not an AFCI product certified to UL 1699B / IEC 63027; field
deployment requires independent validation and certification.

## Network Information

| Network information | Value |
|---|---|
| Framework | TensorFlow Lite (TFLM) |
| Model file | `arc_cnn_int8.tflite` (30,288 B) |
| Parameters | 14,458 |
| MACs | 332,260 / window |
| Quantization | int8 (full-integer PTQ) |
| Input | int8 `[1, 500, 8]` |
| Output | int8 `[1, 2]` logits |
| SHA-256 | `857ee77aa742769d64471327e75919b82c9fde6a808101808b3475cf3011fa14` |

## Network Input/Output

| Input shape | Description |
|---|---|
| `(1, 500, 8)` | 500 points × 8 AGC feature channels; 4 kHz, 125 ms window, 12.5 ms hop; int8, scale 0.050822854, zero_point 5 |

| Output shape | Description |
|---|---|
| `(1, 2)` | `[normal, arc]` logits; int8, scale 0.14278354, zero_point 0; after softmax, class 1 = `p_arc` |

## Preprocessing

The model does not consume the three raw waveforms directly; it uses a causal feature front end
at 4 kHz / 125 ms / 12.5 ms hop:

- Source channels: `i_arc`, `i_dc`, `v_dut`;
- Model channels (8): `arc_shape`, `arc_pu`, `dev_i_agc`, `dev_v_abs`, `trend_i`, `hf_agc`,
  `lvl_i_ctx`, `lvl_v_ctx`;
- 50-point hop (12.5 ms at 4 kHz); the 32 kHz high-frequency magnitude feeds `hf_agc`;
- AGC parameters, the eight gains, clip ±6, and the EMA median estimate are in `ai_spec.yaml`.

## Postprocessing

- The output is two logits (`normal`, `arc`); dequantize, apply softmax, and take class 1 as
  `p_arc`;
- Per-window threshold `0.66`;
- De-bounce rule k-of-m 3/5: at least 3 of the last 5 windows must exceed the threshold; 5 hops
  correspond to 62.5 ms of decision history;
- `ai_spec.yaml` uses the `softmax_threshold` postprocess kind; N32EmbedAI must implement it.

## License

See [`LICENSE.md`](LICENSE.md): the weights are BSD-3-Clause (Nsing Technologies Inc.); dataset
notice in the repository root [`NOTICE`](../../NOTICE). `commercial_use: allowed`, but this is a
reference / DEMO model, not an AFCI product certified
to UL 1699B / IEC 63027; field deployment requires independent validation and certification.
