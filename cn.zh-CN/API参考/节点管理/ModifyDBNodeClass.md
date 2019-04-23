# ModifyDBNodeClass {#reference_187265 .reference}

该接口用于变更集群节点规格。

## 请求参数 {#section_nwt_f3f_wvw .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：ModifyDBNodeClass。|
|DBClusterId|String|是|数据库集群ID。|
|ModifyType|String|否|变更类型，取值： -   Upgrade：升级规格；
-   Downgrade：降级规格。

 |
|DBNodeTargetClass|String|否|所有节点变更的目标规格 ，请参见[规格与定价](../../../../cn.zh-CN/产品简介/规格与定价.md#)。|
|ClientToken|String|否|用于保证请求的幂等性，防止重复提交请求。由客户端生成该参数值，保证在不同请求间唯一，大小写敏感、不超过64个ASCII字符。|

## 返回参数 {#section_i2b_pu2_yzg .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|
|DBClusterId|String|数据库集群ID。|
|OrderId|String|订单ID。|

## 请求示例 {#section_2zx_5jd_b5u .section}

```
https://polardb.aliyuncs.com/?Action=ModifyDBNodeClass
&DBClusterId=pc-xxxxxxxxxx
&ModifyType=Upgrade
&DBNodeTargetClass=polar.mysql.x4.large
&<[公共请求参数]>
```

## 返回示例 {#section_793_j2x_mn9 .section}

XML格式

```
<ModifyDBNodeClassResponse>  
	<RequestId>685F028C-4FCD-407D-A559-072D6378C4C3</RequestId>
	<OrderId>2035629xxxxxx</OrderId>
	<DBClusterId>pc-xxxxxxxxxxxxx</DBClusterId>
</ModifyDBNodeClassResponse>
```

JSON格式

```
{
	"RequestId": "685F028C-4FCD-407D-A559-072D6378C4C3",
	"OrderId": 2035629xxxxxx,
	"DBClusterId": "pc-xxxxxxxxxxxxx"
}
```

