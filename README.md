**简体中文** | [English](README.en.md)

# N32AI Model Party

`n32ai-modelparty` 是一个面向国民技术股份有限公司系列 MCU 的端侧模型库，按用途分类组织。每个模型文件夹里都有模型文件、`ai_spec.yaml`（预处理 / 后处理契约）、`README.md`（模型说明）和 `LICENSE.md`（权重许可与归属；Apache-2.0 / CC BY 4.0 全文见 `licenses/`）。

## 模型一览

| 分类 | 模型 | 任务 | 输入 → 输出 |
|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | 手写数字 10 类 | float32 `[1,28,28,1]` → `[1,10]` |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k 1000 类 | int8 `[1,128,128,3]` → `[1,1000]` |
| `object_detection` | [person_detection](object_detection/person_detection/) | 人 / 无人二分类 | int8 `[1,96,96,1]` → `[1,2]` |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12 类语音关键词识别（MFCC） | float32 `[1,490]` → `[1,12]` |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | 敲击 / 无敲击二分类 | float32 `[1,1,256,3]` → `[1,1]` |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | 不平衡 / 不对中 / 正常三分类 | int8 `[1,4096,4]` → `[1,3]` |
| `regression` | [ccpp_mlp](regression/ccpp_mlp/) | 预测机组净输出功率 PE（MW） | float32 `[1,4]` → `[1,1]` |
| `arc_fault_detection` | [arc_cnn_int8](arc_fault_detection/arc_cnn_int8/) | 电弧 / 正常二分类 | int8 `[1,500,8]` → `[1,2]` |

## 目录结构

```
<category>/
└── <model>/
    ├── README.md                  模型卡：用途、输入输出、数据集、指标、出处与许可
    ├── LICENSE.md                 权重许可与归属；Apache-2.0 / CC BY 4.0 全文见 licenses/
    ├── <model>.tflite             模型文件
    └── ai_spec.yaml               预处理 / 后处理契约（与模型文件同目录）

licenses/
├── Apache-2.0.txt                 Apache-2.0 全文
└── CC-BY-4.0.txt                  CC BY 4.0 全文
```

## 许可与出处

- 根 `LICENSE`：BSD 3-Clause，覆盖仓库自有内容（README、`models.yaml`、
  `ai_spec.yaml`、`mnist` / `tap_detector` / `kfupm_1dcnn` / `ccpp_mlp` / `arc_cnn_int8` 的自训权重等）。
- 根 `NOTICE`：Nsing Technologies Inc. 的版权/许可范围、第三方模型与数据集清单、
  商标声明。
- 每个模型文件夹的 `LICENSE.md`：权重许可声明、修改说明和数据集归属；Apache-2.0 /
  CC BY 4.0 全文在 `licenses/`。
- `licenses/`：Apache-2.0 和 CC BY 4.0 的完整文本，供各模型 `LICENSE.md` 引用；
  再分发时必须随附。
- `models.yaml`：机器可读的 `license` / `copyright` / `dataset_license` /
  `commercial_use` / `sha256` / `provenance`。
- 第三方模型保留各自的许可，**不能**被根许可覆盖。
- **本仓库不包含任何训练数据集**，只分发训练好的权重。数据集许可的义务（署名、
  引用、再分发条件）写在各模型卡、模型 `LICENSE.md` 和根 `NOTICE` 里；CC BY 4.0 允许再分发，
  但必须署名、给出许可链接并注明修改。
- **模型文件再分发约束**：每个 `.tflite` 必须与同目录的 `LICENSE.md`（许可声明与署名）
  以及其引用的 `licenses/` 全文一起再分发。模型文件本身不含内嵌许可 metadata，单独提取
  `.tflite` 使用不免除对应 `LICENSE.md` 和 `licenses/` 的义务；对 Apache-2.0 模型尤其
  要遵守其第 4 条（向接收者提供许可证副本、显著注明修改、保留全部版权/署名声明）。

### Demo / review 模型

以下模型仅作为 demo / 研究模型提供，不进入商业发布包。完整许可证与署名见各模型 `LICENSE.md` 与 `licenses/`。

- `mnist`
- `mobilenetv1_a025_128_int8`
- `person_detection`

商业状态（`allowed` / `review` / `prohibited`）以根 `NOTICE`、各模型 `LICENSE.md` 和 `models.yaml` 的 `commercial_use` 为准。这些是工程事实记录，不是法律意见。
