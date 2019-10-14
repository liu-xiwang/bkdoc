
### 请求地址

/api/c/compapi/v2/job/get_cron_list/



### 请求方法

GET


### 功能描述

查询业务下定时作业信息

### 请求参数


#### 通用参数

| 字段 | 类型 | 必选 |  描述 |
|-----------|------------|--------|------------|
| bk_app_code  |  string    | 是 | 应用ID     |
| bk_app_secret|  string    | 是 | 安全密钥(应用 TOKEN)，可以通过 蓝鲸智云开发者中心 -&gt; 点击应用ID -&gt; 基本信息 获取 |
| bk_token     |  string    | 否 | 当前用户登录态，bk_token与bk_username必须一个有效，bk_token可以通过Cookie获取 |
| bk_username  |  string    | 否 | 当前用户用户名，应用免登录态验证白名单中的应用，用此字段指定当前用户 |

#### 接口参数

| 字段                 |  类型      | 必选   |  描述      |
|----------------------|------------|--------|------------|
| bk_biz_id              |  int       | 是     | 业务ID |
| cron_name              |  string    | 否     | 定时作业名称 |
| cron_id                |  int       | 否     | 定时任务ID，如果存在则忽略其他筛选条件，只查询这个指定的作业信息 |
| cron_status            |  int       | 否     | 定时作业状态：1.已启动、2.已暂停 |
| creator                |  string    | 否     | 定时作业创建人帐号 |
| create_time_start      |  string    | 否     | 创建起始时间，YYYY-MM-DD格式 |
| create_time_end        |  string    | 否     | 创建结束时间，YYYY-MM-DD格式 |
| last_modify_user       |  string    | 否     | 作业修改人帐号 |
| last_modify_time_start |  string    | 否     | 最后修改起始时间，YYYY-MM-DD格式 |
| last_modify_time_end   |  string    | 否     | 最后修改结束时间，YYYY-MM-DD格式 |
| start                  |  int       | 否     | 默认0表示从第1条记录开始返回 |
| length                 |  int       | 否     | 返回记录数量，不传此参数默认返回全部 |

### 请求参数示例

```python
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_biz_id": 1,
    "cron_name": "test",
    "cron_id": 1,
    "cron_status": 1,
    "creator": "admin",
    "create_time_start": "2018-03-02",
    "create_time_end": "2018-03-23",
    "last_modify_user": "admin",
    "last_modify_time_start": "2018-03-02",
    "last_modify_time_end": "2018-03-23",
    "start": 0,
    "length": 100
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
            "bk_biz_id": 1,
            "bk_job_id": 100,
            "job_name": "job name",
            "cron_id": 50,
            "cron_name": "test",
            "cron_status": 1,
            "cron_expression": "2 0/5 * * * ?",
            "creator": "admin",
            "create_time": "2018-03-14 15:46:01 +0800",
            "last_modify_user": "admin",
            "last_modify_time": "2018-03-14 15:48:57 +0800"
        }
    ]
}
```

### 返回结果参数说明

| 字段      | 类型      | 描述      |
|-----------|-----------|-----------|
| bk_biz_id       | int       | 业务ID |
| bk_job_id       | int       | 作业模板ID |
| job_name        | string    | 作业名称 |
| cron_id         | int       | 定时作业ID |
| cron_name       | string    | 定时作业名称 |
| cron_status     | string    | 定时作业状态：1.已启动、2.已暂停 |
| cron_expression | string    | 定时任务crontab的定时规则，各自段含义为：秒 分 时 日 月 周 年（可选），如: 0 0/5 * * * ? 表示每5分钟执行一次 |
| creator         | string    | 作业创建人帐号 |
| create_time     | string    | 创建时间，YYYY-MM-DD HH:mm:ss格式 |
| last_modify_user| string    | 作业修改人帐号 |
| last_modify_time| string    | 最后修改时间，YYYY-MM-DD HH:mm:ss格式 |