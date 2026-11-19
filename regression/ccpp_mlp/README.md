# CCPP — Combined Cycle Power Plant Output Regression

从四路环境量预测联合循环机组的净输出功率 PE（MW）：

`AT` 环境温度（°C）、`V` 排气真空（cm Hg）、`AP` 大气压（mbar）、`RH` 相对湿度（%）。

## 概览

| 项 | 值 |
|---|---|
| 任务 | 回归（输出 PE，单位 MW） |
| 模型文件 | `ccpp_mlp_float32.tflite`（3,444 B，float32） |
| SHA-256 | `4d1436b5940391a56572e390968c4b9dc472c39870873a225e050a57f467f38f` |
| 输入 | float32 `[1,4]`（AT、V、AP、RH） |
| 输出 | float32 `[1,1]`（PE，MW） |
| 契约 | `ai_spec.yaml`：`timeseries_window`（`window_len: 1`、`channels: 4`，四特征标准化）、`regression_threshold`（451.42 MW） |

四特征标准化用训练集估计的均值 / 标准差（写在 `ai_spec.yaml` 的 `normalize` 里）：
均值 `[19.6915, 54.3504, 1013.2044, 73.3364]`，标准差
`[7.4319, 12.7244, 5.9579, 14.5091]`。训练时目标也做过标准化，但收敛后把
target mean/std **折叠进最后一层权重**，所以导出的模型直接输出 PE（MW），端侧
不需要反标准化。

`window_len: 1` 是把「一条四特征记录」表达成一窗多通道；输入按 `t * channels + c` 的布局排列（`t` 为窗口内采样点，`c` 为通道）。

## 结构

```
Input(4) -> Dense(16, ReLU) -> Dense(16, ReLU) -> Dense(1)   # 369 参数
```

## 指标

957 条 held-out 测试样本，原始 MW 单位：

| 模型 | RMSE (MW) | MAE (MW) | R² | max \|err\| (MW) |
|---|---:|---:|---:|---:|
| TFLite float32 | 3.8406 | 2.9859 | 0.95054 | 25.4142 |
| TFLite int8（训练工程里的另一个文件） | 3.9166 | 3.0569 | 0.94857 | 25.2287 |
| 基线（训练集均值） | 17.2770 | 15.0950 | -0.0008 | 40.1558 |

数据划分是 `seed=42` 的确定性随机划分（7,654 / 957 / 957）；CCPP 是逐时刻快照，
没有官方时间或机组划分，随机划分的 R² 不代表跨时间、跨机组泛化。

## 后处理说明

`ai_spec.yaml` 的 `regression_threshold` 把 451.42 MW 以上的输出标成 `high_power`、
以下标成 `low_power`，这是演示用的二值展示；模型本身的输出仍是连续的 MW，
需要原始数值时从浮点输出读（`ai_output_to_float`）。

## 出处与许可

维护者用 Keras 3 / TensorFlow 2.21 训练并导出 float32 TFLite，属于本项目自有产物。
训练数据是 **UCI Combined Cycle Power Plant**（Tüfekci & Kaya, 2014），按
**CC BY 4.0** 发布。本仓库不包含该数据集。

CC BY 4.0 的署名（随模型再分发时保留）：

> Tüfekci, P. & Kaya, H. (2014). *Combined Cycle Power Plant* [Dataset].
> UCI Machine Learning Repository. https://doi.org/10.24432/C5002N

权重许可见 [`LICENSE.md`](LICENSE.md)：Apache-2.0；数据集署名与链接必须保留。
