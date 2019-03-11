# UntagResources {#reference_k5d_gsf_1hb .reference}

该接口用于删除集群上的标签。

## 请求参数 {#section_kyn_pgs_xfb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|系统规定参数，取值：UntagResources。|
|RegionId|String|是|地域ID。|
|ResourceType|String|是|集群类型，取值：ALIYUN::POLARDB::CLUSTER。|
|ResourceId.n|String|是|第n个需要删除标签的集群ID。每次最多为50个集群同时删除标签，各个集群ID后面的数字n必须不同，且必须是从1开始的连续整数。例如：pc-bp1xxx.1、pc-bp2xxx.2。|
|TagKey.n|String|否|标签键。每次最多删除20对标签，各个标签对的数字n必须不同，且必须是从1开始的连续整数。|
|All|String|否|是否全部删除，仅当TagKey.n为空时有效。取值范围：-   true
-   false

默认值为：false。|

## 返回参数 {#section_cf4_phs_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|<公共返回参数\>|-|详见[公共返回参数](cn.zh-CN/API参考/ 使用API/公共参数.md#)。|
|RequestId|String|RequestId。|

## 请求示例 {#section_snj_c3s_xfb .section}

```
https://polardb.aliyuncs.com/?Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=pc-bp15751xxxxxxxx
&ResourceType=ALIYUN%3A%3APOLARDB%3A%3ACLUSTER
&&TagKey.1=a
&<[公共请求参数]>

```

## 返回示例 {#section_blw_cls_xfb .section}

XML格式

```
<UntagResourcesResponse>
    <RequestId>2D69A58F-345C-4FDE-88E4-BF5189484043</RequestId>
</UntagResourcesResponse>
```

JSON格式

```
{ 
"RequestId": "2D69A58F-345C-4FDE-88E4-BF5189484043", 
}
```

