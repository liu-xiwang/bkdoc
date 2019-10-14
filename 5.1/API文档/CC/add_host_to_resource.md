
### 请求地址

/api/c/compapi/v2/cc/add_host_to_resource/



### 请求方法

POST


### 功能描述

新增主机到资源池

### 请求参数


#### 通用参数

| 字段 | 类型 | 必选 |  描述 |
|-----------|------------|--------|------------|
| bk_app_code  |  string    | 是 | 应用ID     |
| bk_app_secret|  string    | 是 | 安全密钥(应用 TOKEN)，可以通过 蓝鲸智云开发者中心 -&gt; 点击应用ID -&gt; 基本信息 获取 |
| bk_token     |  string    | 否 | 当前用户登录态，bk_token与bk_username必须一个有效，bk_token可以通过Cookie获取 |
| bk_username  |  string    | 否 | 当前用户用户名，应用免登录态验证白名单中的应用，用此字段指定当前用户 |

#### 接口参数

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| bk_supplier_account |  string     | 否     | 开发商账号 |
| host_info      |  dict    | 是     | 主机信息 |
| bk_biz_id      |  int     | 否     | 业务ID   |

#### host_info

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| bk_host_innerip |  string   | 是     | 主机内网ip |
| import_from     |  string   | 是     | 主机导入来源,以api方式导入为3 |
| bk_cloud_id     |  int      | 是     | 云区域ID |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_supplier_account": "123456789",
    "host_info": {
        "0": {
            "bk_host_innerip": "10.0.0.1",
            "bk_cloud_id": 0,
            "import_from": "3"
        }
    }
}
```

### 返回结果示例

```python

{
    "result": true,
    "code": 0,
    "message": "",
    "data": {}
}
```