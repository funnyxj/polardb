# DescribeDBClusters {#reference_edc_zls_xfb .reference}

该接口用于查询集群列表或被RAM授权的集群列表。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DescribeDBClusters。|
|RegionId|String|是|地域ID。|
|DBClusterIds|String|否|如果有多个集群ID，以","分隔。|
|DBClusterDescription|String|否|集群描述，可模糊查询。|
|DBClusterStatus|String|否|集群状态，详见[集群状态表](cn.zh-CN/API参考/附表/集群状态表.md#)。|
|DBType|String|否|数据库类型，取值：MySQL。|
|Tag.n.Key|String|否|标签键，可以根据标签过滤集群列表。各个标签对的数字n必须不同，且必须是从1开始的连续整数。Tag.n.Key对应的值为Tag.n.Vlaue。|
|Tag.n.Vlaue|String|否|标签值，可以根据标签过滤集群列表。各个标签对的数字n必须不同，且必须是从1开始的连续整数。Tag.n.Vlaue是Tag.n.Key的值。|
|PageSize|Integer|否|每页记录数，取值：-   30；
-   50；
-   100；
-   默认值：30。

|
|PageNumber|Integer|否|页码，取值为：大于0且不超过Integer数据类型的最大值，默认值为1。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|
|PageNumber|Integer|页数。|
|TotalRecordCount|Integer|总记录数。|
|PageRecordCount|Integer|总页数。|
|Items|List<DBCluster\>|集群列表。|

## DBCluster参数 {#section_ilc_b3s_xfb .section}

|参数|类型|描述|
|:-|:-|:-|
|DBClusterId|String|数据库集群ID。|
|DBClusterDescription|String|集群描述。|
|PayType|String|付费类型： -   Postpaid：按量付费；
-   Prepaid：预付费（包年包月）。

|
|RegionId|String|地域ID。|
|CreateTime|String|创建时间。|
|ExpireTime|String|到期时间。|
|DBClusterStatus|String|集群状态。|
|Engine|String|集群引擎。|
|DBType|String|数据库类型。|
|DBVersion|String|数据库版本。|
|LockMode|String|集群的锁定状态：-   Unlock：正常；
-   ManualLock：手动触发锁定；
-   LockByExpiration：集群过期自动锁定。

|
|DBClusterNetworkType|String|集群的网络类型。|
|VPCId|String|专有网络ID。|
|DBNodeNumber|Integer|节点数量。|
|DBNodeClass|String|主节点规格。|
|StorageUsed|Long|存储用量。|
|DBNodes|List<DBNode\>|节点列表。|

## DBNode参数 {#section_crc_fns_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|DBNodeId|String|节点ID。|
|DBNodeClass|String|节点规格。|
|DBNodeRole|String|节点角色：-   Writer
-   Reader

|

## 请求示例 {#section_snj_c3s_xfb .section}

```
https://polardb.aliyuncs.com/?Action=DescribeDBClusters
&RegionId=cn-hangzhou
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<DescribeDBClustersResponse>  
	<Items>
		<DBCluster>
			<DBVersion>5.6</DBVersion>
			<LockMode>Unlock</LockMode>
			<DBClusterDescription>按备份集恢复pc-xxxxxxxxxxxxxxx</DBClusterDescription>
			<DBClusterNetworkType>VPC</DBClusterNetworkType>
			<StorageUsed>2934964224</StorageUsed>
			<DBClusterId>pc-xxxxxxxxxxxxxx</DBClusterId>
			<Engine>POLARDB</Engine>
			<VpcId>vpc-xxxxxxxxxxxxxxx</VpcId>
			<DBClusterStatus>Creating</DBClusterStatus>
			<DBNodeClass>polar.mysql.x8.medium</DBNodeClass>
			<RegionId>cn-qingdao</RegionId>
			<CreateTime>2018-07-31T03:23:29Z</CreateTime>
			<DBType>MySQL</DBType>
			<DBNodes>
				<DBNode>
					<DBNodeRole></DBNodeRole>
					<DBNodeClass>polar.mysql.x8.medium</DBNodeClass>
					<DBNodeId>pi-xxxxxxxxxxxxxxxxxx</DBNodeId>
				</DBNode>
				<DBNode>
					<DBNodeRole></DBNodeRole>
					<DBNodeClass>polar.mysql.x8.medium</DBNodeClass>
					<DBNodeId>pi-xxxxxxxxxxxxxxxxxxxx</DBNodeId>
				</DBNode>
			</DBNodes>
			<DBNodeNumber>2</DBNodeNumber>
			<PayType>Postpaid</PayType>
		</DBCluster>
		<DBCluster>
			<DBVersion>5.6</DBVersion>
			<LockMode>Unlock</LockMode>
			<DBClusterDescription>备份测试</DBClusterDescription>
			<DBClusterNetworkType>VPC</DBClusterNetworkType>
			<DBClusterId>pc-xxxxxxxxxxxxxxxxxxxx</DBClusterId>
			<Engine>POLARDB</Engine>
			<VpcId>vpc-xxxxxxxxxxxxxxxx</VpcId>
			<DBClusterStatus>ClassChanging</DBClusterStatus>
			<DBNodeClass>polar.mysql.x2.medium</DBNodeClass>
			<RegionId>cn-qingdao</RegionId>
			<CreateTime>2018-07-30T08:04:34Z</CreateTime>
			<DBType>MySQL</DBType>
			<DBNodes>
				<DBNode>
					<DBNodeRole></DBNodeRole>
					<DBNodeClass>polar.mysql.x2.medium</DBNodeClass>
					<DBNodeId>pi-xxxxxxxxxxxxxxxxxxxxx</DBNodeId>
				</DBNode>
				<DBNode>
					<DBNodeRole></DBNodeRole>
					<DBNodeClass>polar.mysql.x2.medium</DBNodeClass>
					<DBNodeId>pi-xxxxxxxxxxxxxxxxxxx</DBNodeId>
				</DBNode>
			</DBNodes>
			<DBNodeNumber>2</DBNodeNumber>
			<PayType>Postpaid</PayType>
		</DBCluster>
	</Items>
	<TotalRecordCount>2</TotalRecordCount>
	<PageNumber>1</PageNumber>
	<RequestId>654AD92C-989C-4C82-B5F4-CC30B028D0DA</RequestId>
	<PageRecordCount>2</PageRecordCount>
  </DescribeDBClustersResponse>
```

JSON格式

```
{
  "Items": {
    "DBCluster": [
      {
        "DBVersion": "5.6",
        "LockMode": "Unlock",
        "DBClusterDescription": "按备份集恢复pc-xxxxxxxxxxxxxxxxx",
        "DBClusterNetworkType": "VPC",
        "StorageUsed": 2934964224,
        "DBClusterId": "pc-xxxxxxxxxxxxxxxxxx",
        "Engine": "POLARDB",
        "VpcId": "vpc-xxxxxxxxxxxxxxx",
        "DBClusterStatus": "Creating",
        "DBNodeClass": "polar.mysql.x8.medium",
        "RegionId": "cn-qingdao",
        "CreateTime": "2018-07-31T03:23:29Z",
        "DBType": "MySQL",
        "DBNodes": {
          "DBNode": [
            {
              "DBNodeRole": "",
              "DBNodeClass": "polar.mysql.x8.medium",
              "DBNodeId": "pi-xxxxxxxxxxxxxxxxx"
            },
            {
              "DBNodeRole": "",
              "DBNodeClass": "polar.mysql.x8.medium",
              "DBNodeId": "pi-xxxxxxxxxxxxxxxxx"
            }
          ]
        },
        "DBNodeNumber": "2",
        "PayType": "Postpaid"
      },
      {
        "DBVersion": "5.6",
        "LockMode": "Unlock",
        "DBClusterDescription": "备份测试",
        "DBClusterNetworkType": "VPC",
        "DBClusterId": "pc-xxxxxxxxxxxxxxxxxxxx",
        "Engine": "POLARDB",
        "VpcId": "vpc-xxxxxxxxxxxxxxxxxxx",
        "DBClusterStatus": "ClassChanging",
        "DBNodeClass": "polar.mysql.x2.medium",
        "RegionId": "cn-qingdao",
        "CreateTime": "2018-07-30T08:04:34Z",
        "DBType": "MySQL",
        "DBNodes": {
          "DBNode": [
            {
              "DBNodeRole": "",
              "DBNodeClass": "polar.mysql.x2.medium",
              "DBNodeId": "pi-xxxxxxxxxxxxxxxx"
            },
            {
              "DBNodeRole": "",
              "DBNodeClass": "polar.mysql.x2.medium",
              "DBNodeId": "pi-xxxxxxxxxxxxxxxxxxx"
            }
          ]
        },
        "DBNodeNumber": "2",
        "PayType": "Postpaid"
      }
    ]
  },
  "TotalRecordCount": 2,
  "PageNumber": 1,
  "RequestId": "654AD92C-989C-4C82-B5F4-CC30B028D0DA",
  "PageRecordCount": 2
}
```

