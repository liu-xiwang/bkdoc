
### 请求地址

/api/c/compapi/v2/cc/search_objects/



### 请求方法

POST


### 功能描述

查询模型

### 请求参数


#### 通用参数

| 字段 | 类型 | 必选 |  描述 |
|-----------|------------|--------|------------|
| bk_app_code  |  string    | 是 | 应用ID     |
| bk_app_secret|  string    | 是 | 安全密钥(应用 TOKEN)，可以通过 蓝鲸智云开发者中心 -&gt; 点击应用ID -&gt; 基本信息 获取 |
| bk_token     |  string    | 否 | 当前用户登录态，bk_token与bk_username必须一个有效，bk_token可以通过Cookie获取 |
| bk_username  |  string    | 否 | 当前用户用户名，应用免登录态验证白名单中的应用，用此字段指定当前用户 |

#### 接口参数

| 字段                 |  类型      | 必选   |  描述                                                    |
|----------------------|------------|--------|----------------------------------------------------------|
| creator              | string     | 否     | 本条数据创建者                                           |
| modifier             | string     | 否     | 本条数据的最后修改人员                                   |
| bk_classification_id | string     | 否     | 对象模型的分类ID，只能用英文字母序列命名                 |
| bk_obj_id            | string     | 否     | 对象模型的ID，只能用英文字母序列命名                     |
| bk_obj_name          | string     | 否     | 对象模型的名字，用于展示，可以使用人类可以阅读的任何语言 |
| bk_supplier_account  | string     | 否     | 开发商账号                                               |

### 请求参数示例

```python
{
    "creator": "user",
    "modifier": "user",
    "bk_classification_id": "test",
    "bk_obj_id": "biz",
    "bk_supplier_account":"0"
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
            "bk_classification_id": "bk_organization",
            "create_time": "2018-03-08T11:30:28.005+08:00",
            "creator": "cc_system",
            "description": "",
            "id": 4,
            "bk_ispaused": false,
            "ispre": true,
            "last_time": null,
            "modifier": "",
            "bk_obj_icon": "icon-XXX",
            "bk_obj_id": "XX",
            "bk_obj_name": "XXX",
            "position": "{\"test_obj\":{\"x\":-253,\"y\":137}}",
            "bk_supplier_account": "0"
        }
    ]
}
```

### 返回结果参数说明

#### data

| 字段                 | 类型               | 描述                                                                                           |
|----------------------|--------------------|------------------------------------------------------------------------------------------------|
| id                   | int                | 数据记录的ID                                                                                   |
| creator              | string             | 本条数据创建者                                                                                 |
| modifier             | string             | 本条数据的最后修改人员                                                                         |
| bk_classification_id | string             | 对象模型的分类ID，只能用英文字母序列命名                                                       |
| bk_obj_id            | string             | 对象模型的ID，只能用英文字母序列命名                                                           |
| bk_obj_name          | string             | 对象模型的名字，用于展示                                                                       |
| bk_supplier_account  | string             | 开发商账号                                                                                     |
| bk_ispaused          | bool               | 是否停用, true or false                                                                        |
| ispre                | bool               | 是否预定义, true or false                                                                      |
| bk_obj_icon          | string             | 对象模型的ICON信息，用于前端显示，取值可参考[(modleIcon.json)](/static/esb/api_docs/res/cc/modleIcon.json)|
| position             | json object string | 用于前端展示的坐标                                                                             |