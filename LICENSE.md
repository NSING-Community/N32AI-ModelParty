# 许可说明

本仓库同时包含自有内容和第三方模型，**根文件不是一个覆盖全仓库的单一许可证**。
每个模型文件夹的 `LICENSE` 是该模型权重的权威说明；本文件只说明仓库层面的分工。

## 本仓库自有内容

以下内容由本仓库维护者创作，按 **Apache License, Version 2.0** 发布：

- 根 `README.md`、各分类与模型文件夹里的 `README.md`、`models.yaml`；
- 各模型文件夹里的 `ai_spec.yaml`（预处理 / 后处理契约）；
- `human_activity_recognition/tap_detector/tap_detector.tflite`（维护者自训）；
- `predictive_maintenance/kfupm_1dcnn/kfupm_1dcnn_int8.tflite`（维护者自训的权重；
  训练数据 KFUPM 数据集是 CC BY 4.0，署名要求见该模型卡）；
- `regression/ccpp_mlp/ccpp_mlp_float32.tflite`（维护者自训；训练数据 UCI CCPP
  是 CC BY 4.0，署名要求见该模型卡）。

Apache-2.0 全文见 [`licenses/Apache-2.0.txt`](licenses/Apache-2.0.txt)。
简单说：可以商用、修改、再分发，需保留版权与许可声明、标注修改，不提供担保，
不授予商标权。

## 第三方模型

下列模型是从上游取得的预训练权重，**不适用上面的 Apache-2.0 发布**，各自保留上游
许可与归属：

| 模型 | 权重许可 | 说明 |
|---|---|---|
| `image_classification/mnist` | **未确认（NOASSERTION）** | 上游示例未记录；商业使用前必须补记录或替换 |
| `image_classification/mobilenetv1_a025_128_int8` | Apache-2.0（Keras Applications） | 训练数据 ImageNet 的条款只允许非商业研究，残留风险 |
| `object_detection/person_detection` | Apache-2.0（TensorFlow Lite Micro） | 与上游模型逐字节一致 |
| `audio_event_detection/ds_cnn_s` | Apache-2.0（Arm ML-KWS-for-MCU） | 本地做的 TFLite 转换属于修改，需标注 |

各文件夹的 `LICENSE` 给出确切的版权行、上游地址和修改说明；模型卡里另写训练数据集
及其许可（Speech Commands、COCO、KFUPM 数据集的 CC BY 4.0 署名要求等）。
`licenses/CC-BY-4.0.txt` 是 CC BY 4.0 全文，随附用于满足数据集的署名条件。

## 数据集

本仓库**不包含任何训练数据集**。数据集只用于训练，其许可不因权重在此分发而改变；
再分发数据集本身需要单独的许可评估（MNIST / ImageNet 尤其如此）。

## 免责声明

本文件是工程事实记录，不是法律意见。商用前请按各模型卡的「出处」与「许可」小节
核对一手来源，并让公司法务确认有争议的两项（mnist 的上游、ImageNet 训练权重）。
