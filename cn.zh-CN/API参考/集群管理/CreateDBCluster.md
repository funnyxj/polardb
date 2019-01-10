# CreateDBCluster {#reference_nyk_pd5_xfb .reference}

该接口用于创建POLARDB集群。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：CreateDBCluster。|
|RegionId|String|是|地域ID，长度不超过50个字符，通过函数DescribeRegions查看可用的地域。|
|ZoneId|String|否|可用区ID，通过函数DescribeRegions查看可用的可用区。|
|DBType|String|是|数据库类型，取值：MySQL。|
|DBVersion|String|是|数据库版本号，取值：5.6。|
|DBNodeClass|String|是|节点规格，参见[规格与定价](../../../../../cn.zh-CN/产品简介/规格与定价.md#)。|
|ClusterNetworkType|String|否|当前仅支持VPC，默认VPC。|
|VPCId|String|否|专有网络ID|
|VSwitchId|String|否|虚拟交换机ID|
|DBClusterDescription|String|否|集群描述：-   不能以http://，https开头；
-   长度为2-256个字符。

|
|PayType|String|是|付费类型：-   Postpaid：按量付费；
-   Prepaid：预付费（包年包月）。

 |
|Period|String|否|若付费类型为Prepaid时，该参数为必传参数。指定预付费集群为包年或包月类型。-   Year：包年；
-   Month：包月。

|
|UsedTime|String|否| -   当Period为Month时，UsedTime取值为\[1-9\]。
-   当Period为Year时，UsedTime取值为\[1-3\]。

 |
|ClientToken|String|否|用于保证请求的幂等性。由客户端生成该参数值，保证在不同请求间唯一，大小写敏感、不超过64个 ASCII 字符。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)|
|RequestId|String|RequestId|
|DBClusterId|String|数据库集群ID|
|OrderId|String|订单ID|

## 请求示例 {#section_lx1_c25_xfb .section}

```
https://polardb.aliyuncs.com/?Action=CreateDBCluster
&RegionId=cn-hangzhou
&DBType=MySQL
&DBVersion=5.6
&DBNodeClass=polar.mysql.x2.medium
&SecurityIPList=127.0.0.1
&PayType=Postpaid
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<CreateDBClusterResponse>  
	<RequestId>E056BF3D-1E4F-4E39-B729-8491A74B301B</RequestId>
	<OrderId>202353277190941</OrderId>
	<DBClusterId>pc-xxxxxxxxxxxxx</DBClusterId>
</CreateDBClusterResponse>
```

JSON格式

```
{
  "RequestId": "E056BF3D-1E4F-4E39-B729-8491A74B301B",
  "OrderId": "202353277190941",
  "DBClusterId": "pc-xxxxxxxxxxxxxxx"
}
```

