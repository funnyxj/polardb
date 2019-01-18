# 从AWS Aurora迁移至POLARDB {#concept_jl1_mw3_kgb .concept}

## 背景信息 {#section_mhf_p1t_lgb .section}

本文介绍使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/product/26590.html)，将 AWS Aurora 数据库迁移到阿里云 POLARDB 的步骤及注意事项。

## 前提条件 {#section_zj4_fxs_lgb .section}

-   迁移的源数据库实例支持公网连接。
-   已创建支持MySQL 5.6 版本的AWS Aurora 实例 。
-   已经[创建阿里云POLARDB集群](../../../../../cn.zh-CN/快速入门/创建数据库集群.md#)。
-   已经[创建拥有读写权限的账号](../../../../../cn.zh-CN/快速入门/创建数据库账号.md#)。

## 迁移限制 {#section_z23_3xs_lgb .section}

-   迁移仅支持 MySQL 5.6 的AWS Aurora 。
-   结构迁移不支持 event 的迁移。
-   对于MySQL的浮点型float/double，DTS通过round\(column,precision\)来读取该列的值，若列类型没有明确定义其精度，对于float，精度为38位，对于double类型，精度为308，请先确认DTS的迁移精度是否符合业务预期。
-   如果使用了对象名映射功能，依赖这个对象的其他对象可能迁移失败。
-   当选择增量迁移时，源端的 MySQL 实例需要开启 binlog。
-   当选择增量迁移时，源库的 binlog\_format 要为 row。
-   当选择增量迁移且源 MySQL 为 5.6 版本时，它的 binlog\_row\_image 必须为 full。
-   当选择增量迁移时，增量迁移过程中如果源MySQL实例出现因实例跨机迁移或跨机重建等导致的binlog 文件ID乱序，可能导致增量迁移数据丢失。

## 注意事项 {#section_ewl_lxs_lgb .section}

-   执行迁移任务前建议提前做好数据备份。

-   对于七天之内的异常任务，DTS会尝试自动恢复，可能会导致迁移任务的源端数据库数据覆盖目标实例数据库中写入的业务数据，迁移任务结束后务必将DTS访问目标实例账号的**写权限**用`revoke`命令回收掉。


## 操作步骤 {#section_uwt_sck_5fb .section}

1.  登录Amazon Aurora数据库实例，单击**数据库名称**，在连接页面查看终端节点和端口。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137254_zh-CN.png)

2.  登录[DTS控制台](https://dts.console.aliyun.com/)。
3.  在左侧菜单栏单击**数据迁移**，单击右上角**创建迁移任务**。
4.  （可选）填写任务名称。

    DTS 为每个任务自动生成一个任务名称，任务名称没有唯一性要求。您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。

5.  填写源库和目标库信息，具体参数配置说明如下：

    |库类别|参数|说明|
    |---|--|--|
    |源库信息|实例类型|源库实例类型，这里选择有公网IP的自建数据库。|
    |实例地区|这里选择源库所在的地区，如果您的实例进行了访问限制，请先放开对应地区公网IP段的访问权限后，再配置数据迁移任务。**说明：** 可以单击右侧**获取DTS IP段**查看、复制对应地区的IP段。

|
    |数据库类型|源数据库类型，这里选择MySQL。|
    |主机名或IP地址|Amazon Aurora数据库的终端节点。|
    |端口|Amazon Aurora数据库的端口。|
    |数据库账号|Amazon Aurora数据库具有读写权限的账号。|
    |数据库密码|Amazon Aurora数据库账号对应的密码。|
    |目标库信息|实例类型|这里选择POLARDB。|
    |实例地区|目标实例所在的地区。|
    |POLARDB实例ID|对应地区下的实例ID，这里选择要迁移到的目标实例的ID。|
    |数据库账号|目标实例的拥有读写权限的账号。|
    |数据库密码|目标实例的对应账号的密码。|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137255_zh-CN.png)

6.  填写完成后单击**测试连接**，确定源库和目标库都测试通过。
7.  单击右下角**授权白名单并进入下一步**。
8.  勾选对应的迁移类型，在迁移对象框中将要迁移的数据库选中，单击![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137264_zh-CN.png)移动到已选择对象框。

    **说明：** 为保证迁移数据的一致性，建议选择结构迁移+全量数据迁移+增量数据迁移。

    结构迁移和全量数据迁移暂不收费，增量数据迁移根据链路规格按小时收费。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137261_zh-CN.png)

9.  单击**预检查并启动**，等待预检查结束。

    **说明：** 如果预检查失败，可以根据错误项的提示进行修复，然后重新启动任务。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137262_zh-CN.png)

10. 单击**下一步**，在**购买配置确认**对话框中，勾选**《数据传输（按量付费）服务条款》**并单击**立即购买并启动**。

    如果选择了增量迁移，那么进入增量迁移阶段后，源库的更新写入都会被DTS同步到目标POLARDB集群。迁移任务不会自动结束。如果您只是为了迁移，那么建议在增量迁移无延迟的状态时，源库停写几分钟，等待增量迁移再次进入无延迟状态后，停止掉迁移任务，直接将业务切换到目标POLARDB集群上即可。

11. 单击目标地域，查看迁移状态。迁移完成时，状态为**已完成**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/95598/154780089137263_zh-CN.png)

    至此，完成 AWS Aurora 数据库到阿里云 POLARDB 数据迁移任务配置。


