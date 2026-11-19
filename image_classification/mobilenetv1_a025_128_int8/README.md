**简体中文** | [English](README.en.md)

# MobileNetV1 a0.25 128 — ImageNet-1k

Keras Applications 的 MobileNetV1（α=0.25，128×128）在 ImageNet-1k 上的 1000 类分类
模型，已做 int8 全整型量化。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 分类（1000 类，Keras 索引顺序） |
| 模型文件 | `mobilenetv1_a025_128_int8.tflite`（597,352 B，int8 输入/输出） |
| SHA-256 | `ec9cbb11b84f4f6d205d2faba9ffd134e122d0ca9b92f8d05b8bf53a2602f149` |
| 输入 | int8 `[1,128,128,3]`（scale 2/255、zero_point −1） |
| 输出 | int8 `[1,1000]`（scale 1/256、zero_point −128，图末尾是 SOFTMAX） |
| 契约 | `ai_spec.yaml`：`image_rgb` 128×128、`scale {div: 127.5, sub: 1}`、`softmax_argmax` + 1000 个类名 |

## 指标

在 imagenette2-160 验证集（3,925 张，ImageNet 的十类子集）上：

| 指标 | int8 | float 基线 |
|---|---|---|
| Top-1（全 1000 类） | 43.95% | 46.19% |
| Top-5（全 1000 类） | 67.24% | 69.43% |
| 限定十类 Top-1 | 87.46% | 89.61% |

int8 与 float 的 Top-1 一致率 77.35%。校准只用了 imagenette2-160 的 500 张训练图，
该数据不随本仓库分发。

## 出处

- 权重：`keras.applications.MobileNet(alpha=0.25, input_shape=(128,128,3),
  weights="imagenet")`；Keras 3 按 Apache-2.0 发布（旧 `keras-applications` 包是 MIT，
  权重最初译自 TF-Slim 的 checkpoint，Apache-2.0）。
- 修改：本项目把它转换为 int8 TFLite（全整型量化），这一点必须在再分发时注明。
- 训练数据：ImageNet / ILSVRC。其访问条款只允许非商业研究与教学
  （“Researcher shall use the Database only for non-commercial research and
  educational purposes.”）。权重是否受数据集条款约束法律上没有定论，属于残留风险。

## 许可

见 [`LICENSE.md`](LICENSE.md)：权重 Apache-2.0；ImageNet 条款的商用口径请法务确认。
