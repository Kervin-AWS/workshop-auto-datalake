---
title: "运用Quickset配置您的数据盘面"
date: 2018-10-03T10:17:52-07:00
draft: false
weight: 20
---
<div style="text-align: center"><h2></h2></div>

进入AWS 控制台，打开QuickSight

13.	点击右上角 Manage data
    ![image](/images/pngs/102.png)

14.	点击左上角 New dataset
15.	数据源选择中，选择Athena
    ![image](/images/pngs/103.png)

16.	选择在上述教程中创建好的Athena数据源名称，此处为 `taxi-trajectory-db`
    ![image](/images/pngs/104.png)

    点击 Validate connection, 当出现绿色对勾时表示数据源测试连接成功

    ![image](/images/pngs/105.png)
    
    点击右侧蓝色 **Create data source** 按钮
    
    随后选择上述步骤中创建的**database**名，数据表名
    
    随后点击**Select**
    ![image](/images/pngs/106.png)

    点击**Visualize**

17.	点击Add visual

    ![image](/images/pngs/107.png)

    选择底部地图模块    
    ![image](/images/pngs/108.png)

    按下图将指定字段拖拽到图标上方的窗格中
    ![image](/images/pngs/109.png)

18.	设置过滤器
    点击Filter，选择id作为过滤变量
    ![image](/images/pngs/110.png)

    在完成下一步参变量设置后，我们将会继续配置过滤器

19.	设置过滤参数
    按下图设置名为 maxID的参数，设置data 类型为Integer
    ![image](/images/pngs/111.png)

20.	点击配置filter

    ![image](/images/pngs/112.png)

    按下图进行配置ID的上限
    ![image](/images/pngs/113.png)
    ![image](/images/pngs/114.png)

21.	添加控制单元
    ![image](/images/pngs/115.png)
    ![image](/images/pngs/116.png)
    
    点击Add，则实现了对车辆的数目的控制器的添加。
    
    通过输入数字，可以控制地图上出现的车辆。
    ![image](/images/pngs/117.png)

22.	点击Share，选择发布报表。
    ![image](/images/pngs/118.png)
