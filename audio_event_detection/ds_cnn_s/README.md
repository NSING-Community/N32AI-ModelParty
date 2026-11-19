# DS-CNN-S Keyword Spotting

Arm 的 DS-CNN-S 关键词识别模型（12 类），输入是 49 帧 × 10 维 MFCC。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 分类（12 类：`silence`、`unknown`、`yes`、`no`、`up`、`down`、`left`、`right`、`on`、`off`、`stop`、`go`） |
| 模型文件 | `DS_CNN_S.tflite`（95,480 B，float32） |
| SHA-256 | `e4e6af6a3fc7db4f7fdea3430da82f979ad9c342082d6afab13c6810ea85c907` |
| 输入 | float32 `[1,490]`（49 帧 × 10 维 MFCC） |
| 输出 | float32 `[1,12]` softmax |
| 契约 | `ai_spec.yaml`：`audio_mfcc`（16 kHz、30 ms / 20 ms、40 个 mel 频带、10 个系数）、`softmax_argmax` |

## 出处

权重来自 `ARM-software/ML-KWS-for-MCU` 的
`Pretrained_models/DS_CNN/DS_CNN_S.pb`（TensorFlow 冻结图，Apache-2.0）。
该上游仓库没有 `.tflite`；本文件是本项目用 TOCO 转出来的 float32 TFLite，
**转换步骤与时间没有记录**，但文件与 N32 的 KWS 参考工程里的副本逐字节一致。
修改说明：TensorFlow 图 → TFLite（TOCO 格式转换），权重未变。

## 训练数据

Google Speech Commands v0.01 / v0.02，按 **CC BY 4.0** 授权（Google 2017 年的发布公告
与 Warden 2018, arXiv:1804.03209 第 2 节都这样写）。商用允许，条件是署名与许可链接；
没有 ShareAlike。**本仓库不包含 Speech Commands 数据。**

## 指标

源仓库没有记录这份模型的准确率；没有可复核的数字就不在这里写。

## 许可

见 [`LICENSE`](LICENSE)：权重 Apache-2.0，数据 CC BY 4.0，署名要求见文件内。
