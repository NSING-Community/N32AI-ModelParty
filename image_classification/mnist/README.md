**简体中文** | [English](README.en.md)

# MNIST Digit Recognition

手写数字 0–9 的 10 类分类模型，适合作为入门示例。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 分类（10 类：`0` … `9`） |
| 模型文件 | `mnist.tflite`（110,916 B，float32） |
| SHA-256 | `1ba966ac590b50b2c695bab1cb95b6b22a78f386d07eeee2e9aa6d16a5c7d64e` |
| 输入 | float32 `[1,28,28,1]` 灰度 |
| 输出 | float32 `[1,10]` softmax |
| 契约 | `ai_spec.yaml`：`image_grayscale` 28×28，`scale.div = 255`，`softmax_argmax` |

## 出处

维护者用官方 MNIST 数据集自训：TensorFlow/Keras 的 `mnist` 加载器 → 训练 →
float32 TFLite。权重是 Nsing Technologies Inc. 自有产物；本仓库不包含 MNIST 数据、
训练脚本或模型产物。

训练数据是 MNIST（Yann LeCun、Corinna Cortes、Christopher J.C. Burges）。官方 MNIST
页面没有许可或使用条款；TensorFlow/Keras 文档称其按 CC BY-SA 3.0 提供，该说法可追溯
到 2018 年的一个 TensorFlow commit，没有给出来源。权重是否构成 MNIST 的改编作品、
CC BY-SA 3.0 的 ShareAlike 是否适用于权重，法律上没有定论；商用前请法务确认。

## 指标

本仓库没有记录这份模型的准确率；没有可复核的数字就不在这里写。

## 许可

见 [`LICENSE.md`](LICENSE.md)：权重 BSD-3-Clause（Nsing Technologies Inc.）；训练数据 MNIST 的许可未定（官方页无声明，Keras 文档称 CC BY-SA 3.0）。`commercial_use: review`，商用前请法务确认。
