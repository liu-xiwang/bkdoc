
### 请求地址

/api/c/compapi/v2/bk_paas/get_app_info/



### 请求方法

GET


### 功能描述

获取应用信息，支持批量获取

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
| target_app_code |  string    | 否     | target_app_code 表示应用 ID，多个 ID 以英文分号分隔，target_app_code 为空则表示所有应用 |
| fields          |  string    | 否     | fields 需要返回的字段，多个字段以英文分号分割，如果不传或传入 &#34;&#34;，则返回应用的 bk_app_code、bk_app_name 字段。可选的字段有：bk_app_code（应用ID），bk_app_name（应用名），introduction（应用简介），creator（创建者），developer（开发人员） |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "target_app_code": "bk_test;esb_test",
}
```
### 返回结果示例
```python
{
    "result": true,
    "code": 0,
    "message": "",
    "data": [
        {
            "bk_app_code": "bk_test",
            "bk_app_name": "BKTest"
        },
        {
            "bk_app_code": "esb_test",
            "bk_app_name": "ESBTest"
        }
    ]
}
```