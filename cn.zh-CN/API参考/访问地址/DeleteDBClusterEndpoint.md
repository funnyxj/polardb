# DeleteDBClusterEndpoint {#reference_189542 .reference}

该接口用于释放自定义集群地址。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：DeleteDBClusterEndpoint。|
|DBClusterId|String|是|数据库集群ID。|
|EndpointId|String|是|自定义集群地址ID。例如pe-xxxxxxxx。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|requestId。|

## 请求示例 {#section_lx1_c25_xfb .section}

```
https://polardb.aliyuncs.com/?Action=DeleteDBClusterEndpoint
&DBClusterId=pc-xxxxxxxxxxxxxxxxxx
&EndpointId=pe-xxxxxxxxxxxxxxxxxx
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<DeleteDBClusterEndpointResponse>  
	<RequestId>CD3FA5F3-FAF3-44CA-AFFF-BAF869666D6B</RequestId>
</DeleteDBClusterEndpointResponse>
```

JSON格式

```
{
	"RequestId": "CD3FA5F3-FAF3-44CA-AFFF-BAF869666D6B"
}
```

