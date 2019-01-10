# 将ECS的自建MySQL迁移至POLARDB {#concept_ybs_vzx_bgb .concept}

通过使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/document_detail/26592.html)，您可以实现ECS MySQL到POLARDB的数据迁移。通过DTS增量迁移的存储引擎，可以在源ECS实例上的自建数据库不停机的情况下，将数据迁移到目标POLARDB集群。

本文介绍使用DTS将ECS MySQL迁移至POLARDB的任务配置流程。

## 迁移权限要求 {#section_lrw_l1y_bgb .section}

当使用DTS进行ECS上自建MySQL实例与POLARDB集群间的数据迁移时，不同迁移类型，对源和目标数据库的迁移账号权限要求如下表：

|迁移类型|结构迁移|全量迁移|增量迁移|
|:---|:---|:---|:---|
|源RDS实例|迁移对象的SELECT权限|迁移对象的SELECT权限|迁移对象的SELECT、REPLICATION CLIENT、REPLICATION SLAVE权限|
|目的PolarDB|迁移对象的ALL权限|迁移对象的ALL权限|迁移对象的ALL权限|

## 迁移任务配置 {#section_owb_pby_bgb .section}

下面详细介绍用户如何使用DTS实现ECS自建MySQL实例与PolarDB集群间的数据迁移。

**创建迁移账号**

迁移任务配置时，需要提供源ECS自建数据库及目的POLARDB集群的迁移账号。迁移账号的相关权限详见上面的 迁移权限要求 一节。如果尚未创建迁移账号，您可以参考 [MySQL账号创建手册](https://dev.mysql.com/doc/refman/8.0/en/grant.html)，[POLARDB集群账号创建](https://help.aliyun.com/document_detail/68508.html) 先在源实例及目的集群创建迁移账号，并将要迁移的库表的读写权限授权给上面创建的账号。

**配置迁移任务**

当上面的所有前置条件都配置完成后，就可以开始正式的数据迁移了。下面详细介绍迁移任务配置流程。

1.  进入[DTS控制台](https://dts.console.aliyun.com/)。
2.  在左侧导航栏选择**数据迁移**，然后在右上角单击**创建迁移任务**，开始迁移任务配置。
3.  （可选）填写任务名称。

    DTS 为每个任务自动生成一个任务名称，任务名称没有唯一性要求。您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。

4.  填写源实例的信息。
    -   实例类型：选择**ECS上的自建数据库**。
    -   实例地区：选择ECS实例对应的区域。
    -   ECS实例ID: 配置迁移的源ECS实例的实例ID。DTS支持经典网络、VPC网络的ECS实例。
    -   数据库账号：连接RDS实例的账号。
    -   数据库密码：以上账号的密码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78734/154709625234051_zh-CN.png)

5.  单击**测试连接**，确认DTS可以连接到源POLARDB集群。
6.  填写目标集群的信息。
    -   实例类型：选择**POLARDB**
    -   实例地区：选择目标集群所在的地域。
    -   POLARDB实例ID：输入POLARDB集群的ID。
    -   数据库账号：POLARDB集群的账号。
    -   数据库密码：以上账号的密码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78734/154709625234052_zh-CN.png)

7.  单击**测试连接**，确认DTS可以连接到目标POLARDB集群。
8.  当配置完连接信息后，单击右下角的**授权白名单并进入下一步**，进行白名单授权。这个步骤DTS会将DTS服务器的IP地址添加到源RDS实例及目标POLARDB的白名单中，避免因为RDS实例及POLARDB设置了白名单，DTS服务器连接不上实例导致迁移失败。
9.  选择迁移对象及迁移类型。
    -   迁移类型

        DTS迁移类型支持结构迁移、全量数据迁移及增量迁移。

        如果只需要进行全量迁移，那么迁移类型选择：结构迁移＋全量数据迁移。

        如果需要进行不停机迁移，那么迁移类型选择：结构迁移＋全量数据迁移＋增量数据迁移。

    -   迁移对象

        这个步骤选择要迁移的对象。迁移对象的选择粒度细化为：库、表、列三个粒度。默认情况下，对象迁移到目标POLARDB后，对象名跟源ECS上自建数据库一致。如果您迁移的对象在源实例跟目标集群名称不同，那么需要使用DTS提供的对象名映射功能，详细使用方式可以参考[库表列映射](https://help.aliyun.com/document_detail/26628.html)。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78734/154709625234080_zh-CN.png)

10. 预检查。

    在迁移任务正式启动之前，会先进行前置预检查，只有预检查通过后，才能成功启动迁移。

    如果预检查失败，那么可以单击具体检查项后的按钮，查看具体的失败详情，并根据失败原因修复后，重新进行预检查。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78734/154709625234081_zh-CN.png)

11. 迁移任务。

    当预检查通过后，可以启动迁移任务，任务启动成功后，可以在任务列表中查看迁移的具体状态及迁移进度。

    如果选择了增量迁移，那么进入增量迁移阶段后，源库的更新写入都会被DTS同步到目标POLARDB集群任务不会自动结束。如果用户只是为了迁移，那么建议在增量迁移无延迟的状态时，源库停写几分钟，等待增量迁移再次进入无延迟状态后，停止掉迁移任务，直接将业务切换到目标POLARDB集群上即可。

    至此，完成ECS上自建数据库到POLARDB数据迁移任务配置。


