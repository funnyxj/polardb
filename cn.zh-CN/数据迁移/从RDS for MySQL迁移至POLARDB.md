# 从RDS for MySQL迁移至POLARDB {#concept_pf2_25y_bgb .concept}

使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/document_detail/26592.html?spm=a2c4g.11186623.2.8.6df54b84N9BVGE)，您可以实现RDS for MySQL到POLARDB的数据迁移。通过DTS增量迁移的存储引擎，可以在源RDS实例不停机的情况下，将数据迁移到目标POLARDB集群。

本文介绍使用DTS将RDS for MySQL迁移至POLARDB的任务配置流程。

## 迁移权限要求 {#section_lrw_l1y_bgb .section}

当使用DTS进行RDS实例与POLARDB集群间的数据迁移时，源RDS实例的账号需拥有读写权限，目的POLARDB集群的账号需拥有迁移对象的ALL权限。

## 迁移任务配置 {#section_owb_pby_bgb .section}

下面详细介绍用户如何使用DTS实现RDS for MySQL实例与POLARDB集群间的数据迁移。

**创建迁移账号**

迁移任务配置时，需要提供源RDS实例及目的POLARDB集群的迁移账号。迁移账号的相关权限详见上面的 迁移权限要求 一节。如果尚未创建迁移账号，您可以参考 [RDS实例账号创建](https://help.aliyun.com/document_detail/26186.html?spm=5176.product26090.6.190.dNyWM8)和[POLARDB集群账号创建](https://help.aliyun.com/document_detail/68508.html?spm=a2c4g.11186623.2.10.3f657dd3d4orj4)。先在源实例及目的集群中创建迁移账号，并将要迁移的库表的读写权限授权给上面创建的账号。

**配置迁移任务**

当上面的所有前置条件都配置完成后，可以开始正式的数据迁移。下面详细介绍迁移任务配置流程。

1.  登录[DTS控制台](https://dts.console.aliyun.com/)。
2.  在页面左侧，选择**数据迁移**。
3.  单击右上角**创建迁移任务**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79403/154725680236105_zh-CN.png)

4.  （可选）填写任务名称。

    DTS 为每个任务自动生成一个任务名称，任务名称没有唯一性要求。您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。

5.  填写源实例信息。

    -   实例类型：选择**RDS实例**。
    -   实例地区：选择RDS实例所在的地域。
    -   RDS实例ID：选择配置迁移的源RDS实例的实例ID。
    -   数据库账号：连接RDS实例的账号。
    -   数据库密码：以上数据库账号的密码。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79403/154725680234082_zh-CN.png)

6.  单击**测试连接**，确认DTS可以连接到源RDS实例。
7.  填写目标POLARDB集群信息。

    -   实例类型：选择**POLARDB**。
    -   实例地区：选择POLARDB集群所在的地域。
    -   POLARDB实例ID：选择配置迁移的目标POLARDB集群的集群ID。
    -   数据库账号：连接POLARDB集群的账号。
    -   数据库密码：以上数据库账号的密码。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79403/154725680234083_zh-CN.png)

8.  单击**测试连接**，确认DTS可以连接到目标POLARDB集群。
9.  单击右下角的**授权白名单并进入下一步**。这个步骤DTS会将DTS服务器的IP地址添加到源RDS实例及目标POLARDB的白名单中，避免因为RDS实例及POLARDB设置了白名单，DTS服务器连接不上实例导致迁移失败。
10. 选择**迁移类型**和**迁移对象**。

    -   **迁移类型**：

        -   结构迁移

            DTS 会将迁移对象的结构定义迁移到目标集群。目前 DTS 支持结构迁移的对象包括：表。其他对象如视图、同义词、触发器、存储过程、存储函数、包、自定义类型等暂不支持。

        -   全量数据迁移

            DTS 会将源数据库迁移对象的存量数据全部迁移到目标集群。

        -   增量迁移

            DTS 会将迁移过程中源实例的数据变更同步到目标集群。如果迁移期间进行了 DDL 操作，那么这些结构变更不会迁移到目标集群。

        如果只需要进行全量迁移，那么迁移类型选择：结构迁移＋全量数据迁移。

        如果需要进行不停机迁移，那么迁移类型选择：结构迁移＋全量数据迁移＋增量数据迁移。

        **说明：** 结构迁移和全量数据迁移均不收费，增量迁移需要收费。

    -   **迁移对象**：选择要迁移的对象，单击向右的箭头，将选中的对象添加到右侧。

        迁移对象的选择粒度细化为：库、表、列三个粒度。默认情况下，对象迁移到目标POLARDB集群后，对象名跟源RDS实例一致。如果您迁移的对象在源实例跟目标集群上名称不同，那么需要使用DTS提供的对象名映射功能，详细使用方式可以参考[库表列映射](https://help.aliyun.com/document_detail/26628.html?spm=5176.doc26624.6.125.Mpn8On)。

        **说明：** 

        -   暂时不支持对系统表的迁移。
        -   目标集群中不能有和迁移对象同名的对象。将鼠标移至右侧框中的对象，单击**编辑**，即可修改迁移后的对象名。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79403/154725680334085_zh-CN.png)

11. 单击右下角的**预检查并启动**，成功后单击**下一步**。

    如果预检查失败，可以单击具体检查项后的检查结果，查看具体的失败详情，并根据失败原因修复后，重新进行预检查。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79403/154725680334084_zh-CN.png)

12. 确认DTS购买信息，阅读并勾选**服务条款**，单击**立即购买并启动**。

    当预检查通过后，可以启动迁移任务，任务启动成功后，可以在任务列表中查看迁移的具体状态及迁移进度。

    如果选择了增量迁移，那么进入增量迁移阶段后，源库的更新写入都会被DTS同步到目标POLARDB集群。迁移任务不会自动结束。如果用户只是为了迁移，那么建议在增量迁移无延迟的状态时，源库停写几分钟，等待增量迁移再次进入无延迟状态后，停止掉迁移任务，直接将业务切换到目标POLARDB集群上即可。

13. 单击目标地域，查看迁移状态。迁移完成时，状态为**已完成**。

    至此，完成RDS实例到POLARDB数据迁移任务配置。


