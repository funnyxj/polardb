# 从POLARDB实时同步至POLARDB {#concept_sbm_hxy_3hb .concept}

本文介绍如何使用数据传输服务DTS快速创建POLARDB集群和POLARDB集群间的实时同步作业，实现POLARDB到POLARDB的增量数据实时同步。

## 前提条件 {#section_osb_2yy_3hb .section}

-   数据同步的源RDS实例和目标RDS实例已存在，如不存在请先创建POLARDB集群，详情请参考[创建数据库集群](../../../../cn.zh-CN/快速入门/创建数据库集群.md#)。
-   开启源POLARDB集群的Binlog，详情请参考[如何开启Binlog](../../../../cn.zh-CN/用户指南/如何开启Binlog.md#)。

## 支持的数据源 {#section_fsd_wmy_bgb .section}

-   支持同一个阿里云账号下POLARDB集群同POLARDB集群间的实时同步。
-   支持不同阿里云账号下POLARDB集群同POLARDB集群间的实时同步，须使用目标POLARDB所属的阿里云账号配置数据同步任务。

## 支持的同步语法 {#section_qpw_cny_bgb .section}

对于**POLARDB** \> **POLARDB**数据同步，DTS支持同步的SQL操作包括：

-   INSERT、UPDATE、DELETE、REPLACE。
-   ALTER TABLE、ALTER VIEW、ALTER FUNCTION、ALTER PROCEDURE。
-   CREATE DATABASE、CREATE SCHEMA、CREATE INDEX、CREATE TABLE、CREATE PROCEDURE、CREATE FUNCTION、CREATE TRIGGER、CREATE VIEW、CREATE EVENT。
-   DROP FUNCTION、DROP EVENT、DROP INDEX、DROP PROCEDURE、DROP TABLE、DROP TRIGGER、DROP VIEW。
-   RENAME TABLE、TRUNCATE TABLE。

## 同步限制 {#section_tf3_kny_bgb .section}

**数据源**

对于下列DDL语句，如果**new\_tbl\_name**不在指定的同步对象中，则不支持对此 DDL进行复制。

```language-sql
rename table tbl_name to new_tbl_name;
create table tbl_name like new_tbl_name;
create…select…from new_tbl_name;
alter table tbl_name rename to new_tbl_name;
```

**同步架构**

目前数据传输服务提供的实时同步功能支持的同步架构有限，其仅能支持如下架构。如果在配置同步链路过程中，配置不在下述支持范围内的的同步架构，那么预检查中的**复杂拓扑**检查项会检查失败。

1.  **A** \> **B**即一对一单向同步。要求实例B中同步的对象必须为只读，否则会导致同步链路异常，出现数据不一致的情况。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547961834086_zh-CN.png)

2.  **A** \> **B/C/D**即一对多的分发式同步架构。此架构对POLARDB集群个数没有限制，但是要求目标实例中的同步对象必须为只读，否则会导致同步链路异常，出现数据不一致的情况。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547961934087_zh-CN.png)

3.  **B/C/D** \> **A**即多对一的数据汇总架构。为保证同步数据一致性，要求每条同步链路同步的对象不相同。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547961934088_zh-CN.jpg)

4.  A-\>B-\>C 即级联架构。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547961934089_zh-CN.png)

5.  **A** \> **B** \> **A**即集群A和集群B之间的双向同步架构。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547961934090_zh-CN.png)

    **说明：** 

    如果需要使用双向同步，需要在[购买同步链路](#section_pdn_1pj_dhb)时，选择**双向同步**，并在 [数据传输 DTS 控制台](https://dts.console.aliyun.com/) 中根据指引进行配置。


**功能限制**

-   不兼容触发器。

    同步对象为整个库且这个库中包含了会更新同步表内容的触发器，那么可能导致同步数据不一致。例如数据库中存在了表A和表B。表A上有一个触发器，触发器内容为在insert一条数据到表A之后，在表B中插入一条数据。这种情况在同步过程中，如果源实例表A上进行了Insert操作，则会导致表B在源实例跟目标实例数据不一致。

    此类情况需要将目标实例中的对应触发器删除掉，表B的数据由源实例同步过去，详情请参考[触发器存在情况下如何配置同步作业](https://help.aliyun.com/document_detail/26655.html) 。

-   rename table限制。

    rename table操作可能导致同步数据不一致。例如同步对象只包含表A，如果同步过程中源实例将表A重命名为表B，那么表B将不会被同步到目标库。为避免该问题，您可以在数据同步配置时，选择同步表A和表B所在的整个数据库作为同步对象。

-   DDL语法同步方向限制。

    为保障双向同步链路的稳定性，对于同一张表的DDL更新只能在其中一个同步方向进行同步。即一旦某个同步方向配置了DDL同步，则在反方向上不支持DDL同步，只进行DML同步。


## 操作步骤一 购买数据同步实例 {#section_pdn_1pj_dhb .section}

1.  登录[数据传输服务DTS控制台](https://dts.console.aliyun.com/)。
2.  在左侧导航栏，单击**数据同步**。
3.  在页面右上角，单击**创建同步作业**。
4.  在数据传输服务购买页面，选择付费类型为**预付费**或**按量付费**。
    -   预付费：属于预付费，即在新建实例时需要支付费用。适合长期需求，价格比按量付费更实惠，且购买时长越长，折扣越多。
    -   按量付费：属于后付费，即按小时扣费。适合短期需求，用完可立即释放实例，节省费用。
5.  选择数据同步实例的参数配置信息，参数说明如下表所示。

    |参数配置区|参数项|说明|
    |:----|:--|:-|
    |基本配置|功能|选择**数据同步**。|
    |源实例|选择**MySQL**。|
    |源实例地域|选择数据同步链路中源POLARDB集群的地域。**说明：** 订购后不支持更换地域，请谨慎选择。

|
    |目标实例|选择**MySQL**。|
    |目标实例地域|选择数据同步链路中目标POLARDB集群的地域。**说明：** 订购后不支持更换地域，请谨慎选择。

 |
    |同步拓扑|数据同步支持的拓扑类型，根据业务需求进行选择，本案例选择为**单向同步**。|
    |网络类型|数据同步服务使用的网络类型，目前固定为**专线**。|
    |同步链路规格|数据传输为您提供了不同性能的链路规格，以同步的记录数为衡量标准。详情请参考[数据同步规格说明](https://help.aliyun.com/document_detail/26605.html)。|
    |购买量|购买数量|一次性购买数据同步实例的数量，默认为1，如果购买的是按量付费实例，一次最多购买 99 条链路。|

6.  单击**立即购买**，根据提示完成支付流程。

## 操作步骤二 配置同步链路 {#section_g3l_jxy_bgb .section}

1.  登录[数据传输服务DTS控制台](https://dts.console.aliyun.com/)。
2.  在左侧导航栏，单击**数据同步**。
3.  定位至已购买的数据同步实例，单击目标实例**操作**栏中的**配置同步链路**。
4.  配置同步通道的源实例及目标实例信息。

    ![POLARDB数据同步源目库配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155093/155547961943503_zh-CN.png)

    1.  配置任务名称。

        -   DTS为每个任务自动生成一个任务名称，任务名称没有唯一性要求。
        -   您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。
    2.  配置源实例信息。

        |配置项目|配置选项|配置说明|
        |:---|:---|:---|
        |源实例信息|实例类型|选择**通过专线/VPN网关/智能网关接入的自建数据库**。|
        |对端专有网络|此处选择为源POLARDB的VPC ID。**说明：** VPC ID可以到POLARDB控制台的基本信息页面获取。

|
        |数据库类型|固定为**MySQL**。|
        |IP地址|填入源POLARDB主实例的私网IP地址。您可以在ECS中ping该POLARDB集群的主地址（私网）来获取该IP地址。**说明：** 填写IP地址而不是域名，例如应该填写 192.168.xx.xx，而不是 pc-xxxxx.mysql.polardb.rds.aliyuncs.com。

|
        |端口|填入源POLARDB集群的监听端口，默认为**3306**。|
        |数据库账号|填入源POLARDB集群的数据库账号。|
        |数据库密码|填入源POLARDB集群数据库账号对应的密码。|

    3.  配置目标实例信息。

        |配置项目|配置选项|配置说明|
        |:---|:---|:---|
        |目标实例信息|实例类型|选择**通过专线/VPN网关/智能网关接入的自建数据库**。|
        |对端专有网络|此处选择为目标POLARDB集群的VPC ID。具体VPC ID可以到POLARDB控制台的基本信息页面获取。|
        |数据库类型|选择为**MySQL**。|
        |IP地址|填入目标POLARDB主实例的私网IP地址。在ECS中ping该POLARDB集群的**主地址（私网）**可以获取该IP地址。**说明：** 填写IP地址而不是域名，例如应该填写 192.168.xx.xx，而不是 pc-xxxxx.mysql.polardb.rds.aliyuncs.com。

|
        |端口|填入目标POLARDB集群的监听端口，默认为**3306**。|
        |数据库账号|填入目标POLARDB集群的数据库账号。|
        |数据库密码|填入目标POLARDB集群数据库账号对应的密码。|

5.  上述步骤配置完成后，单击页面右下角的**授权白名单并进入下一步**。

    **说明：** 此步骤会将DTS服务器的IP地址自动添加到源POLARDB集群和目标POLARDB集群的白名单中，用于保障DTS服务器能够正常连接目标POLARDB集群。

6.  选择同步对象。

    -   同步对象的选择粒度为库、表。
    -   如果选择整个库作为同步对象，那么该库中所有对象的结构变更操作都会同步至目标库。
    -   如果选择某个表作为同步对象，那么只有这个表的drop/alter/truncate/rename table、create/drop index操作会同步至目标库。
    ![POLARDB数据同步迁移对象配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155093/155547961943506_zh-CN.png)

7.  配置同步初始化的高级配置信息。

    -   此步骤会将源实例中已经存在同步对象的结构及数据在目标实例中初始化，作为后续增量同步数据的基线数据。
    -   同步初始化类型细分为：结构初始化，全量数据初始化。默认情况下，须选择**结构初始化**和**全量数据初始化**。

        **说明：** 全量初始化过程中，并发Insert导致目标实例的表碎片，全量初始化完成后，目标实例的表空间比源集群的表空间大。

    ![POLARDB数据同步初始化配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155093/155547961943508_zh-CN.png)

8.  上述配置完成后，单击页面右下角的**预检查并启动**。

    **说明：** 

    -   在数据同步任务正式启动之前，会先进行预检查。只有预检查通过后，才能成功启动数据同步任务。
    -   如果预检查失败，单击具体检查项后的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155093/155547961943501_zh-CN.png)，查看具体的失败详情。根据失败原因修复后，重新进行预检查。
9.  在预检查对话框中显示**预检查通过**后，关闭预检查对话框，该同步作业的同步任务正式开始。
10. 等待该同步作业的链路初始化完成，直至状态处于**同步中**。

    ![数据同步中](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155093/155547961943517_zh-CN.png)


## 故障排查 {#section_mfx_jxw_fhb .section}

-   如果单击**授权白名单并进入下一步**后，提示**当前请求失败，建议您刷新页面或稍后重试**，请检查POLARDB集群地址，该地址为IP地址，例如192.168.xx.xx，而不是域名地址。在ECS实例中ping该POLARDB集群的**主地址（私网）**可以获取该IP地址。

    ![当前请求失败](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547962041928_zh-CN.png)

-   如果预检查失败，提示**源库binlog开启检查**失败，请参见[如何开启Binlog](../../../../cn.zh-CN/用户指南/如何开启Binlog.md#)。

    ![源库binlog开启检查](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79400/155547962041938_zh-CN.png)


