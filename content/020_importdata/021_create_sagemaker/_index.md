---
title: "创建SageMaker笔记本实例"
date: 2018-10-03T10:17:52-07:00
draft: false
weight: 10
---
<div style="text-align: center"><h2></h2></div>

1.	登陆SageMaker控制台，点击右上角的创建笔记本实例
    ![image](/images/pngs/001.png)

2.	填写笔记本实例的名称，如battery-prediction，选择合适的实例类型，建议**ml.t2.xlarge**卷大小请设置为 **>= 20g**，本实验建议为20g
    ![image](/images/pngs/002.png)

3.	权限和加密部分，选择“创建新角色”
    ![image](/images/pngs/003.png)

    ![image](/images/pngs/004.png)

    保持默认，然后选择创建角色。然后点击“创建笔记本实例“
    ![image](/images/pngs/005.png)

    等待实例加载，大约需要一到两分钟。
    ![image](/images/pngs/006.png)

4.	点击**“打开JupyterLab”**，进入SageMaker JupyterLab。
    ![image](/images/pngs/007.png)
 
    至此您已经完成SageMaker Notebook的创建。
