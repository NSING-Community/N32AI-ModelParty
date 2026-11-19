# Audio Event Detection

音频事件 / 关键词识别模型，输入是 MFCC 特征而不是原始波形。

| 模型 | 任务 | 输入 | 说明 |
|---|---|---|---|
| [ds_cnn_s](ds_cnn_s/) | 12 类关键词识别 | float32 `[1,490]`（49 帧 × 10 维 MFCC） | Arm DS-CNN-S，Google Speech Commands |

`ai_spec.yaml` 的 `audio_mfcc` 段给出 16 kHz、30 ms 窗、20 ms 步长、40 个 mel 滤波器、
10 个 DCT 系数；生成的固件前端会按这张表算 MFCC，训练侧的表与它逐项对应。
