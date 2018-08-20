# 创建POLARDB集群初始账号 {#concept_ew4_wmq_tdb .concept}

初始账号只能在集群详情页设置，可用于登录到集群中的任意实例。

初始账号为高权限账号，可满足个性化和精细化的权限管理需求。您可以使用初始账号登录数据库实例，然后使用阿里云的[数据管理DMS](https://help.aliyun.com/knowledge_detail/52065.html)或SQL命令来管理数据库和普通账号。

**说明：** 初始账号创建后无法删除，也无法修改用户名，只能修改密码。

## 操作步骤 {#section_ktr_gqv_tdb .section}

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com)。

2.  在左侧导航栏中单击**集群列表**，找到目标集群。

3.  单击集群的ID，或者单击**操作**列中的**管理**。

4.  在**访问信息**区域，单击**申请账号**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3016/15347330452081_zh-CN.png)

5.  在对话框中输入用户名和密码，单击**确认**。

    用户名不能超过16位，且不能使用某些预留的用户名，如root、admin。


**说明：** 

-   出于安全原因，POLARDB不提供root账号。

-   POLARDB控制台不支持创建普通账号。

-   不能使用初始账号来修改普通账号的密码，如果需要修改普通账号的密码，只能删除普通账号后重新创建。


