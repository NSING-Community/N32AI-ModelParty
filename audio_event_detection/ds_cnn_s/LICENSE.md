Model: DS-CNN-S Keyword Spotting (DS_CNN_S.tflite)
Weights: Copyright (C) 2018 Arm Limited or its affiliates.
Upstream notices: Copyright 2017 The TensorFlow Authors. All Rights Reserved.
Modifications Copyright 2017 Arm Inc. All Rights Reserved.
Modifications Copyright (c) 2026 Nsing Technologies Inc.
License: Apache License, Version 2.0.
Full text: [Apache License 2.0](../../licenses/Apache-2.0.txt).

Upstream: ARM-software/ML-KWS-for-MCU, Pretrained_models/DS_CNN/DS_CNN_S.pb
(Apache-2.0). Modifications: converted from the TensorFlow frozen graph to a
TFLite flatbuffer with TOCO by Nsing Technologies Inc. During conversion the
batch-normalization parameters were folded into the preceding convolution
weights/biases (deterministic BN folding; no retraining or quantization).
This conversion must be stated when redistributing the file.

Training data: Google Speech Commands, licensed CC BY 4.0. When redistributing
this model, keep the dataset attribution too:
  Google Speech Commands dataset (Pete Warden / Google), CC BY 4.0,
  https://creativecommons.org/licenses/by/4.0/
  https://www.tensorflow.org/datasets/catalog/speech_commands
  Warden, P. (2018). Speech Commands: A Dataset for Limited-Vocabulary
  Speech Recognition. arXiv:1804.03209.
Dataset license text: [Creative Commons Attribution 4.0 International](../../licenses/CC-BY-4.0.txt).
The dataset itself is not distributed here.

Upstream NOTICE status (verified 2026-11-19): ARM-software/ML-KWS-for-MCU
ships a LICENSE file (Apache-2.0) but no NOTICE file. Apache-2.0 Section 4(d)
therefore carries no additional upstream NOTICE content for this model; the
Arm and TensorFlow copyright notices above are the complete attribution set
to retain.
