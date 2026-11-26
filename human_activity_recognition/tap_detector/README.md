**简体中文** | [English](README.en.md)

# Accelerometer Tap Detection

在桌面静置的 MPU6050 上检测「手指敲击桌面」事件：256 点 × 三轴加速度 → `tap_prob`。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 二分类（`no_tap` / `tap`，sigmoid） |
| 模型文件 | `tap_detector.tflite`（22,240 B，float32） |
| SHA-256 | `497ede0883d8d7e6497e54ae22f0f05efb77d844a8fb07b4dc24192e3c59c591` |
| 输入 | float32 `[1,1,256,3]`（256 个采样点 × x/y/z） |
| 输出 | float32 `[1,1]`（sigmoid 概率） |
| 契约 | `ai_spec.yaml`：`timeseries_window`（256 点 × 3 轴、`window_mean: true`）、`binary_threshold` 0.5 |

## 出处

维护者自训：PyTorch 训练 → 导出 ONNX → `onnx2tf` 转成 TFLite。训练工程用两段自己
采集的 MPU6050 CSV（连续手指敲击 / 桌面静置）构造了 40 个窗口，记录了原始 CSV 的
SHA-256、标签规则和「同一张桌子、同一设备、同一次采集」的限制。没有第三方权重或数据。

## 指标与已知限制

训练工程的报告给出：分块 5 折交叉验证的样本外 AUC 均值 1.0000，判决阈值 0.5315
（模型卡里的 `ai_spec.yaml` 用 0.5，二者都可用，按应用调整）。**诚实的结论**：在干净
数据与加性噪声下，这个 CNN 并没有战胜「峰值阈值」基线；它只在幅度与敲击相当、仅波形
不同的合成干扰下明显领先。完整证据在训练工程的 `METRICS_REPORT.md`。

## 许可

见 [`LICENSE.md`](LICENSE.md)：维护者自有权重，按 BSD-3-Clause 发布；数据由维护者自行
采集、训练自行完成，不涉及任何第三方数据或权重。`commercial_use: allowed`。
