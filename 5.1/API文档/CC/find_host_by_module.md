
### 请求地址

/api/c/compapi/v2/cc/find_host_by_module/



### 请求方法

POST


### 功能描述

根据模块ID查询主机

### 请求参数


#### 通用参数

| 字段 | 类型 | 必选 |  描述 |
|-----------|------------|--------|------------|
| bk_app_code  |  string    | 是 | 应用ID     |
| bk_app_secret|  string    | 是 | 安全密钥(应用 TOKEN)，可以通过 蓝鲸智云开发者中心 -&gt; 点击应用ID -&gt; 基本信息 获取 |
| bk_token     |  string    | 否 | 当前用户登录态，bk_token与bk_username必须一个有效，bk_token可以通过Cookie获取 |
| bk_username  |  string    | 否 | 当前用户用户名，应用免登录态验证白名单中的应用，用此字段指定当前用户 |

#### 接口参数

| 字段                |  类型      | 必选   |  描述                       |
|---------------------|------------|--------|-----------------------------|
| metadata           | object     | 是     | 请求元数据                      |
| bk_module_ids | int array     | 是     | 模块ID数组 |
| page                | object     | 是     | 分页参数                    |

metadata参数

| 字段                |  类型      | 必选   |  描述                       |
|---------------------|------------|--------|-----------------------------|
| label           | string map     | 是     | 请求中需要携带的信息，例如业务ID |

label参数

| 字段                |  类型      | 必选   |  描述                       |
|---------------------|------------|--------|-----------------------------|
| bk_biz_id           | string      | 是     | 业务ID |


### 请求参数示例

```python
{
    "metadata":{
        "label":{
            "bk_biz_id":"3"
        }
    },
    "bk_module_ids":[
        56
    ],
    "page":{
        "start":0,
        "limit":10
    }
}
```

### 返回结果示例

```python
{
    "result":true,
    "bk_error_code":0,
    "bk_error_msg":"success",
    "data":{
        "count":1,
        "info":[
            {
                "biz":[
                    {
                        "bk_biz_developer":"",
                        "bk_biz_id":2,
                        "bk_biz_maintainer":"admin",
                        "bk_biz_name":"蓝鲸"
                    }
                ],
                "host":{
                    "bk_asset_id":"DKUXHBUH189",
                    "bk_bak_operator":"admin",
                    "bk_cloud_id":[
                        {
                            "id":"0",
                            "bk_obj_id":"plat",
                            "bk_obj_icon":"",
                            "bk_inst_id":0,
                            "bk_obj_name":"",
                            "bk_inst_name":"default area"
                        }
                    ],
                    "bk_comment":"",
                    "bk_cpu":8,
                    "bk_cpu_mhz":2609,
                    "bk_cpu_module":"E5-2620",
                    "bk_disk":300000,
                    "bk_host_id":17,
                    "bk_host_innerip":"192.168.1.1",
                    "bk_host_name":"nginx-1",
                    "bk_host_outerip":"",
                    "bk_isp_name":null,
                    "bk_mac":"",
                    "bk_mem":32000,
                    "bk_os_bit":""
                },
                "module":[
                    {
                        "TopModuleName":"蓝鲸##公共组件##consul",
                        "bk_bak_operator":"",
                        "bk_biz_id":2,
                        "bk_module_id":35,
                        "bk_module_name":"consul",
                        "bk_module_type":"1",
                        "bk_parent_id":8,
                        "bk_set_id":8,
                        "bk_supplier_account":"0",
                        "create_time":"2018-05-16T21:03:22.724+08:00",
                        "default":0,
                        "last_time":"2018-05-16T21:03:22.724+08:00",
                        "operator":""
                    }
                ],
                "set":[
                    {
                        "TopSetName":"蓝鲸##公共组件",
                        "bk_biz_id":2,
                        "bk_capacity":null,
                        "bk_parent_id":3,
                        "bk_service_status":"1",
                        "bk_set_desc":"111",
                        "bk_set_env":"3",
                        "bk_set_id":8,
                        "bk_set_name":"公共组件",
                        "bk_supplier_account":"0",
                        "create_time":"2018-05-16T21:03:22.692+08:00",
                        "default":0,
                        "description":"",
                        "last_time":"2018-05-18T11:50:53.947+08:00"
                    }
                ]
            }
        ]
    }
}
```

### 返回结果参数说明

| 名称  | 类型  | 说明 |
|---|---|---|---|
| result | bool | 请求成功与否。true:请求成功；false请求失败 |
| bk_error_code | int | 错误编码。 0表示success，>0表示失败错误 |
| bk_error_msg | string | 请求失败返回的错误信息 |
| data | object| 请求返回的数据 |

data 字段说明：

| 名称  | 类型  | 说明 |
|---|---|---|---|
| count| int| 记录条数 |
| info| object array | 主机实际数据 |


info 字段说明:

| 名称  | 类型  | 说明 |
|---|---|---|---| 
| biz | object array| 主机所属的业务信息 |
| set| object array | 主机所属的集群信息 |
| module| object array| 主机所属的模块信息 |
| host| object | 主机自身属性|