---
title: "数据预览"
weight: 40
draft: false
--- 

12.	点击刷新按钮，可以看见Athena已经从Glue中获取数据。
    
    编写SQL语句进行数据预览
    
    在右侧查询中输入以下语句：

    ```sql
    Select * from taxi_xxx
    Limit 100
    ```

    可以对数据进行预览。
    ![image](/images/pngs/031.png)

    如果出现运行查询按钮为灰色，点击上方创建S3 Athena 输出桶
    ![image](/images/pngs/032.png)

13.	将unix timestamp转化为time格式
    
    新建查询，在其中输入，请将表名更换为自己的表名

    ```SQL
    CREATE TABLE taxi_etl_decryp_data 
    as 
    SELECT lat,
            lon,
            passenger,
            from_unixtime(timestamp) AS time,
            id,
            passenger_cnt
    FROM taxikervin_decryp_data;
    ```
    点击运行，我们可以看到左侧出现新的表，且出现了timestamp格式的字段
    ![image](/images/pngs/033.png)




