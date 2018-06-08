# 使用DMS登录数据库实例 {#concept_n2f_4mq_tdb .concept}

[数据管理](https://account.aliyun.com/login/login.htm?oauth_callback=https%3A%2F%2Fdms-rds.aliyun.com)（Data Management Service，简称DMS）是一种集数据管理、结构管理、访问安全、BI图表、数据趋势、数据轨迹、性能与优化和服务器管理于一体的数据管理服务。支持对关系型数据库（MySQL、SQL Server、PostgreSQL等）和NoSQL数据库（MongoDB、Redis等）的管理，同时还支持Linux服务器管理。

## 前提条件 {#section_ofm_qmq_tdb .section}

已创建数据库集群的初始账号或普通账号。具体操作请参见[创建数据库集群的初始账号](https://help.aliyun.com/document_detail/68508.html)。

## 操作步骤 {#section_pfm_qmq_tdb .section}

1.  进入集群列表或实例列表，找到目标集群或实例。
2.  单击集群或实例的ID，或者单击**操作**列中的**管理**。
3.  单击页面右上角的**登录数据库**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3019/2084_zh-CN.png)

4.  在数据库登录页面，输入集群的VPC连接串和端口号（以英文冒号隔开），以及初始账号或普通账号的用户名和密码，然后单击**登录**。

    -   如果您从集群详情页跳转到数据库登录界面，系统默认为您填写集群的VPC连接串和端口号。

    -   如果您从实例详情页跳转到数据库登录界面，系统默认为您填写该实例的VPC连接串和端口号。

    关于如何查看连接串和端口号，请参见[查看数据库连接串](https://help.aliyun.com/document_detail/68510.html)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3019/2085_zh-CN.png)


**说明：** 

-   若您希望浏览器记住该账号的密码，可以先勾选**记住密码**，然后再单击**登录**。

-   若出现以下提示，单击**设置所有实例**或者**设置本实例**，以将DMS服务器的IP段加入到所有实例或当前实例的IP白名单中，否则将无法登录。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3019/2087_zh-CN.png)


