---
title: "模型训练"
weight: 50
draft: false
---

本章节将逐步带领您模型训练。

21.	打开trainer.ipynb
     ![image](/images/pngs/025.png)
22.	点击右上角的Kernel选择，选择conda_tensorflow_p36

23.	导入模型函数，加载训练模型。

    本实验神经网络的模型框架位于 **trainer/** 文件夹中
     ![image](/images/pngs/026.png)

24.	启动训练脚本

    出于现场时间考虑，默认学习3个Epoch
     ![image](/images/pngs/027.png)

    3轮训练后的结果如下所示：
     ![image](/images/pngs/028.png)

    我们可以看到loss以及MAE误差在逐渐减小
