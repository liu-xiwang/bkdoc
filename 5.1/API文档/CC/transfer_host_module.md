
### 请求地址

/api/c/compapi/v2/cc/transfer_host_module/



### 请求方法

POST


### 功能描述

业务内主机转移模块

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
| bk_supplier_account | string     | 否     | 开发商账号 |
| bk_biz_id     |  int     | 是     | 业务ID |
| bk_host_id    |  array   | 是     | 主机ID |
| bk_module_id  |  array   | 是     | 模块ID |
| is_increment  |  bool    | 是     | 覆盖或者追加,会删除原有关系. true是更新，false是覆盖 |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_supplier_account": "123456789",
    "bk_biz_id": 1,
    "bk_host_id": [
        9,
        10
    ],
    "bk_module_id": [
        10
    ],
    "is_increment": true
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