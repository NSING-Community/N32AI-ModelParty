# N32AI Model Party

`n32ai-modelparty` 是一个面向国民技术股份有限公司 MCU 的模型库，按用途分类组织。每个模型文件夹里都有模型文件、`ai_spec.yaml`（预处理 / 后处理契约）、`README.md`（模型卡）和 `LICENSE.md`（权重许可与归属，含许可证全文）。

## 模型一览

| 分类 | 模型 | 任务 | 输入 → 输出 | 训练数据 | 许可 |
|---|---|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | 手写数字 10 类 | float32 `[1,28,28,1]` → `[1,10]` | MNIST | BSD-3-Clause（自有权重）；MNIST 数据许可待确认 |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k 1000 类 | int8 `[1,128,128,3]` → `[1,1000]` | ImageNet | Apache-2.0（权重），ImageNet 条款需法务确认 |
| `object_detection` | [person_detection](object_detection/person_detection/) | 人 / 无人二分类 | int8 `[1,96,96,1]` → `[1,2]` | Visual Wake Words（COCO 2014） | Apache-2.0；商用 `review`（COCO 含部分 NC 图片） |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12 类语音关键词识别（MFCC） | float32 `[1,490]` → `[1,12]` | Google Speech Commands | Apache-2.0（权重）+ CC BY 4.0（数据） |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | 敲击 / 无敲击二分类 | float32 `[1,1,256,3]` → `[1,1]` | 维护者自采 MPU6050 数据 | BSD-3-Clause（自有），商用 `allowed` |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | 不平衡 / 不对中 / 正常三分类 | int8 `[1,4096,4]` → `[1,3]` | KFUPM 四通道振动数据集 | BSD-3-Clause（权重）+ CC BY 4.0（数据） |
| `regression` | [ccpp_mlp](regression/ccpp_mlp/) | 预测机组净输出功率 PE（MW） | float32 `[1,4]` → `[1,1]` | UCI CCPP | BSD-3-Clause（权重）+ CC BY 4.0（数据） |

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



## 许可与出处


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
- **模型文件再分发约束**：每个 `.tflite` 必须与同目录的 `LICENSE.md`（许可证全文与
  署名）一起再分发。模型文件本身不含内嵌许可 metadata，单独提取 `.tflite` 使用不免除
  对应 `LICENSE.md` 的义务；对 Apache-2.0 模型尤其要遵守其第 4 条（向接收者提供许可
  证副本、显著注明修改、保留全部版权/署名声明）。

商业状态（`allowed` / `review` / `prohibited`）以根 `NOTICE`、各模型 `LICENSE.md`
和 `models.yaml` 的 `commercial_use` 为准。当前 `review` 共 3 个，均与训练数据的
外部许可有关：

- `mnist`：MNIST 数据许可状态不明（官方页无声明，Keras 文档称 CC BY-SA 3.0）；
- `mobilenetv1_a025_128_int8`：ImageNet 访问条款限定非商业研究/教学，商用口径待法务确认；
- `person_detection`：训练数据 VWW/COCO 含部分 NonCommercial Flickr 图片，影响待定。

这些是工程事实记录，不是法律意见。
