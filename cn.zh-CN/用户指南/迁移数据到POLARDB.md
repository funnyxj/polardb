# 迁移数据到POLARDB {#concept_vtf_vqq_tdb .concept}

通过使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/document_detail/26592.html)，您可以实现数据库的结构迁移、全量迁移和增量迁移，可以在不影响业务的情况下将数据库平滑迁移至云数据库POLARDB上面。

本文介绍如何使用DTS通过公网迁移RDS MySQL数据库到POLARDB数据库。

## 设置IP白名单 {#section_fcl_zqq_tdb .section}

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com/)。
2.  单击实例ID或集群ID，进入详细信息页面。
3.  在访问信息区域，单击白名单列表后面的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2115_zh-CN.png)图标，在弹出的对话框中添加IP地址0.0.0.0/0（表示允许所有IP地址访问该集群）。

    **说明：** 迁移完成后，需要将IP地址0.0.0.0/0从IP白名单中移除。


## 迁移步骤 {#section_v5x_xqq_tdb .section}

1.  进入[RDS控制台](https://rdsnew.console.aliyun.com/)。

2.  选择地域，然后单击要迁移数据的RDS实例的ID，进入详细信息页面。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2104_zh-CN.png)

3.  单击**迁移数据库**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2105_zh-CN.png)

4.  选择**数据迁移**，单击**创建迁移任务**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2106_zh-CN.png)

5.  填写源数据库信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2107_zh-CN.png)

6.  填写目标库信息。**实例类型**设置为**有公网IP的自建数据库**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2108_zh-CN.png)

7.  单击**授权白名单**。

8.  选择**迁移类型**和**迁移对象**，单击向右的箭头，将选中的对象添加到右侧。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2109_zh-CN.png)

9.  单击**预检查并启动**。

10. 确认DTS购买信息，阅读并勾选**服务条款**，单击**立即购买并启动**。

11. 单击目标地域，查看迁移状态。迁移完成时，状态为**已完成**。


