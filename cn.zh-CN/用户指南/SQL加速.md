# SQL加速 {#concept_c1h_sbf_4fb .concept}

集群中有多个只读实例时，每个查询都只在一个只读实例上执行，没有同时利用多个只读实例的计算能力，对于大表（一千万行以上）的复杂查询，响应时间不够短。因此，POLARDB提供SQL加速功能，针对这种分析型场景提升查询性能。

SQL加速功能提供一个专门的数据库连接地址，您只需将该地址配置到应用中，该连接所发送的每个读请求都会被分发到所有的只读实例，并行计算。只读实例越多，SQL加速的性能越好。

SQL加速的连接地址不会转发请求到主实例，避免对主实例的影响。

## 功能优势 {#section_a5t_cpt_4fb .section}

-   **性能提升**：针对复杂查询（例如TCP-H基准测试中的复杂查询），SQL加速有很好的性能优化效果，性能可提升**8~30倍**。复杂场景包括：

    -   多张大表的关联查询
    -   对无索引列的点查询
    -   Group By指定多个字段
    具体示例请参见[示例](#)。

-   **高兼容性**：兼容绝大部分MySQL查询语法，支持TPC-H、TPC-DS标准。

    **说明：** 对于极少数不兼容的语法，可以通过提交工单申请支持。

-   **扩展命令和函数**：提供以下的扩展命令和函数，具体用法请参见[扩展命令和函数](#)。
    -   INTERSEC和MINUS/EXCEP
    -   CTE \(WITH\)
    -   开窗函数
-   **免费使用**：SQL加速功能不额外收取任何费用。

## 最佳实践 {#section_l5m_qkq_4fb .section}

-   表的主键由多种数据类型组合而成时，如果第一个字段具有很好的区分度，SQL加速的性能会更好。
-   如果使用SQL加速地址，建议您不同时使用读写分离地址，否则两个连接地址会互相影响性能。（SQL加速地址与读写分离地址后续将会合并为一个地址，则不再存在此影响。）

## 注意事项 {#section_sz1_xw5_4fb .section}

-   SQL加速地址从只读实例读取数据，理论上主实例和只读实例之间存在数据延迟，对于要求实例之间零延迟的场景，不建议使用SQL加速地址。
-   对于Double或Float字段，查询结果的最大精度为小数点后4位。

## 前提条件 {#section_u1y_tbf_4fb .section}

-   POLARDB集群中有至少两个只读实例。如需添加只读实例，请参见[增加只读实例](cn.zh-CN/用户指南/集群和实例管理/增加或删除只读实例.md#section_zb3_yhd_lfb)。
-   集群中实例的规格至少为4核32GB。如需调整规格，请参见[变更配置](cn.zh-CN/用户指南/集群和实例管理/变更配置.md)。
-   数据库中的**所有表**不能包含以下数据类型，否则SQL加速功能无法开启。
    -   数值型：[FIXED\[\(M\[,D\]\)\] \[UNSIGNED\]](https://dev.mysql.com/doc/refman/5.6/en/fixed-point-types.html) 、[DOUBLE PRECISION\[\(M,D\)\] \[UNSIGNED\]](https://dev.mysql.com/doc/refman/5.6/en/floating-point-types.html)
    -   日期型：YEAR
-   SQL加速要读取的表的字符集为utf8或utf8mb4，主键为Int、Bigint、Float、Double或Short类型，且要读取的数据不包含视图或以下类型的字段：BINARY、VARBINARY、TINYBLOB、MEDIUMBLOB、BLOB、LONGBLOB、SET、ENUM。

## 使用SQL加速功能 {#section_t5v_lgq_4fb .section}

1.  登录[POLARDB控制台](https://polardb.console.aliyun.com)。
2.  选择集群所在地域。
3.  选择**集群列表**，单击目标集群的ID。
4.  在**访问信息**中，单击**申请集群SQL加速连接地址（VPC）**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24120/154089367114289_zh-CN.png)

5.  在弹出的对话框中，单击**确定**。
6.  设置地址的前缀，并单击**确定**。
7.  在应用中配置该SQL加速的地址即可。

## 释放SQL加速地址 {#section_kk2_4gv_4fb .section}

不需要使用SQL加速功能时，您可以在**集群信息**页面释放SQL加速地址。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24120/154089367114306_zh-CN.png)

## 示例 {#section_np1_xgc_pfb .section}

**背景：**以下表格用于记录工作人员在仓库中的作业产出情况，每小时操作的商品件数。记录会准实时地持续更新，高峰期 TPS 近万，存在上亿条记录。POLARDB集群中使用4个只读实例。

```
Create Table `labor_operate_stat` (
 `id` varchar(60) NOT NULL COMMENT '业务主键',
 `gmt_create` datetime,
 `gmt_modified` datetime,
 `cur_month` int NOT NULL COMMENT '操作月',
 `cur_day` date NOT NULL COMMENT '操作日期(天)',
 `cur_hour` int NOT NULL COMMENT '操作时间(小时)',
 `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
 `biz_type` int NOT NULL COMMENT '业务类型',
 `user_group_id` bigint COMMENT '用户组ID',
 `user_id` bigint NOT NULL COMMENT '用户ID',
 `user_work_number` varchar(60) COMMENT '工号',
 `operate_num` int COMMENT '商品件数',
 `user_account` varchar(60) COMMENT '用户名',
 primary key (id, warehouse_id, cur_month)
);
```

**SQL语句：**对一段时间内工作人员的小时工作量及总工作量进行统计和排序， 使用的 SQL 如下：

```
SELECT
  a.user_id as user_id,
  cur_day,
  cur_hour,
  CONCAT(
    DATE_FORMAT(cur_day, '%Y-%m-%d'),
    " ",
    IF(cur_hour < 10, CONCAT("0", cur_hour), cur_hour),
    ":00:00"
  ) AS stat_time,
  operate_num,
  b.user_num as user_num,
  b.total_operate_num as total_operate_num,
  user_group_id
FROM
  (
    SELECT
      user_id,
      cur_day,
      cur_hour,
      operate_num,
      user_group_id
    FROM
      labor_operate_stat
    WHERE
      warehouse_id = 1111
      AND biz_type = 1111
      AND CONCAT(
        DATE_FORMAT(cur_day, '%Y-%m-%d'),
        " ",
        IF(cur_hour < 10, CONCAT("0", cur_hour), cur_hour),
        ":00:00"
      ) >= 'xxxx'
      AND CONCAT(
        DATE_FORMAT(cur_day, '%Y-%m-%d'),
        " ",
        IF(cur_hour < 10, CONCAT("0", cur_hour), cur_hour),
        ":00:00"
      ) < 'xxxx'
      AND cur_month >= xxxx
  ) a
  JOIN (
    SELECT
      user_id,
      count(user_id) as user_num,
      sum(operate_num) AS total_operate_num
    FROM
      labor_operate_stat
    WHERE
      warehouse_id = 1111
      AND biz_type = 1111
      AND CONCAT(
        DATE_FORMAT(cur_day, '%Y-%m-%d'),
        " ",
        IF(cur_hour < 10, CONCAT("0", cur_hour), cur_hour),
        ":00:00"
      ) >= 'xxxxx xxxxx'
      AND CONCAT(
        DATE_FORMAT(cur_day, '%Y-%m-%d'),
        " ",
        IF(cur_hour < 10, CONCAT("0", cur_hour), cur_hour),
        ":00:00"
      ) < 'xxxxx xxxxx'
      AND cur_month >= xxxx
    GROUP BY
      user_id
    ORDER BY
      total_operate_num DESC
    LIMIT
      0, 20
  ) b ON a.user_id = b.user_id
ORDER BY
  total_operate_num DESC,
  stat_time ASC;
```

**效果：**开启SQL加速前，响应时间为大约10分钟，开启SQL加速后，响应时间只需大约30秒。

## 扩展的命令和函数 {#section_wfh_q4d_pfb .section}

**INTERSEC和MINUS/EXCEP**

除了UNION，SQL加速还扩展支持了Intersect 和 Minus/Except语法。UNION / INTERSECT / EXCEPT / MINUS用于进行集合求并、交、差操作，语法为：

```
query
{ UNION [ ALL ] | INTERSECT | EXCEPT | MINUS }
query
```

参数：

-   `query`： 操作符前后的query输出的列数目和类型都必须完全一致。
-   `UNION [ALL]`：集合求并操作，输出合并后的结果， ALL表示无需去重。
-   `INTERSECT`： 集合求交操作，输出各个query的交集。
-   `EXCEPT`：集合求差操作，返回query的差集结果。
-   `MINUS`: 集合求差操作，同`EXCEPT`。

集合操作的排序：

`UNION`和`EXCEPT` 操作符都是左结合（left-associative），例如：

```
select * from t1
union
select * from t2
except
select * from t3
order by c1;
```

相当于先做完t1 union t2 except t3, 最后才是对前面的结果按照c1进行全局排序

`Intersect`操作符的优先级是高于 `UNION`和`EXCEPT`的, 例如：

```
select * from t1
union
select * from t2
intersect
select * from t3
order by c1;
```

等同于：

```
select * from t1
union
(select * from t2
intersect
select * from t3)
order by c1;
```

**说明：** 集合操作符前的query是不可以带order by语句的，如果要带，需要用括号括起来。

**WITH**

WITH语句用于定义一个或者多个子查询，每个子查询定义一个临时表，类似于视图的定义； 在WITH中定义的临时表可以在当前查询的其他子句中引用；所有的WITH语句定义的临时表，都可以通过SELECT子句中的子查询定义来完成类似的效果，但是对于这些子查询或者临时表被后面的字句多次引用时，WITH语句只需要计算一次临时表结果，然后多次复用，从而达到减少公共表达式计算的次数。

语法：

```
[ WITH with_subquery [, ...] ]
```

with\_suquery的语法：

```
with_subquery_table_name AS ( query )
```

参数：

-   with\_subquery\_table\_name：当前查询中一个唯一的临时表名称
-   query：所有可以支持的SELECT查询

示例：

```
with t as (select x,y from A) select t.y from t order by t.x limit 10
```

**开窗函数**

SQL加速扩展支持了Oracle的开窗函数，大大提升用户分析数据聚合内分组、固定窗口、滑动窗口的分析能力。

语法定义：

```
*function* OVER (
[ PARTITION BY *expr\_list* ]
[ ORDER BY *order\_list* [ *frame\_clause* ] ] )

```

**说明：** 命令中不需要输入中括号，以上中括号只表示可选。

*function*可以是以下排名函数：

-   RANK
-   DENSE\_RANK
-   ROW\_NUMBER

*function*可以是以下聚合函数：

-   AVG
-   COUNT
-   SUM
-   MAX
-   MIN

*expr\_list*为一个或多个表达式或列名，由英文逗号分隔。

*order\_list*为一个或多个表达式或列名，由英文逗号分隔。\(各表达式或字段后可以加上ASC或DESC）

*frame\_clause*为：

```
ROWS BETWEEN
{ UNBOUNDED PRECEDING }
AND
{ UNBOUNDED FOLLOWING | CURRENT ROW }

```

示例：

```

SELECT
d_year,
d_month_seq,
---以下为开窗函数
row_number() OVER(
PARTITION BY d_year
ORDER BY
d_year,
d_month_seq
)
---以上为开窗函数
FROM
tpcds.date_dim
WHERE
d_year > 2090
GROUP BY
d_year,
d_month_seq
ORDER BY
d_year,
d_month_seq;
```

