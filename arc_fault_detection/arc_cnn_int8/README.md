**简体中文** | [English](README.en.md)

# arc_cnn_int8 模型

## 应用场景：电弧故障检测（AFD）

## 模型描述

arc_cnn_int8 是一个面向光伏直流系统的电弧故障检测二分类模型，输出 `normal` / `arc`。模型使用
4 kHz 的三路信号（`i_arc` / `i_dc` / `v_dut`），经 AGC 特征前端生成 500 点 × 8 通道的因果特征窗，
再由一个轻量双分支 1D CNN 输出两类 logits：

- 主分支：3 层 Conv1D（16 / 32 / 48 通道，kernel 8 / 8 / 4，stride 4 / 4 / 2）；
- 高频分支：2 层 Conv1D（8 / 8 通道，kernel 16 / 8，stride 8 / 8）；
- 两分支分别做 GAP / GMP 拼接，经 Dense 头后输出 2 类 logits 并相加。

本目录提供全整数 int8 TFLite 模型 [`arc_cnn_int8.tflite`](arc_cnn_int8.tflite)（30,288 B）。
部署使用该 int8 模型。预处理与后处理契约见 [`ai_spec.yaml`](ai_spec.yaml)。

本模型是参考 / DEMO，不是经过 UL 1699B / IEC 63027 认证的 AFCI 产品；现场部署需要独立验证和认证。

## 网络信息

| 网络信息 | 值 |
|---|---|
| 框架 | TensorFlow Lite（TFLM） |
| 模型文件 | `arc_cnn_int8.tflite`（30,288 B） |
| 参数量 | 14,458 |
| MAC | 332,260 / 窗 |
| 量化 | int8（全整数 PTQ） |
| 输入 | int8 `[1, 500, 8]` |
| 输出 | int8 `[1, 2]` logits |
| SHA-256 | `857ee77aa742769d64471327e75919b82c9fde6a808101808b3475cf3011fa14` |

## 网络输入/输出

| 输入形状 | 说明 |
|---|---|
| `(1, 500, 8)` | 500 点 × 8 通道 AGC 特征；4 kHz、125 ms 窗、12.5 ms hop；int8，scale 0.050822854，zero_point 5 |

| 输出形状 | 说明 |
|---|---|
| `(1, 2)` | `[normal, arc]` logits；int8，scale 0.14278354，zero_point 0；softmax 后取 class 1 = `p_arc` |

## 预处理

模型不直接使用三路原始波形，而是 4 kHz / 125 ms / 12.5 ms hop 的因果特征前端：

- 源通道：`i_arc`、`i_dc`、`v_dut`；
- 模型通道（8 路）：`arc_shape`、`arc_pu`、`dev_i_agc`、`dev_v_abs`、`trend_i`、`hf_agc`、
  `lvl_i_ctx`、`lvl_v_ctx`；
- 50 点 hop（12.5 ms @ 4 kHz）；32 kHz 高频幅值作为 `hf_agc` 的输入；
- AGC 参数、8 路 gain、clip ±6 和 EMA 中值估计都写在 `ai_spec.yaml`。

## 后处理

- 输出是 2 类 logits（`normal`、`arc`），先反量化，再 softmax，取 class 1 作为 `p_arc`；
- 单窗阈值 `0.66`；
- 去抖规则 k-of-m 3/5：最近 5 窗至少 3 窗超过阈值才置位；5 个 hop 对应 62.5 ms 的判决历史；

## 许可

见 [`LICENSE.md`](LICENSE.md)：权重 BSD-3-Clause（Nsing Technologies Inc.）；数据集声明见仓库根
[`NOTICE`](../../NOTICE)。`commercial_use: allowed`，
但本模型是参考 / DEMO，不是经过 UL 1699B / IEC 63027 认证的 AFCI 产品；现场部署需自行验证和认证。
