---
title: "数据抓取"
weight: 30
draft: false
---

本章节将会运用AWS Glue 服务完成数据的抓取

10.	进入Athena，点击连接数据源
    ![image](/images/pngs/021.png)

    选择AWS Glue数据目录，点击下一步

    ![image](/images/pngs/022.png)

    点击连接到AWS Glue

11.	配置Glue

    输入您的爬网程序名称，此处输入taxi-data-glue

    ![image](/images/pngs/023.png)

    写入第2步中创建的解密数据s3地址，注意，此处地址写到csv文件的文件夹即可，不要将csv文件写入到地址内，
    ![image](/images/pngs/024.png)

    创建Glue IAM角色

    ![image](/images/pngs/025.png)

    进入IAM控制台，选择创建的角色，附加S3的相关权限。

    ![image](/images/pngs/026.png)
    完成后返回Glue页面。

    计划选择按需运行，点击下一步。

    输出配置界面，选择添加数据库

    ![image](/images/pngs/027.png)

    此处输入taxi_trajectory_db,并在前缀里填入taxi_ 。

    ![image](/images/pngs/028.png)

    完成后，选中爬网程序，点击**运行爬网程序**

    ![image](/images/pngs/029.png)

    运行成功后，可以在左侧边栏中的数据库中表中看见与S3中的同名表。

    ![image](/images/pngs/030.png)

    然后返回Athena。
