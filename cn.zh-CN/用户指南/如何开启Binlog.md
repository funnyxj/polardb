# 如何开启Binlog {#concept_ekv_brx_3hb .concept}

POLARDB是一款完全兼容MySQL的云原生数据库，默认使用了更高级别的物理日志代替Binlog，但为了更好地与MySQL生态融合，POLARDB上线了开启Binlog的功能，开启Binlog后您就可以连接[ElasticSearch](https://help.aliyun.com/document_detail/90777.html)、[AnalyticDB](https://help.aliyun.com/document_detail/98724.html)等数据产品，也可以搭建[POLARDB到RDS](https://help.aliyun.com/document_detail/102184.html)、[RDS到POLARDB](https://help.aliyun.com/document_detail/102185.html)或POLARDB之间的数据实时同步。

## 注意事项 {#section_ggz_dk4_kgb .section}

-   Binlog的空间属于集群存储空间的一部分，需要收取费用，详情请参见[规格与定价](../cn.zh-CN/产品简介/规格与定价.md#)。
-   2019年4月5日之前创建的集群，由于是较早的内核版本，暂时需要[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)开启。
-   Binlog功能默认关闭。
-   Binlog的保留时长受参数**loose\_expire\_logs\_hours**（取值范围0~2376，单位：小时）控制。
-   开启Binlog后会导致写性能下降，读性能不受影响。
-   拉取、订阅或同步Binlog（例如使用DTS等工具）时，建议使用POLARDB的**主地址**，因为直接指向生成Binlog的主节点，具有更好的兼容性和稳定性。您可以在基本信息页面查看主地址，如下图所示。

    ![主地址](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155021/155470369743468_zh-CN.png)


## 开启Binlog {#section_ebg_frx_3hb .section}

1.  登录[POLARDB控制台](https://polardb.console.aliyun.com)。
2.  选择地域。
3.  找到目标集群，单击**集群名称**列的集群ID。
4.  在左侧导航栏选择**配置与管理** \> **参数配置**。
5.  搜索**loose\_polar\_log\_bin**，将当前值修改为ON\_WITH\_GTID，并单击**提交修改**。

    ![开启Binlog](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/155030/155470369743470_zh-CN.png)

6.  在右侧提示重启的对话框中单击**确定**。

    **说明：** 如果报错提示**The operation is not permitted due to lock of cluster**，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)开启。


## 常见问题 {#section_k2h_cly_3hb .section}

-   开启Binlog后可以关闭吗?

    答：将参数**loose\_polar\_log\_bin**修改为OFF并提交即可关闭。

-   开启Binlog对性能影响有多大？

    答：测试数据显示，在64线程并发下，开启Binlog后会有30%~40%的写性能衰减（性能衰减随并发增加而减少，后续将会持续优化），读性能不受影响。对于读多写少的业务场景，整体数据库性能影响较小，例如读写比是4:1的数据库，整体性能影响大约是10%。


