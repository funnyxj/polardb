# ModifyDBClusterEndpoint {#reference_189417 .reference}

该接口用于修改集群地址。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：ModifyDBClusterEndpoint。|
|DBClusterId|String|是|数据库集群ID。|
|EndpointId|String|是|集群地址ID。例如pe-xxxxxxxx。|
|ReadWriteMode|String|否|读写模式： -   ReadWrite：可读可写（自动读写分离）；
-   ReadOnly：只读。

 默认为ReadOnly。|
|Nodes|String|否|加入本地址用于处理读请求的节点，用“,”分隔，至少两个。 默认为全部节点。

 |
|AutoAddNewNodes| | |新节点是否自动加入本地址，取值： -   Enable；
-   Disable。

 默认为Disable。|
|EndpointConfig| | |一致性级别，输入格式为`{"ConsistLevel": "<级别>"}`。取值： -   0：最终一致性；
-   1：会话一致性。

 示例：`{"ConsistLevel": "0"}`

 **说明：** 如果参数ReadWriteMode为ReadOnly，本参数只能为0。

 |

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|requestId。|

## 请求示例 {#section_lx1_c25_xfb .section}

```
https://polardb.aliyuncs.com/?Action=ModifyDBClusterEndpoint
&DBClusterId=pc-xxxxxxxxxxxxxxxxxx
&EndpointId=pe-xxxxxxxxxxxxxxxxxx
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<ModifyDBClusterEndpointResponse>  
	<RequestId>CD3FA5F3-FAF3-44CA-AFFF-BAF869666D6B</RequestId>
</ModifyDBClusterEndpointResponse>
```

JSON格式

```
{
	"RequestId": "CD3FA5F3-FAF3-44CA-AFFF-BAF869666D6B"
}
```

