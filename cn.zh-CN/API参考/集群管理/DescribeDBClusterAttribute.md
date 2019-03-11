# DescribeDBClusterAttribute {#reference_p5z_535_xfb .reference}

 该接口用于查看指定数据库集群的详细属性。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DescribeDBClusterAttribute。|
|DBClusterId|String|是|数据库集群ID。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RegionId|String|地域ID。|
|DBClusterId|String|数据库集群ID。|
|DBClusterDescription|String|集群描述。|
|PayType|String|付费类型：-   Postpaid：按量付费；
-   Prepaid：预付费（包年包月）。

|
|Engine|String|集群引擎。|
|DBType|String|数据库类型。|
|DBVersion|String|数据库版本。|
|DBClusterStatus|String|集群状态，详见[集群状态表](cn.zh-CN/API参考/附表/集群状态表.md#)。|
|LockMode|String|锁定模式：-   Unlock：正常；
-   ManualLock：手动触发锁定；
-   LockByExpiration：集群过期自动锁定。

|
|DBClusterNetworkType|String|集群的网络类型。|
|VPCId|String|专有网络ID。|
|VSwitchId|String|虚拟交换机ID。|
|StorageUsed|Long|存储用量，单位：Byte。|
|CreationTime|String|创建时间，格式：YYYY-MM-DD’T’hh:mm:ssZ，如2011-05-30T12:11:4Z。|
|ExpireTime|String|到期时间。**说明：** 按量付费集群无到期时间。

|
|MaintainTime|String|集群的可维护时间：格式：HH:mmZ- HH:mmZ

示例：00:00Z-01:00Z，表示0点到1点可进行例行维护。

|
|SQLSize|String|SQL的存储量，单位：Byte，-1表示没有数据。|
|DBNodes|List<DBNode\>|DBNode组成的集合。|
|Tags|List<Tags\>|Tag组成的集合。|

## DBNodes {#section_lmx_xg5_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|DBNodeId|String|节点ID。|
|ZoneId|String|可用区ID。|
|DBInstanceStatus|String|节点状态。|
|CreationTime|String|创建时间。|
|DBNodeClass|String|节点规格|
|DBNodeRole|String|节点角色：-   Writer
-   Reader

|
|MaxIOPS|Integer|最大IO请求次数，即IOPS。|
|MaxConnections|Integer|最大集群并发连接数。|

## Tags {#section_yth_nv1_1hb .section}

|名称|类型|描述|
|:-|:-|:-|
|Key|String|标签键。|
|Value|String|标签值。|

## 请求示例 {#section_snj_c3s_xfb .section}

```
https://polardb.aliyuncs.com/?Action=DescribeDBClusterInfo
&DBClusterId=pc-xxxxxxxxxxxxxxx
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<DescribeDBClusterInfoResponse>  
     	<DBVersion>5.6</DBVersion>
	<DBNodes>
		<CreationTime>2018-10-11T23:57:55Z</CreationTime>
		<MaxIOPS></MaxIOPS>
		<ReaderDelayTime></ReaderDelayTime>
		<DBNodeRole>Writer</DBNodeRole>
		<MaxConnections></MaxConnections>
		<DBNodeClass>polar.mysql.x2.medium</DBNodeClass>
		<DBNodeStatus>Running</DBNodeStatus>
		<ZoneId>cn-hangzhou-g</ZoneId>
		<DBNodeId>pi-xxxxxxxxxxxxxx</DBNodeId>
	</DBNodes>
	<DBNodes>
		<CreationTime>2018-10-11T23:57:55Z</CreationTime>
		<MaxIOPS></MaxIOPS>
		<ReaderDelayTime></ReaderDelayTime>
		<DBNodeRole>Reader</DBNodeRole>
		<MaxConnections></MaxConnections>
		<DBNodeClass>polar.mysql.x2.medium</DBNodeClass>
		<DBNodeStatus>Running</DBNodeStatus>
		<ZoneId>cn-hangzhou-g</ZoneId>
		<DBNodeId>pi-xxxxxxxxxxxxxxx</DBNodeId>
	</DBNodes>
	<LockMode>Unlock</LockMode>
	<DBClusterDescription>云监控测试</DBClusterDescription>
	<DBClusterNetworkType>VPC</DBClusterNetworkType>
	<StorageUsed>2960130048</StorageUsed>
	<DBClusterId>pc-xxxxxxxxxxxxxxxx</DBClusterId>
	<Engine>POLARDB</Engine>
	<CreationTime>2018-10-11T07:57:55Z</CreationTime>
	<DBClusterStatus>Running</DBClusterStatus>
	<MaintainTime></MaintainTime>
	<VPCId>vpc-xxxxxxxxxxxxx</VPCId>
	<VSwitchId>vsw-xxxxxxxxxxxxxx</VSwitchId>
	<ExpireTime></ExpireTime>
	<RequestId>D0CEC6AC-7760-409A-A0D5-E6CD8660E9CC</RequestId>
	<RegionId>cn-hangzhou</RegionId>
	<DBType>MySQL</DBType>
	<PayType>Postpaid</PayType>
</DescribeDBClusterInfoResponse>
```

JSON格式

```
{
  "DBVersion": "5.6",
  "DBNodes": [
    {
      "CreationTime": "2018-10-11T23:57:55Z",
      "MaxIOPS": "",
      "ReaderDelayTime": "",
      "DBNodeRole": "Writer",
      "MaxConnections": "",
      "DBNodeClass": "polar.mysql.x2.medium",
      "DBNodeStatus": "Running",
      "ZoneId": "cn-hangzhou-g",
      "DBNodeId": "pi-xxxxxxxxxxxxxx"
    },
    {
      "CreationTime": "2018-10-11T23:57:55Z",
      "MaxIOPS": "",
      "ReaderDelayTime": "",
      "DBNodeRole": "Reader",
      "MaxConnections": "",
      "DBNodeClass": "polar.mysql.x2.medium",
      "DBNodeStatus": "Running",
      "ZoneId": "cn-hangzhou-g",
      "DBNodeId": "pi-xxxxxxxxxxxxxx"
    }
  ],
  "LockMode": "Unlock",
  "DBClusterDescription": "云监控测试",
  "IsLatestVersion": true,
  "DBClusterNetworkType": "VPC",
  "StorageUsed": 2960130048,
  "DBClusterId": "pc-xxxxxxxxxxxxxx",
  "Engine": "POLARDB",
  "CreationTime": "2018-10-11T07:57:55Z",
  "DBClusterStatus": "Running",
  "MaintainTime": "",
  "VPCId": "vpc-xxxxxxxxxxxxxx",
  "VSwitchId": "vsw-xxxxxxxxxxxxx",
  "ExpireTime": "",
  "RequestId": "D0CEC6AC-7760-409A-A0D5-E6CD8660E9CC",
  "RegionId": "cn-hangzhou",
  "DBType": "MySQL",
  "PayType": "Postpaid"
}
```

