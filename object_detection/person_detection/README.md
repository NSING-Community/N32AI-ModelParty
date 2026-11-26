**简体中文** | [English](README.en.md)

# Person Detection

TensorFlow Lite Micro 的人体存在性检测模型：96×96 灰度图 → `no_person` / `person`。
它是逐窗口的二分类器，边框/滑窗逻辑在应用侧（参考固件对每个窗口调用一次）。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 分类（2 类：`no_person`、`person`） |
| 模型文件 | `person_detect.tflite`（300,568 B，int8） |
| SHA-256 | `808cfdfc0cf3a6fa6f6fa26bfa379ea97c16d5db7334637766e39c3408502e9d` |
| 输入 | int8 `[1,96,96,1]` 灰度 |
| 输出 | int8 `[1,2]` softmax |
| 契约 | `ai_spec.yaml`：`image_grayscale` 96×96、`scale {div: 127.5, sub: 1}`、`softmax_argmax` |

## 出处

与 `tensorflow/tflite-micro` 的 `tensorflow/lite/micro/models/person_detect.tflite`
**逐字节一致**；上游最后一次改动是 commit
`aeac6f39e5c7475cea20c54e86d41e3a38312546`（2021-08-12，Apache-2.0）。
官方发布的 `person_detect_model_data.cc` 文件头写明
“Copyright 2019 The TensorFlow Authors. All Rights Reserved. Licensed under the
Apache License, Version 2.0.”。本仓库没有做任何修改。

## 训练数据

Visual Wake Words（person / not-person），由 COCO 2014 的训练+验证图像与标注生成。
COCO 标注按 CC BY 4.0 授权，图像逐张按 Flickr 原许可；COCO 条款本身没有非商用限制，
但图像许可中有一部分是 NonCommercial（CC BY-NC 系列），权重是否受图像许可约束
法律上没有定论。**本仓库不包含 COCO/VWW 的图像或标注。**

数据集引用：COCO 2014（COCO Consortium，CC BY 4.0，https://cocodataset.org）；
Visual Wake Words（Chowdhery et al., 2019, arXiv:1906.05721）。

## 指标

源仓库没有记录这份模型的准确率；没有可复核的数字就不在这里写。

## 许可

见 [`LICENSE.md`](LICENSE.md)：Apache-2.0。
