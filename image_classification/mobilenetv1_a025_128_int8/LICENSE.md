Model: MobileNetV1 alpha=0.25, 128x128, ImageNet-1k, int8 TFLite
Weights: Copyright Google LLC / The TensorFlow Authors, Keras Applications.
Upstream notices: tensorflow/models LICENSE (Copyright 2022 Google LLC, all
rights reserved); TF-Slim source headers (Copyright 2017 The TensorFlow
Authors); keras-applications (Copyright (c) 2016 - 2018, the respective
contributors, MIT License).
Modifications Copyright (c) 2026 Nsing Technologies Inc.
License: Apache License, Version 2.0.
Full text: [Apache License 2.0](../../licenses/Apache-2.0.txt).

Upstream: keras.applications.MobileNet(alpha=0.25, input_shape=(128,128,3),
weights="imagenet"); Keras 3 is distributed under Apache-2.0. The legacy
keras-applications package is MIT; the weights were originally translated from
the TensorFlow-Slim checkpoints (tensorflow/models, Apache-2.0).

Modifications: converted to a fully int8 TFLite model by Nsing Technologies Inc. This
modification must be stated when redistributing the file.

Training data: ImageNet/ILSVRC. Its terms of access limit the database to
non-commercial research and educational purposes. Whether those terms reach
the trained weights is legally unsettled. See README.md.

Upstream NOTICE status (verified 2026-11-19): tensorflow/models (the TF-Slim
checkpoint origin) ships LICENSE and AUTHORS but no NOTICE file; the legacy
keras-applications package is MIT and has no NOTICE file; Keras 3
(Apache-2.0) likewise ships no NOTICE file. Apache-2.0 Section 4(d) therefore
carries no additional upstream NOTICE content for these weights; the copyright
notices above are the complete attribution set to retain.
