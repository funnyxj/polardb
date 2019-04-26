# CreateDBEndpointAddress {#reference_rpf_ndz_xfb .reference}

该接口用于创建集群地址。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：CreateDBEndpointAddress。|
|DBClusterId|String|是|数据库集群ID。|
|DBEndpointId|String|是|集群地址ID。 **说明：** DBEndpointId可通过DescribeDBClusterEndpoints接口查询。

 |
|ConnectionStringPrefix|String|是|连接数据库的地址前辍： -   由小写字母，数字，中划线组成，字母开头；
-   长度不超过30个字符。

 |
|NetType|String|是|新增连接串的网络类型，当前仅支持： -   Public（公网）。

 |

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|

## 请求示例 {#section_khc_1dz_xfb .section}

```
https://polardb.aliyuncs.com/?Action=CreateDBEndpointAddress
&DBClusterId=pc-xxxxxxxxxxxxxx
&DBEndpointId=pc-xxxxxxxxxxxxx
&ConnectionStringPrefix=pc-xxxxxxxxxxxx-pub
&NetType=Public
&<[公共请求参数]>
```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<CreateDBEndpointAddressResponse>  
	<RequestId>D0CEC6AC-7760-409A-A0D5-E6CD8660E9CC</RequestId>
</CreateDBEndpointAddressResponse>
```

JSON格式

```
{
  "RequestId": "D0CEC6AC-7760-409A-A0D5-E6CD8660E9CC"
}
```

