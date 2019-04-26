# DescribeDBClusterEndpoints {#reference_gkh_gbz_xfb .reference}

该接口用于查询集群地址。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DescribeDBClusterEndpoints。|
|DBClusterId|String|是|数据库集群ID。|
|DBEndpointId|String|否|集群地址ID。例如pe-xxxxxxxx。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|
|Items|List<DBEndpoint\>|集群地址ID。。|

## DBEndpoint参数 {#section_ejf_mbz_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|DBEndpointId|String|集群地址ID。。|
|EndpointType|String|集群地址类型： -   Cluster：集群默认地址；
-   Primary：主地址；
-   Custom：自定义集群地址。

 |
|AutoAddNewNodes|String|新节点是否自动加入本地址： -   Enable；
-   Disable。

 |
|ReadWriteMode|String|读写模式： -   ReadWrite：可读可写（自动读写分离）；
-   ReadOnly：只读。

 |
|Nodes|String| -   Auto（根据集群地址类型，自动选择）；
-   用逗号分隔的节点ID列表（自定义的列表）。

 |
|EndpointConfig|String|一致性级别： -   0：最终一致性；
-   1：会话一致性。

 |
|AddressItems|List<Address\>|连接串信息。|

## Address参数 {#section_sdq_qbz_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|ConnectionString|String|连接串。|
|IPAddress|String|IP地址。|
|Port|String|端口。|
|NetType|String|IP 网络类型： -   Public（公网）；
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
		<AddressItems>
			<Port>3306</Port>
			<ConnectionString>pc-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId>vpc-xxxxxxxxxx</VPCId>
			<IPAddress>172.xx.xx.xx</IPAddress>
			<NetType>Private</NetType>
			<VSwitchId>vsw-xxxxxxxxxx</VSwitchId>
		</AddressItems>
		<Nodes>pi-xxxxxxxxxx</Nodes>
		<DBEndpointId>pe-xxxxxxxxxx</DBEndpointId>
		<EndpointConfig>{}</EndpointConfig>
	</Items>
	<Items>
		<EndpointType>Cluster</EndpointType>
		<AutoAddNewNodes>Enable</AutoAddNewNodes>
		<ReadWriteMode>ReadWrite</ReadWriteMode>
		<AddressItems>
			<Port>3306</Port>
			<ConnectionString>pc-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId>vpc-xxxxxxxxxx</VPCId>
			<IPAddress>172.xx.xx.xx</IPAddress>
			<NetType>Private</NetType>
			<VSwitchId>vsw-xxxxxxxxxx</VSwitchId>
		</AddressItems>
		<Nodes>pi-xxxxxxxxxx,pi-xxxxxxxxxx</Nodes>
		<DBEndpointId>pe-xxxxxxxxxx</DBEndpointId>
		<EndpointConfig>{&quot;ConsistLevel&quot;:&quot;1&quot;,&quot;CausalConsistRead&quot;:&quot;1&quot;,&quot;LoadBalanceStrategy&quot;:&quot;load&quot;}</EndpointConfig>
	</Items>
	<Items>
		<EndpointType>Custom</EndpointType>
		<AutoAddNewNodes>Disable</AutoAddNewNodes>
		<ReadWriteMode>ReadOnly</ReadWriteMode>
		<AddressItems>
			<Port>3306</Port>
			<ConnectionString>pe-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com</ConnectionString>
			<VPCId>vpc-xxxxxxxxxx</VPCId>
			<IPAddress>172.xx.xx.xx</IPAddress>
			<NetType>Private</NetType>
			<VSwitchId>vsw-xxxxxxxxxx</VSwitchId>
		</AddressItems>
		<Nodes>pi-xxxxxxxxxx,pi-xxxxxxxxxx</Nodes>
		<DBEndpointId>pe-xxxxxxxxxx</DBEndpointId>
		<EndpointConfig>{&quot;ConsistLevel&quot;:&quot;0&quot;,&quot;CausalConsistRead&quot;:&quot;0&quot;,&quot;LoadBalanceStrategy&quot;:&quot;load&quot;}</EndpointConfig>
	</Items>
	<RequestId>ABA96273-606B-4616-9394-336A06312713</RequestId>
</DescribeDBClusterEndpointsResponse>
```

JSON格式

```
{
	"Items": [
		{
			"EndpointType": "Primary",
			"AddressItems": [
				{
					"Port": "3306",
					"ConnectionString": "pc-xxxxxxxxxx.w.polardb.cn-qd-pldb1.rds.aliyuncs.com",
					"VPCId": "vpc-xxxxxxxxxx",
					"IPAddress": "172.xx.xx.xx",
					"NetType": "Private",
					"VSwitchId": "vsw-xxxxxxxxxx"
				}
			],
			"Nodes": "pi-xxxxxxxxxx",
			"DBEndpointId": "pe-xxxxxxxxxx",
			"EndpointConfig": "{}"
		},
		{
			"EndpointType": "Cluster",
			"AutoAddNewNodes": "Enable",
			"ReadWriteMode": "ReadWrite",
			"AddressItems": [
				{
					"Port": "3306",
					"ConnectionString": "pc-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com",
					"VPCId": "vpc-xxxxxxxxxx",
					"IPAddress": "172.xx.xx.xx",
					"NetType": "Private",
					"VSwitchId": "vsw-xxxxxxxxxx"
				}
			],
			"Nodes": "pi-xxxxxxxxxx,pi-xxxxxxxxxx",
			"DBEndpointId": "pe-xxxxxxxxxx",
			"EndpointConfig": "{\"ConsistLevel\":\"1\",\"CausalConsistRead\":\"1\",\"LoadBalanceStrategy\":\"load\"}"
		},
		{
			"EndpointType": "Custom",
			"AutoAddNewNodes": "Disable",
			"ReadWriteMode": "ReadOnly",
			"AddressItems": [
				{
					"Port": "3306",
					"ConnectionString": "pe-xxxxxxxxxx.polardb.cn-qd-pldb1.rds.aliyuncs.com",
					"VPCId": "vpc-xxxxxxxxxx",
					"IPAddress": "172.xx.xx.xx",
					"NetType": "Private",
					"VSwitchId": "vsw-xxxxxxxxxx"
				}
			],
			"Nodes": "pi-xxxxxxxxxx,pi-xxxxxxxxxx",
			"DBEndpointId": "pe-xxxxxxxxxx",
			"EndpointConfig": "{\"ConsistLevel\":\"0\",\"CausalConsistRead\":\"0\",\"LoadBalanceStrategy\":\"load\"}"
		}
	],
	"RequestId": "ABA96273-606B-4616-9394-336A06312713"
}
```

