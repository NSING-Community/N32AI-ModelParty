# Image Classification

静态图片分类模型。

| 模型 | 类别数 | 输入 | 说明 |
|---|---|---|---|
| [mnist](mnist/) | 10 | float32 `[1,28,28,1]` | 灰度手写数字；上游出处未记录，商用前需处理 |
| [mobilenetv1_a025_128_int8](mobilenetv1_a025_128_int8/) | 1000 | int8 `[1,128,128,3]` | ImageNet-1k，Keras MobileNetV1 α=0.25 @128 |

两个都是分类模型，`ai_spec.yaml` 是 `image_rgb` / `image_grayscale` + `softmax_argmax`。
ImageNet 那一份的类别名已经写全（1000 个，Keras 索引顺序）。
