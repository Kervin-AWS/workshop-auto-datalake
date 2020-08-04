---
title: "Overview"
date: 2018-10-03T10:17:52-07:00
draft: false
weight: 10
---

Datalake 是云计算利用数据赋能业务的一个关键方法，本实验将基于车联网场景，实现对批量车辆轨迹的云端存储、分析与展示。

[本实验数据集](http://crawdad.org/epfl/mobility/20090224/)来自于SanFrancisco **500辆出租车30天**的（2008年5月17日-6月10日）行驶数据。

本实验在中国区账号的服务架构如下
![image](/images/pngs/001.png)

global区的服务架构如下
![image](/images/pngs/101.png)


{{% notice info %}}
如果您运用中国区账号进行实验，您需要提前配置好docker（数据盘面制作过程将会用到）
{{% /notice  %}}


本实验将从引导您上述服务实现：

- 数据导入与解密

- 数据抓取

- 数据预览

- 数据盘面制作