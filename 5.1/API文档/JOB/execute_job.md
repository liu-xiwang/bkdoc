
### 请求地址

/api/c/compapi/v2/job/execute_job/



### 请求方法

POST


### 功能描述

根据作业模板ID启动作业。支持全局变量，如果全局变量的类型为IP，参数值必须包含custom_query_id或ip_list。没有设置的参数将使用作业模版中的默认值。

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
| bk_biz_id   |  int       | 是     | 业务ID |
| bk_job_id   |  int       | 是     | 作业模板ID |
| steps       |  array     | 否     | 指定要执行或自定义参数的步骤数组，要执行全部步骤可不传此参数，见下面steps结构定义 |
| global_vars |  array     | 否     | 全局变量信息，作业包含的全局变量和类型，可以通过接口“查询作业模板详情” (get_job_detail)获取。对于作业中的全局变量值，如果global_vars包含该变量信息，那么会使用api指定的值；否则使用全局变量默认值 |
| bk_callback_url |  string  | 否     | 回调URL，当任务执行完成后，JOB会调用该URL告知任务执行结果。回调协议参考callback_protocol组件文档 |

#### global_vars

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| id               |  int      | 否     | 全局变量id，唯一标识。如果id为空，那么使用name作为唯一标识 |
| name             |  string   | 否     | 全局变量name |
| value            |  string   | 否     | 字符串全局变量值，此字段仅在字符串类型变量有效。 |
| custom_query_id  |  array    | 否     | 配置平台上的自定义查询id列表。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并，此字段仅在IP类型变量有效。 |
| ip_list          |  array    | 否     | IP对象数组。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并，此字段仅在IP类型变量有效。 |

#### steps

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| step_id            |  int     | 是     | 步骤ID |
| script_id          |  int     | 否     | 脚本ID，如果不更改脚本，则不传 |
| script_type        |  int     | 否     | 脚本类型：1(shell脚本)、2(bat脚本)、3(perl脚本)、4(python脚本)、5(powershell脚本)，当script_content有内容时必填 |
| script_content     |  string  | 否     | 脚本内容Base64，如果同时传了script_id和script_content，则script_id优先 |
| script_param       |  string  | 否     | 脚本参数Base64 |
| script_timeout     |  int     | 否     | 脚本超时时间，秒。默认1000，取值范围60-86400 |
| account            |  string  | 否     | 执行帐号名/别名 |
| db_account_id      |  int     | 否     | SQL执行的db帐号ID，SQL步骤必填 |
| file_target_path   |  string  | 否     | 文件传输目标路径 |
| file_source        |  array   | 否     | 源文件对象数组，见下面file_source定义 |
| custom_query_id    |  array   | 否     | 配置平台上的自定义查询id列表。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并 |
| ip_list            |  array   | 否     | IP对象数组。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并 |

#### file_source

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| files           |  array     | 是     | 源文件的绝对路径数组，支持多个文件 |
| account         |  string    | 是     | 执行帐号名/别名 |
| custom_query_id |  array     | 否     | 配置平台上的自定义查询id列表。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并 |
| ip_list         |  array     | 否     | IP对象数组。ip_list与custom_query_id之间任意选一或并存，ip数据会去重合并 |

#### ip_list

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| bk_cloud_id |  int    | 是     | 云区域ID |
| ip          |  string | 是     | IP地址 |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_biz_id": 1,
    "bk_job_id": 100,
    "global_vars": [
        {
            "id": 436,
            "custom_query_id": [
                "3",
                "5",
                "7"
            ],
            "ip_list": [
                {
                    "bk_cloud_id": 0,
                    "ip": "10.0.0.1"
                },
                {
                    "bk_cloud_id": 0,
                    "ip": "10.0.0.2"
                }
            ]
        },
        {
            "id": 437,
            "value": "new String value"
        }
    ],
    "steps": [{
        "script_timeout": 1000,
        "script_param": "aGVsbG8=",
        "ip_list": [
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.1"
            },
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.2"
            }
        ],
        "custom_query_id": [
            "3"
        ],
        "script_id": 1,
        "script_content": "ZWNobyAkMQ==",
        "step_id": 200,
        "account": "root",
        "script_type": 1
    },
    {
        "script_timeout": 1003,
        "ip_list": [
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.1"
            },
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.2"
            }
        ],
        "custom_query_id": [
            "3"
        ],
        "script_id": 1,
        "script_content": "ZWNobyAkMQ==",
        "step_id": 1,
        "db_account_id": 31
    },
    {
        "file_target_path": "/tmp/[FILESRCIP]/",
        "file_source": [
            {
                "files": [
                    "/tmp/REGEX:[a-z]*.txt"
                ],
                "account": "root",
                "ip_list": [
                    {
                        "bk_cloud_id": 0,
                        "ip": "10.0.0.1"
                    },
                    {
                        "bk_cloud_id": 0,
                        "ip": "10.0.0.2"
                    }
                ],
                "custom_query_id": [
                    "3"
                ]
            }
        ],
        "ip_list": [
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.1"
            },
            {
                "bk_cloud_id": 0,
                "ip": "10.0.0.2"
            }
        ],
        "custom_query_id": [
            "3"
        ],
        "step_id": 2,
        "account": "root"
    }]
}
```

### 返回结果示例

```python
{
    "result": true,
    "code": 0,
    "message": "success",
    "data": {
        "job_instance_name": "Test",
        "job_instance_id": 10000
    }
}
```