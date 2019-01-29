# 从本地 MySQL 迁移至 POLARDB {#concept_rmt_hrm_4gb .concept}

使用阿里云[数据传输服务（DTS）](https://help.aliyun.com/document_detail/26592.html?spm=a2c4g.11186623.2.8.6df54b84N9BVGE)，您可以实现本地 MySQL到POLARDB集群的数据迁移。通过DTS增量迁移的存储引擎，可以实现在本地应用不停服的情况下，将数据迁移到目标POLARDB集群。

本文介绍使用DTS进行本地 MySQL迁移至POLARDB的任务配置流程。

## 支持同步的SQL操作 {#section_zrx_vb4_4gb .section}

对于**本地 MySQL** \> **POLARDB**数据迁移，DTS支持同步的SQL操作如下：

INSERT、UPDATE、DELETE、REPLACE

ALTER TABLE、ALTER VIEW、ALTER FUNCTION、ALTER PROCEDURE

CREATE DATABASE、CREATE SCHEMA、CREATE INDEX、CREATE TABLE、CREATE PROCEDURE、CREATE

FUNCTION、CREATE TRIGGER、CREATE VIEW、CREATE EVENT

DROP FUNCTION、DROP EVENT、DROP INDEX、DROP PROCEDURE、DROP TABLE、DROP TRIGGER、DROP

VIEW

RENAME TABLE、TRUNCATE TABLE

## 前提条件 {#section_f4w_2f4_4gb .section}

-   已经[创建阿里云POLARDB集群](../../../../../cn.zh-CN/快速入门/创建数据库集群.md#)。
-   已经[创建拥有读写权限的账号](../../../../../cn.zh-CN/快速入门/创建数据库账号.md#)。
-   已经开通本地MySQL的远程访问权限。开通命令为grant all privileges on \*.\* to <username\>@'<ipaddress\>' identified by "<password\>";

    **说明：** 

    -   <username\>：本地MySQL数据库的用户名；
    -   <ipaddress\>：允许登录数据库的ip地址，localhost 表示只能本地登录数据库，%表示任何ip地址都能登录数据库；
    -   <password\>：数据库的用户名对应的密码。

## 注意事项 {#section_xbb_nd4_4gb .section}

-   执行迁移任务前建议提前做好数据备份。
-   对于七天之内的异常任务，DTS会尝试自动恢复，可能会导致迁移任务的源端数据库数据覆盖目标实例数据库中写入的业务数据，迁移任务结束后务必将DTS访问目标实例账号的**写权限**用`revoke`命令回收掉。

## 迁移限制 {#section_wrp_xtm_4gb .section}

-   迁移仅支持 MySQL 5.6版本。
-   结构迁移不支持 event 的迁移。
-   对于MySQL的浮点型float/double，DTS通过round\(column,precision\)来读取该列的值，若列类型没有明确定义其精度，对于float，精度为38位，对于double类型，精度为308，请先确认DTS的迁移精度是否符合业务预期。
-   如果使用了对象名映射功能，依赖这个对象的其他对象可能迁移失败。
-   当选择增量迁移时，源端的 MySQL 实例需要开启 binlog。
-   当选择增量迁移时，源库的 binlog\_format 要为 row。
-   当选择增量迁移且源 MySQL 为 5.6 版本时，它的 binlog\_row\_image 必须为 full。
-   当选择增量迁移时，增量迁移过程中如果源MySQL实例出现因实例跨机迁移或跨机重建等导致的binlog 文件ID乱序，可能导致增量迁移数据丢失。

## 迁移权限要求 {#section_gmj_3tm_4gb .section}

当使用 DTS 进行**本地 MySQL** \> **POLARDB**迁移时，不同的迁移类型，对源端和目标端 MySQL 实例的迁移账号权限要求如下：

|迁移类型|结构迁移|全量数据迁移|增量数据迁移|
|----|----|------|------|
|本地 MySQL 实例|select|select| super

 select

 replication slave

 replication client

 |
|目标端 POLARDB 集群|读写权限|读写权限|读写权限|

## 迁移流程 {#section_stx_xg4_4gb .section}

DTS 在进行**本地 MySQL** \> **POLARDB**数据迁移时，为了解决对象间的依赖关系，提高迁移成功率。结构对象及数据的迁移顺序如下：

1.  结构对象：表、视图的迁移。
2.  全量数据迁移。
3.  结构对象：存储过程、函数、触发器、外键的迁移。
4.  增量数据迁移。

**说明：** 如果没有选择增量数据迁移，那么当全量数据迁移完成后，任务列表中的迁移进度为：结构迁移 100%，全量迁移 100%，迁移状态为**迁移中**。此时迁移任务正在进行步骤3中的对象的迁移。请勿手动结束任务，否则会造成迁移数据丢失。

## 操作步骤 {#section_uwt_sck_5fb .section}

1.  登录[DTS控制台](https://dts.console.aliyun.com/)。
2.  在左侧菜单栏单击**数据迁移**，单击右上角**创建迁移任务**。
3.  （可选）填写任务名称。

    DTS 为每个任务自动生成一个任务名称，任务名称没有唯一性要求。您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。

4.  填写源库和目标库信息，具体参数配置说明如下：

    |库类别|参数|说明|
    |---|--|--|
    |源库信息|实例类型|源数据库实例类型，选择有公网IP的自建数据库。|
    |实例地区|选择源数据库所在的地区。|
    |数据库类型|源数据库类型，选择MySQL。|
    |主机名或IP地址|源数据库的公网IP地址。|
    |端口|源数据库的端口。|
    |数据库账号|源数据库具有读写权限的账号。|
    |数据库密码|源数据库账号对应的密码。|
    |目标库信息|实例类型|选择POLARDB。|
    |实例地区|目标实例所在的地区。|
    |POLARDB实例ID|对应地区下的实例ID，选择要迁移到的目标实例的ID。|
    |数据库账号|目标实例的拥有读写权限的账号。|
    |数据库密码|目标实例的对应账号的密码。|

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120364/154874809138182_zh-CN.png)

5.  填写完成后单击**测试连接**，确定源库和目标库都测试通过。
6.  单击右下角**授权白名单并进入下一步**。
7.  选择**迁移类型**和**迁移对象**。

    -   **迁移类型**：

        -   结构迁移

            DTS 会将迁移对象的结构定义迁移到目标集群。目前 DTS 支持结构迁移的对象有：表、视图、触发器、存储过程、存储函数。

        -   全量数据迁移

            DTS 会将源数据库迁移对象的存量数据全部迁移到目标集群。由于全量迁移并发Insert导致目标实例的表存在碎片，全量迁移完成后目标实例的表空间会比源实例大。

            如果用户只进行全量数据迁移，那么迁移过程中本地 MySQL 实例新增的业务写入不会被同步到目标 RDS for MySQL 实例。

        -   增量数据迁移

            DTS 会将迁移过程中源实例的数据变更同步到目标集群。如果迁移期间进行了 DDL 操作，那么这些结构变更不会迁移到目标集群。

        如果只需要进行全量迁移，建议迁移类型选择：结构迁移＋全量数据迁移。

        如果需要进行不停机迁移，建议迁移类型选择：结构迁移＋全量数据迁移＋增量数据迁移。

        **说明：** 结构迁移和全量数据迁移均不收费，增量迁移需要收费。

    -   **迁移对象**：选择要迁移的对象，单击向右的箭头，将选中的对象添加到右侧。

        迁移对象的选择粒度细化为：库、表、列三个粒度。默认情况下，对象迁移到目标POLARDB集群后，对象名跟源RDS实例一致。如果您迁移的对象在源实例跟目标集群上名称不同，那么需要使用DTS提供的对象名映射功能，详细使用方式可以参考[库表列映射](https://help.aliyun.com/document_detail/26628.html?spm=5176.doc26624.6.125.Mpn8On)。

        **说明：** 

        -   暂时不支持对系统表的迁移。
        -   目标集群中不能有和迁移对象同名的对象。将鼠标移至右侧框中的对象，单击**编辑**，即可修改迁移后的对象名。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120364/154874809138183_zh-CN.png)

8.  单击右下角的**预检查并启动**，成功后单击**下一步**。

    **说明：** 如果预检查失败，可以单击具体检查项后的检查结果，查看具体的失败详情，并根据失败原因修复后，重新进行预检查。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120364/154874809138184_zh-CN.png)

9.  确认DTS购买信息，阅读并勾选**服务条款**，单击**立即购买并启动**。

    如果选择了增量迁移，那么进入增量迁移阶段后，源库的更新写入都会被DTS同步到目标POLARDB集群。迁移任务不会自动结束。如果您只是为了迁移，那么建议在增量迁移无延迟的状态时，源库停写几分钟，等待增量迁移再次进入无延迟状态后，停止掉迁移任务，直接将业务切换到目标POLARDB集群上即可。

10. 单击目标地域，查看迁移状态。迁移完成时，状态为**已完成**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120364/154874809138209_zh-CN.png)

    至此，完成 本地 MySQL 数据库到阿里云 POLARDB 的数据迁移任务。


