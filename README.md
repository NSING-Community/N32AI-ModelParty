# N32AI Model Zoo

N32EmbedAI 的预置模型库：面向 **N32H787（Cortex-M7 + 可选 NPU）** 的 TFLite 模型，
按用途分类组织。每个模型文件夹里都有模型文件、`ai_spec.yaml`（预处理 / 后处理契约）、
`README.md`（模型卡）和 `LICENSE`（权重与训练数据的许可说明）。

N32EmbedAI 的 `generate` / `parse` / `evaluate` 可以直接吃这里的路径——`ai_spec.yaml`
必须和模型文件放在同一目录，工具不会猜前后处理参数。

## 模型一览

| 分类 | 模型 | 任务 | 输入 → 输出 | 训练数据 | 许可 |
|---|---|---|---|---|---|
| `image_classification` | [mnist](image_classification/mnist/) | 手写数字 10 类 | float32 `[1,28,28,1]` → `[1,10]` | MNIST | **未确认**（见模型卡） |
| `image_classification` | [mobilenetv1_a025_128_int8](image_classification/mobilenetv1_a025_128_int8/) | ImageNet-1k 1000 类 | int8 `[1,128,128,3]` → `[1,1000]` | ImageNet | Apache-2.0（权重），ImageNet 条款需法务确认 |
| `object_detection` | [person_detection](object_detection/person_detection/) | 人 / 无人二分类 | int8 `[1,96,96,1]` → `[1,2]` | Visual Wake Words（COCO 2014） | Apache-2.0 |
| `audio_event_detection` | [ds_cnn_s](audio_event_detection/ds_cnn_s/) | 12 类关键词识别（MFCC） | float32 `[1,490]` → `[1,12]` | Google Speech Commands | Apache-2.0（权重）+ CC BY 4.0（数据） |
| `human_activity_recognition` | [tap_detector](human_activity_recognition/tap_detector/) | 敲击 / 无敲击二分类 | float32 `[1,1,256,3]` → `[1,1]` | 维护者自采 MPU6050 数据 | Apache-2.0（自有） |
| `predictive_maintenance` | [kfupm_1dcnn](predictive_maintenance/kfupm_1dcnn/) | 不平衡 / 不对中 / 正常三分类 | int8 `[1,4096,4]` → `[1,3]` | KFUPM 四通道振动数据集 | Apache-2.0（权重）+ CC BY 4.0（数据） |

## 目录结构

```
<category>/<model>/
├── README.md      模型卡：用途、输入输出、数据集、指标、出处与许可
├── LICENSE        该模型权重的许可与随附归属
├── <model>.tflite 模型文件
└── ai_spec.yaml   预处理 / 后处理契约（N32EmbedAI 生成工程时读取）
```

分类沿用 STM32 Model Zoo 的组织方式：`image_classification` / `object_detection` /
`audio_event_detection` / `human_activity_recognition` / `predictive_maintenance`。
新增模型时按同样的四件套放进去，并在根 README 的表格里加一行。

## 在 N32EmbedAI 里使用

```bash
# 解析
python aistudio.py parse image_classification/mobilenetv1_a025_128_int8/mobilenetv1_a025_128_int8.tflite --json

# 生成库（随库带 TFLM + CMSIS-NN）
python aistudio.py generate image_classification/mobilenetv1_a025_128_int8/mobilenetv1_a025_128_int8.tflite -p out/lib --lib

# 本机精度评估（需要 .npz 测试集）
python aistudio.py evaluate image_classification/mnist/mnist.tflite --data mnist-test.npz

# 量化（只接受 FP32 源）
python aistudio.py quantize image_classification/mnist/mnist.tflite --data mnist-calib.npz -o mnist_int8.tflite
```

把模型文件夹整个拷进 N32EmbedAI 的 `models/preset/<槽位>/` 也能当预置模型用；`ai_spec.yaml`
跟着走，生成工程时预/后处理代码就按它生成。

## 许可与出处

- 每个模型文件夹的 `LICENSE` 写该模型权重的许可与必须随附的归属行；
  `README.md` 里另写训练数据集及其许可。
- 仓库自身的内容（各 README、`ai_spec.yaml`、`tap_detector` 与 `kfupm_1dcnn` 的权重）
  按 Apache-2.0 发布；第三方模型保留各自的许可，**不能**被根许可覆盖。
- `licenses/Apache-2.0.txt` 与 `licenses/CC-BY-4.0.txt` 是全文；各模型文件夹里引用它们。
- **本仓库不包含任何训练数据集**，只分发训练好的权重。数据集许可的义务（署名、
  引用、不得再分发图像/标注）写在各模型卡里。

### 两个必须先确认的缺口

1. **mnist** 的权重来自一个未记录的上游示例，具体项目和 commit 不明，无法确认再分发
   许可。模型卡的 LICENSE 标为未确认；把本仓库用于商业产品前请补记录或替换该模型。
2. **mobilenetv1_a025_128_int8** 的权重是 Keras MobileNet（Apache-2.0），但训练数据
   ImageNet 的访问条款只允许非商业研究与教学。权重是否受该条款约束法律上没有定论，
   属于残留风险；商用前请公司法务确认，或换用非 ImageNet 训练的模型。

这些说明是工程事实记录，不是法律意见。

## 维护

模型文件与 `ai_spec.yaml` 与 N32EmbedAI 仓库的 `models/preset/` 保持同源；更新模型时
两边的 `README.md` / `LICENSE` / 哈希一起改。模型卡里的 SHA-256 用于核对文件完整性。
