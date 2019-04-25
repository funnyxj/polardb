# SQL洞察 {#concept_sfj_qqr_2gb .concept}

SQL洞察功能为您的数据库提供安全审计、性能诊断等增值服务。

## 费用说明 {#section_vjr_yct_2gb .section}

-   试用版：免费使用，审计日志仅保存一天，即只能查询一天范围内的数据；不支持数据导出等高级功能；不保障数据完整性。
-   30天或以上： 0.008元/GB/小时。

## 功能说明 {#section_ovx_hcv_lfb .section}

-   **SQL审计日志**：记录对数据库执行的所有操作。通过审计日志记录，您可以对数据库进行故障分析、行为分析、安全审计等操作。
-   **增强搜索**：可以按照数据库、用户、客户端IP、线程ID、执行耗时、执行状态等进行多维度检索，并支持导出和下载搜索结果。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618444834838_zh-CN.png)

-   **SQL分析**：可以对指定时间段的SQL日志进行可视化交互式分析，找出异常SQL，定位性能问题。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618444834839_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618444834840_zh-CN.png)


## 开通SQL洞察 {#section_g4n_21b_mfb .section}

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com/)。
2.  选择地域。
3.  找到目标集群，单击集群名称列的**集群ID**。
4.  在左侧导航栏中选择**日志与审计** \> **SQL洞察**。
5.  单击**立即开通**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445134841_zh-CN.png)

6.  选择SQL审计日志的保存时长，单击**开通服务**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445134842_zh-CN.png)


## 修改SQL日志的存储时长 {#section_sgz_q13_mfb .section}

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com/)。
2.  选择地域。
3.  找到目标集群，单击集群名称列的**集群ID**。
4.  在左侧导航栏中选择**日志与审计** \> **SQL洞察**。
5.  单击右上角**服务设置**。
6.  修改存储时长并单击**确认**。

## 导出SQL记录 {#section_f4b_sb3_mfb .section}

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com/)。
2.  选择地域。
3.  找到目标集群，单击集群名称列的**集群ID**。
4.  在左侧导航栏中选择**日志与审计** \> **SQL洞察**。
5.  单击右侧**导出**。
6.  在弹出的对话框中，勾选需要导出的选项，单击**确认**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445134843_zh-CN.png)

7.  导出完成后，在导出SQL日志记录中，下载已导出的文件并妥善保存。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445134844_zh-CN.png)


## 关闭SQL洞察 {#section_myx_gws_2gb .section}

**说明：** 

SQL洞察功能关闭后，SQL审计日志会被清空。请将[SQL审计日志导出](#)后，再关闭SQL洞察功能。

1.  进入[POLARDB控制台](https://polardb.console.aliyun.com/)。
2.  选择地域。
3.  找到目标集群，单击集群名称列的**集群ID**。
4.  在左侧导航栏中选择**日志与审计** \> **SQL洞察**。
5.  单击**服务设置**。
6.  单击滑块关闭SQL洞察。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445134845_zh-CN.png)


## 查看审计日志的大小和消费明细 {#section_ycb_ldv_ggb .section}

1.  登录[阿里云管理控制台](https://home.console.aliyun.com/new#/)。
2.  在页面右上角，选择**费用** \> **进入费用中心**。
3.  在左侧导航栏中，选择**消费记录** \> **消费明细** 。
4.  选择**云产品**页签。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445335495_zh-CN.png)

5.  选择**流水详单**。
6.  选择**后付费**。
7.  设置查询条件，然后单击**查询**。

    **说明：** 若要查询超过12个月前的记录，请提交工单。

8.  根据POLARDB集群的计费方式，找到**POLARDB-按量付费**或**POLARDB-包年包月**，单击最右侧的**详情**。
9.  单击最右侧的箭头符号，即可查看SQL洞察的审计日志大小以及费用。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/81394/155618445335494_zh-CN.png)


