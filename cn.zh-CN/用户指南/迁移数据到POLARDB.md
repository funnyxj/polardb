# 迁移数据到POLARDB {#concept_vtf_vqq_tdb .concept}

通过使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/document_detail/26592.html)，您可以实现数据库的结构迁移和全量迁移，可以在不影响业务的情况下将数据库平滑迁移至云数据库POLARDB。

本文介绍如何使用DTS通过内网迁移RDS for MySQL数据库到POLARDB数据库。

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

6.  填写目标库信息。**实例类型**设置为**POLARDB**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2108_zh-CN.png)

7.  单击右下角的**授权白名单并进入下一步**。

8.  对于**迁移类型**，选择**结构迁移**和**全量数据迁移**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3032/2109_zh-CN.png)

9.  选择要迁移的对象，单击向右的箭头，将选中的对象添加到右侧。
10. 单击**预检查并启动**。

11. 预检查成功后，单击**下一步**。
12. 确认DTS购买信息，阅读并勾选**服务条款**，单击**立即购买并启动**。

13. 单击目标地域，查看迁移状态。迁移完成时，状态为**已完成**。


