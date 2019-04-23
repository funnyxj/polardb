# DeleteDBNodes {#reference_187257 .reference}

该接口用于删除集群节点。

## 请求参数 {#section_in0_nc4_4b7 .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DeleteDBNodes。|
|DBClusterId|String|是|数据库集群ID。|
|DBNodeId.N|String|是|数据库节点ID。N为从1开始正整数，最大值=当前节点数-2，即必须保留一个主节点和一个只读节点。 **说明：** 当前仅支持一次删除一个节点。

 |
|ClientToken|String|否|用于保证请求的幂等性，防止重复提交请求。由客户端生成该参数值，保证在不同请求间唯一，大小写敏感、不超过64个ASCII字符。|

## 返回参数 {#section_tqw_ftw_di4 .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|
|DBClusterId|String|数据库集群ID。|
|OrderId|String|订单ID。|

## 请求示例 {#section_avs_eu2_bkk .section}

```
https://polardb.aliyuncs.com/?Action=DeleteDBNodes
&DBClusterId=pc-xxxxxxxxxx
&DBNodeId.N=pi-xxxxxxxxxx
&<[公共请求参数]>
```

## 返回示例 {#section_hi6_s6e_bet .section}

XML格式

```
<DeleteDBNodesResponse>  
	<RequestId>6566B2E6-3157-4B57-A693-AFB751E67C25</RequestId>
	<OrderId>2035638xxxxxxx</OrderId>
	<DBClusterId>pc-xxxxxxxxxx</DBClusterId>
</DeleteDBNodesResponse>
```

JSON格式

```
{
	"RequestId": "6566B2E6-3157-4B57-A693-AFB751E67C25",
	"OrderId": 2035638xxxxxxx,
	"DBClusterId": "pc-xxxxxxxxxx"
}
```

