# DescribeDBClusterEndpoints {#reference_gkh_gbz_xfb .reference}

该接口用于查询集群的访问点地址。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DescribeDBClusterEndpoints。|
|DBClusterId|String|是|数据库集群ID。|
|DBEndpointId|String|否|访问点ID。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|
|Items|List<DBEndpoint\>|集群列表。|

## DBEndpoint参数 {#section_ejf_mbz_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|EndpointId|String|访问点ID。|
|EndpointType|String|访问点类型：-   Cluster：集群访问点；
-   Primary：主访问点。

|
|Nodes|String| -   Auto（根据访问点类型，自动选择）；
-   用逗号分隔的节点ID列表（自定义的列表）。

 |
|EndpointConfig|String|Endpoint配置。取值：-   CausalConsistRead（1）：会话强一致；
-   'LoadBalanceStrategy（load）：读请求调度策略。

**说明：** 当前只有一种load，是基于负载的自动调度。

|
|AddressItems|List<Address\>|连接串信息。|

## Address参数 {#section_sdq_qbz_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|ConnectionString|String|连接串。|
|IPAddress|String|IP地址。|
|Port|String|端口。|
|NetType|String|IP 网络类型：-   Public（公网）；
-   Private（私网）。

|
|VPCId|String|专有网络ID。|
|VSwitchId|String|虚拟交换机ID。|

## 请求示例 {#section_snj_c3s_xfb .section}

```
https://polardb.aliyuncs.com/?Action=DescribeDBClusterEndpoints
&DBClusterId=pc-xxxxxxxxxx
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<DescribeDBClusterEndpointsResponse>  
	<Items>
		<EndpointType>Primary</EndpointType>
		<Nodes>Auto</Nodes>
		<DBEndpointId>pc-xxxxxxxxxx</DBEndpointId>
		<NetInfoItems>
			<Port>3306</Port>
			<ConnectionString>pe-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId>vpc-xxxxxxxxxx</VPCId>
			<IPAddress>172.xx.xx.xx</IPAddress>
			<NetType>Private</NetType>
			<VSwitchId>vsw-xxxxxxxxxx</VSwitchId>
		</NetInfoItems>
		<NetInfoItems>
			<Port>3306</Port>
			<ConnectionString>pe-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId></VPCId>
			<IPAddress>120.xx.xx.xx</IPAddress>
			<NetType>Public</NetType>
			<VSwitchId></VSwitchId>
		</NetInfoItems>
		<EndpointConfig>null</EndpointConfig>
	</Items>
	<Items>
		<EndpointType>Cluster</EndpointType>
		<Nodes>Auto</Nodes>
		<DBEndpointId>pe-xxxxxxxxxx</DBEndpointId>
		<NetInfoItems>
			<Port>3306</Port>
			<ConnectionString>pe-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId>vpc-xxxxxxxxxx</VPCId>
			<IPAddress>172.xx.xx.xx</IPAddress>
			<NetType>Private</NetType>
			<VSwitchId>vsw-xxxxxxxxxx</VSwitchId>
		</NetInfoItems>
		<EndpointConfig>{&quot;CausalConsistRead&quot;:0,&quot;WriterAcceptReads&quot;:0,&quot;LoadBalanceStrategy&quot;:&quot;load&quot;}</EndpointConfig>
	</Items>
	<RequestId>3FAC22CA-AADE-40A2-9841-30BDC81C4E9C</RequestId>
</DescribeDBClusterEndpointsResponse>
```

JSON格式

```
{
    "Items":[
        {
            "EndpointType":"Primary",
            "Nodes":"Auto",
            "DBEndpointId":"pc-xxxxxxxxxx",
            "NetInfoItems":[
                {
                    "Port":"3306",
                    "ConnectionString":"pe-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com",
                    "VPCId":"vpc-xxxxxxxxxx",
                    "IPAddress":"172.xx.xx.xx",
                    "NetType":"Private",
                    "VSwitchId":"vsw-xxxxxxxxxx"
                },
                {
                    "Port":"3306",
                    "ConnectionString":"pe-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com",
                    "VPCId":"",
                    "IPAddress":"120.xx.xx.xx",
                    "NetType":"Public",
                    "VSwitchId":""
                }
            ],
            "EndpointConfig":"null"
        },
        {
            "EndpointType":"Cluster",
            "Nodes":"Auto",
            "DBEndpointId":"pe-xxxxxxxxxx",
            "NetInfoItems":[
                {
                    "Port":"3306",
                    "ConnectionString":"pe-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com",
                    "VPCId":"vpc-xxxxxxxxxx",
                    "IPAddress":"172.xx.xx.xx",
                    "NetType":"Private",
                    "VSwitchId":"vsw-xxxxxxxxxx"
                }
            ],
            "EndpointConfig":"{"CausalConsistRead":0,"WriterAcceptReads":0,"LoadBalanceStrategy":"load"}"
        }
    ],
    "RequestId":"3FAC22CA-AADE-40A2-9841-30BDC81C4E9C"
}
```

