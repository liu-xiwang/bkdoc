
### 请求地址

/api/c/compapi/v2/cc/search_biz_inst_topo/



### 请求方法

GET


### 功能描述

查询业务实例拓扑

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
| bk_supplier_account |  string  | 否     | 开发商账号 |
| bk_biz_id           |  int     | 是     | 业务id |
| level               |  int     | 否     | 拓扑的层级索引，索引取值从0开始，默认值为2，当设置为 -1 的时候会读取完整的业务实例拓扑 |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_supplier_account": "123456789",
    "bk_biz_id": 1,
}
```

### 返回结果示例

```python
{
    "result": true,
    "code": 0,
    "message": "success",
    "data": [
        {
            "bk_inst_id": 2,
            "bk_inst_name": "blueking",
            "bk_obj_id": "biz",
            "bk_obj_name": "business",
            "child": [
                {
                    "bk_inst_id": 3,
                    "bk_inst_name": "job",
                    "bk_obj_id": "set",
                    "bk_obj_name": "set",
                    "child": [
                        {
                            "bk_inst_id": 5,
                            "bk_inst_name": "job",
                            "bk_obj_id": "module",
                            "bk_obj_name": "module",
                            "child": []
                        }
                    ]
                }
            ]
        }
    ]
}
```

### 返回结果参数说明

#### data

| 字段      | 类型      | 描述      |
|-----------|-----------|-----------|
| bk_inst_id    | int       | 实例ID |
| bk_inst_name  | string    | 实例用于展示的名字 |
| bk_obj_icon   | string    | 模型图标的名字 |
| bk_obj_id     | string    | 模型ID |
| bk_obj_name   | string    | 模型用于展示的名字 |
| child         | array     | 当前节点下的所有实例的集合 |

#### child

| 字段      | 类型      | 描述      |
|-----------|-----------|-----------|
| bk_inst_id    | int       | 实例ID |
| bk_inst_name  | string    | 实例用于展示的名字 |
| bk_obj_icon   | string    | 模型图标的名字 |
| bk_obj_id     | string    | 模型ID |
| bk_obj_name   | string    | 模型用于展示的名字 |
| child         | array     | 当前节点下的所有实例的集合 |