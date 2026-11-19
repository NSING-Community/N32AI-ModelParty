# Regression

数值回归模型（输出是一个连续量，不是一个类别）。

| 模型 | 任务 | 输入 | 说明 |
|---|---|---|---|
| [ccpp_mlp](ccpp_mlp/) | 机组净输出功率 PE（MW） | float32 `[1,4]`（AT / V / AP / RH） | UCI CCPP 数据集上的小型 MLP |

这一分类是 N32AI Model Zoo 在 STM32 Model Zoo 的分类之外新增的：该参考库按视觉 /
音频 / 姿态等用途分类，没有放通用回归的位置。回归模型的后处理通常用
`regression_threshold`（阈值 + 上下标签）或直接把浮点输出读出来，见各模型卡。
