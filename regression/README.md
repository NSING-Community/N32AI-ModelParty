# Regression

数值回归模型（输出是一个连续量，不是一个类别）。

| 模型 | 任务 | 输入 | 说明 |
|---|---|---|---|
| [ccpp_mlp](ccpp_mlp/) | 机组净输出功率 PE（MW） | float32 `[1,4]`（AT / V / AP / RH） | UCI CCPP 数据集上的小型 MLP |

后处理通常用 `regression_threshold`（阈值 + 上下标签），或直接把浮点输出读出来；见各模型卡。
