# KFUPM 1D-CNN — Rotating Machinery Fault Diagnosis

四通道振动 1D-CNN：`imbalance`（不平衡）/ `misalignment`（不对中）/ `normal`（正常）。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 分类（3 类：imbalance、misalignment、normal） |
| 模型文件 | `kfupm_1dcnn_int8.tflite`（32,456 B，int8） |
| SHA-256 | `41b645cfc431f2b6ec4ad111d75d9287460cf5ad3347ff28c7fcf6b155046ba1` |
| 输入 | int8 `[1,4096,4]`（4,096 点 × 4 通道，25.6 kHz 下 160 ms） |
| 输出 | int8 `[1,3]` softmax |
| 契约 | `ai_spec.yaml`：`timeseries_window`（4096 × 4、`window_mean: true`、逐通道 std）、`softmax_argmax` |

通道顺序 V1 / H1 / V2 / H2（两个测点的正交对）；逐通道标准差取自训练的 global
归一化模式（600/900 RPM 训练划分）。

## 指标

官方跨转速测试（训练时未见的 1,200 RPM，1,178 条记录）：**记录级 100%（1,178/1,178）**。
这是数据集发布方协议的留出划分，不是独立的第三方复现。

## 训练数据

Qasim, Yakoop; Almubarak, Hassan (2026). *Four-Channel Vibration Dataset for
Cross-Speed Rotating-Machinery Fault Diagnosis.* Mendeley Data, V1.
https://doi.org/10.17632/y8mv28hjwb.1

许可 **CC BY 4.0**：允许商用，必须署名、给出许可链接、注明修改。数据集页面另要求引用
其关联的 SRO-Net 论文（截至 2026-11 尚未给出正式论文）。SRO-Net 代码是 MIT，但本仓库
不分发它，也不分发数据集。

## 出处

维护者用 Keras 训练 1D-CNN（conv1d + batch norm + max pooling + global average pooling
+ dense），再导出 int8 TFLite。变更说明：基于该数据集训练并做 int8 量化，模型权重是
本项目自己的产物。

## 许可

见 [`LICENSE.md`](LICENSE.md)：权重 Apache-2.0；数据 CC BY 4.0 的署名义务必须随产品保留。
