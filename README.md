# N32AI Model Party

`n32ai-modelparty`（N32AI Model Party）是一个面向 **N32H787** 等 MCU 的 TFLite 模型库，按用途分类组织。每个模型文件夹里都有模型文件、`ai_spec.yaml`（预处理 / 后处理契约）、`README.md`（模型卡）和 `LICENSE.md`（权重许可与归属，含许可证全文）。仓库根 `LICENSE` 是自有内容的 BSD 3-Clause 许可，根 `NOTICE` 列出第三方模型、数据集和商标声明。

`ai_spec.yaml` 记录预处理 / 后处理参数，必须和模型文件放在同一目录。

## 模型一览

| 分类 | 模型 | 任务 | 输入 → 输出 | 训练数据 | 许可 |
|---|---|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | 手写数字 10 类 | float32 `[1,28,28,1]` → `[1,10]` | MNIST | BSD-3-Clause（自有权重）；MNIST 数据许可待确认 |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k 1000 类 | int8 `[1,128,128,3]` → `[1,1000]` | ImageNet | Apache-2.0（权重），ImageNet 条款需法务确认 |
| `object_detection` | [person_detection](object_detection/person_detection/) | 人 / 无人二分类 | int8 `[1,96,96,1]` → `[1,2]` | Visual Wake Words（COCO 2014） | Apache-2.0 |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12 类关键词识别（MFCC） | float32 `[1,490]` → `[1,12]` | Google Speech Commands | Apache-2.0（权重）+ CC BY 4.0（数据） |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | 敲击 / 无敲击二分类 | float32 `[1,1,256,3]` → `[1,1]` | 维护者自采 MPU6050 数据 | BSD-3-Clause（自有） |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | 不平衡 / 不对中 / 正常三分类 | int8 `[1,4096,4]` → `[1,3]` | KFUPM 四通道振动数据集 | BSD-3-Clause（权重）+ CC BY 4.0（数据） |
| `regression` | [ccpp_mlp](regression/ccpp_mlp/) | 机组净输出功率 PE（MW） | float32 `[1,4]` → `[1,1]` | UCI CCPP | BSD-3-Clause（权重）+ CC BY 4.0（数据） |

## 目录结构

```
<category>/
└── <model>/
    ├── README.md                  模型卡：用途、输入输出、数据集、指标、出处与许可
    ├── LICENSE.md                 权重许可与归属，含许可证全文
    │                              （第三方模型 Apache-2.0；自有模型 BSD-3-Clause；
    │                               数据集为 CC BY 4.0 时也含 CC BY 4.0 全文）
    ├── <model>.tflite             模型文件
    └── ai_spec.yaml               预处理 / 后处理契约（与模型文件同目录）
```

模型按用途分类：`image_classification` / `object_detection` / `audio_event_detection` /
`human_activity_recognition` / `predictive_maintenance` / `regression`。
新增模型时同步更新模型文件夹、根 `NOTICE`、`models.yaml` 和本 README 的表格。


## 许可与出处

本仓库按 Nsing Technologies Inc. 的发布规范组织：

- 根 `LICENSE`：BSD 3-Clause，覆盖仓库自有内容（README、`models.yaml`、
  `ai_spec.yaml`、`mnist` / `tap_detector` / `kfupm_1dcnn` / `ccpp_mlp` 的自训权重等）。
- 根 `NOTICE`：Nsing Technologies Inc. 的版权/许可范围、第三方模型与数据集清单、
  商标声明。
- 每个模型文件夹的 `LICENSE.md`：该模型权重的许可证全文（第三方模型保留上游
  Apache-2.0；自有模型 BSD-3-Clause），以及数据集许可文本（如 CC BY 4.0）。
- `models.yaml`：机器可读的 `license` / `copyright` / `dataset_license` /
  `commercial_use` / `sha256` / `provenance`。
- 第三方模型保留各自的许可，**不能**被根许可覆盖。
- **本仓库不包含任何训练数据集**，只分发训练好的权重。数据集许可的义务（署名、
  引用、再分发条件）写在各模型卡和模型 `LICENSE.md` 里；CC BY 4.0 允许再分发，
  但必须署名、给出许可链接并注明修改。

商业状态（`allowed` / `review` / `prohibited`）以根 `NOTICE`、各模型 `LICENSE.md`
和 `models.yaml` 的 `commercial_use` 为准；mnist 为 `review`（MNIST 数据许可待确认），mobilenet 为 `review`。
这些是工程事实记录，不是法律意见。

## 维护

更新模型时同步 `README.md` / 模型 `LICENSE.md` / 哈希，并同步根 `NOTICE`、
`models.yaml`（`copyright` / `dataset_license` / `commercial_use` / `sha256` /
`provenance`）和模型 `LICENSE.md` 里的许可证全文。模型卡里的 SHA-256 用于核对文件完整性。
