---
title: "数据导入与解密"
weight: 20
chapter: false
draft: false
---

目前加密数据encryption_data.csv如下所示，出于便于演示目的，本实验仅对 **经纬度** 进行了加密。
    ![image](/images/pngs/002.png)

1. 创建解密数据储存桶，用于存储解密后的数据

    ![image](/images/pngs/003.png)


2.	打开[Lambda终端](https://cn-northwest-1.console.amazonaws.cn/lambda/home?region=cn-northwest-1)，创建Lambda层

    由于Lambda函数默认系统不含有pandas等第三方库，本实验通过**层**的方式实现。

    点击左侧扩展栏，选择**层**。

    选择从S3上传文件，编译好的包存储在：`s3://kervin-datalake-taxi-2020/pandas_xlrd.zip` ，随后点击**创建**

    ![image](/images/pngs/004.png)

    ![image](/images/pngs/005.png)

3.	创建Lambda函数

    点击左侧边栏中的函数，点击创建函数。

    ![image](/images/pngs/006.png)

4.	函数语言选择python3.7，函数名称建议设置为myDecryptor，权限保持默认选项，该选择会生成一个新的角色。随后点击创建函数。

    ![image](/images/pngs/007.png)

5.	编辑Lambda函数

    在lambda_function中填入如下代码，注意，请修改UPLOAD_BUCKET_NAME为上述第二步中创建的解密数据存储桶（此时该桶内应该为空）。

    ![image](/images/pngs/008.png)

    代码如下
    ```python
    import boto3
    import pandas as pd

    UPLOAD_BUCKET_NAME = 'kervin-decryp-data'


    DOWNLOAD_BUCKET_NAME = 'kervin-datalake-taxi-2020'
    key = 'encryption_data.csv'
    new_key = 'decryption_data.csv'
    # call s3 bucket
    s3 = boto3.resource('s3')
    bucket = s3.Bucket(DOWNLOAD_BUCKET_NAME)


    def encryption(num):

        newNum = []
        for i in str(num):
            if int(i):
                newNum.append(str(10 - int(i) * 7 % 10))
            else:
                newNum.append(str(0))

        # print int(''.join(newNum))
        return int(''.join(newNum))


    def decryption_num(num, seed):
        return num - encryption(seed) / 1000


    def my_decryption_apply(arrLike, raw_data, seed):
        return decryption_num(arrLike[raw_data], arrLike[seed])


    # lambda function
    def lambda_handler(event, context):
        # download s3 csv file to lambda tmp folder
        print('start download')
        local_file_name = '/tmp/data.csv'  #
        s3.Bucket(DOWNLOAD_BUCKET_NAME).download_file(key, local_file_name)
        print('finish download')

        df = pd.read_csv(local_file_name)
        df['lat'] = df.apply(
            my_decryption_apply, axis=1, args=('lat', 'timestamp'))
        df['lon'] = df.apply(
            my_decryption_apply, axis=1, args=('lon', 'timestamp'))
        df.to_csv(local_file_name, index=False)
        print('start upload')

        # upload file from tmp to s3 key
        upload_bucket = s3.Bucket(UPLOAD_BUCKET_NAME)
        upload_bucket.upload_file(local_file_name, new_key)
        print('finish upload')

        return {
            'message': 'success!!'
        }
    ```

6.	在Lambda函数中添加第6步创建的层

    点击Layers，选择添加层。

    ![image](/images/pngs/009.png)

    下拉框中选择“您的层”，即my-Pandas-Layer

    ![image](/images/pngs/010.png)

    
7.	拖动右侧滚动条，修改Lambda函数的基本设置，修改Lambda内存以及超时限制

    ![image](/images/pngs/011.png)

    ![image](/images/pngs/012.png)

8.  同时为了Lambda函数可以将解密后文件写入S3，此处需修改IAM角色权限，点击底部的“查看xxxxx角色”转至IAM控制台。

    点击附加策略    

    ![image](/images/pngs/013.png)

    搜索S3, 将AmazonS3FullAccess添加到策略中`(生产环境中将采用严格的权限策略)`。

    ![image](/images/pngs/014.png)

    成功后返回Lambda函数基本设置页面，点击 **保存**.
    ![image](/images/pngs/015.png)

    配置成功后，点击右上角的保存
    ![image](/images/pngs/016.png)

9.	执行Lambda函数，进行车辆轨迹解密。

    本实验中通过测试按钮，进行Lambda函数的调用。生产环境中配置触发器即可自动根据S3库中文件的变化调用Lambda。

    点击测试，输入事件名称，然后点击创建

    ![image](/images/pngs/017.png)

    点击测试，执行Lambda函数。等待约1分钟后，解密成功。

    ![image](/images/pngs/018.png)

    此时步骤2中创建的解密S3储存桶中以出现解密后的数据decryption_data.csv，可以下载到本地进行预览。

    ![image](/images/pngs/019.png)

    ![image](/images/pngs/020.png)

    可以看到经纬度数据已经解密。