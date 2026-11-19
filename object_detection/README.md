# Object Detection

目标检测 / 存在性判定模型。

| 模型 | 任务 | 输入 | 说明 |
|---|---|---|---|
| [person_detection](person_detection/) | 人 / 无人二分类 | int8 `[1,96,96,1]` | TensorFlow Lite Micro 的人体检测示例模型 |

这一份是逐区域二分类的「人体存在性」模型（不是输出边框的检测器）：对 96×96 灰度图
逐窗口推理，返回 `no_person` / `person` 的 softmax。参考固件对每个窗口跑一次，
滑窗逻辑在应用侧。
