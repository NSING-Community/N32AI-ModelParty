# Arc Fault Detection

光伏直流电弧故障检测模型。

| 模型 | 任务 | 输入 | 说明 |
|---|---|---|---|
| [arc_cnn_int8](arc_cnn_int8/) | 电弧 / 正常二分类 | int8 `[1,500,8]` | 4 kHz 电流/电压 AGC 特征，int8 TFLite |
