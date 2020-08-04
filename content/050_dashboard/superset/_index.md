---
title: "运用Superset配置您的数据盘面"
date: 2018-10-03T10:17:52-07:00
draft: false
weight: 10
---
<div style="text-align: center"><h2></h2></div>

14.	基于docker安装Superset

    本实验运用开源BI Superset作为前端盘面展示，用户需要提前配置好[docker](https://www.docker.com/get-started)。Superset Docker镜像[https://hub.docker.com/r/tylerfowler/superset](https://hub.docker.com/r/tylerfowler/superset)
    
    打开terminal，执行
    
    ```shell
    docker pull tylerfowler/superset
    ```
    下载完成后，执行

    ```shell
    docker run -d --name superset -p 8088:8088 tylerfowler/superset
    ```

    随后打开浏览器，输入[http://localhost:8088/login/](http://localhost:8088/login/)

    ![image](/images/pngs/034.png)

    `username: admin`

    `password: superset`

15.	在Superset中配置Amazon Athena数据源

    进入docker命令行

    方法1: 点击docker桌面版按钮
    ![image](/images/pngs/035.png)

    方法2:

    通过 `sudo docker ps`  查看运行中容器名称
    ![image](/images/pngs/036.png)

    输入
    ```shell
    sudo docker exec -it 472bc306878d /bin/bash 
    ```

    进入docker,如下图所示
    ![image](/images/pngs/037.png)

    在docker内执行：
    ```shell
    pip install PyAthenaJDBC -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install PyAthena -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install PyAthena -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install psycopg2 -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install awscli -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install botocore==1.17.6 -i https://pypi.tuna.tsinghua.edu.cn/simple
    ```

16.	Docker内配置aws账户信息

    进入aws 控制台查看账号
    ![image](/images/pngs/038.png)

    进入docker，输入`aws configure`，配置相关信息
    ![image](/images/pngs/039.png)

17.	进入Superset 获取Athena数据表
    
    点击添加Databases

    ![image](/images/pngs/040.png)

    填写Database名称，此处填写`taxi_db`
    
    填写Athena连接URL：格式如下

    `awsathena+rest://@athena.cn-northwest-1.amazonaws.com.cn/<Athena中数数据库名>?s3_staging_dir=<用来存储查询结果的S3地址>`

    awsathena+rest://@athena.cn-northwest-1.amazonaws.com.cn/taxi-trajectory-db?s3_staging_dir=s3://auto-immersion-day-sf-taxi-data-set/athena-output/

    **测试连接**
    ![image](/images/pngs/041.png)

    成功后点击save  
    ![image](/images/pngs/042.png)

    配置数据表
    ![image](/images/pngs/043.png)

    选择数据库taxi_db，填写表名为taxi_etl_decryp_data，然后点击save
    ![image](/images/pngs/044.png)

18.	制作Superset线条数据分析图
    点击Charts，绘制车辆载客数随时间的变化模块，限制车辆id为<5样本来进行显示。
    
    按照下图配置聚合函数
    ![image](/images/pngs/045.png)

    然后点击save
    ![image](/images/pngs/046.png)

19.	制作Superset Map

    新建一个Chart，命名为`trajectory_analysis`

    按照图中配置筛选条件
    ![image](/images/pngs/047.png)

    点击save保存chart。

{{% notice info %}}
注意，此处您可能没有底图，后续会指导您配置。
{{% /notice  %}}

20.	配置Superset 盘面

    点击Dashboards，点击新建，新增Row、charts
    ![image](/images/pngs/048.png)



21.	配置Superset Mapbox 底图

    用户可以在[Mapbox](https://docs.mapbox.com/api/#access-tokens-and-token-scopes)申请自己的`Mapbox_API_Key`

    本实验提供实验用key：

    `sk.eyJ1Ijoia2Vydmlu请联系现场工作人员YyJ9.aWMoRw2QA_-7SD2UtLv58w`

    随后在本地环境中，**打开terminal**，输入如下指令，将docker中的Superset配置文件拷贝出来（注意，请将`docker ID`更换为自己的docker ID）

    ```shell
    sudo docker cp 472bc306878d:/superset/superset_config.py ./
    ```

    在当前文件夹找到superset_config.py，将上述的`Mapbox_API_Key`填写到该文件中，如下图所示。

    ![image](/images/pngs/049.png)
    
    随后执行
    ```shell
    sudo docker cp  ./superset_config.py  472bc306878d:/superset/
    ```
    将该文件传回docker。

    刷新superset页面，即可出现地图底图
    ![image](/images/pngs/050.png)