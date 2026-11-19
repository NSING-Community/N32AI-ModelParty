# MNIST Digit Recognition

手写数字 0–9 的 10 类分类模型，N32EmbedAI 的入门预置。

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

`models/preset` 的出处表把权重记为「上游示例」（TensorFlow/Keras 风格的
`mnist_cnn` 网络），但**具体上游项目、训练脚本与 commit 没有记录**，无法据此确认
再分发许可。N32EmbedAI 仓库里的 MNIST 数据工程只做数据准备，不含训练脚本或模型产物。

训练数据是 MNIST（Yann LeCun、Corinna Cortes、Christopher J.C. Burges）。官方 MNIST
页面没有许可或使用条款；TensorFlow/Keras 文档称其按 CC BY-SA 3.0 提供，该说法可追溯
到 2018 年的一个 TensorFlow commit，没有给出来源。因此这份权重的许可状态标为
**未确认（NOASSERTION）**。本仓库不包含 MNIST 数据。

## 指标

源仓库没有记录这份模型的准确率；没有可复核的数字就不在这里写。

## 许可

见 [`LICENSE`](LICENSE)：上游未确认，**商业使用前必须补记录或替换该模型**。
