# 服务拨测常见问题排查指引

## 一. 拨测节点初始化失败
> 可能原因：机器未部署拨测采集器、配置文件有误、数据链路不通

### 排查思路
1. 在蓝鲸监控 -> “主机监控”中，检查该台机器的基础性能数据是否展示正常，若基础性能数据没有显示，可判定该机器数据上报链路有问题。
2. 登录采集器所在的机器，查看采集器进程是否已正常工作

    ```bash
    ps aux | grep uptimecheckbeat
    ```
![15456208356084](../media/15456208356084.jpg)
3. 若采集器进程未启动，请到`/usr/local/gse/plugins/bin`目录下，查看是否存在 uptimecheckbeat 这个二进制文件（拨测采集器），如果不存在，请联系技术人员提供 gse 版本号，确认此版本是否已内置拨测采集器。
4. 若步骤 2 没问题，且采集器进程未启动，请到`/usr/local/gse/plugins/etc`目录下，检查 uptimecheckbeat.conf 文件是否存在。若不存在，请到 job 作业平台查看 api 调用的历史任务，是否有执行失败的记录，给出截图；若 uptimecheckbeat.conf 存在，请执行以下命令手动启动采集器，将输出结果截图提供

    ```bash
    cd /usr/local/gse/plugins/bin
    ./uptimecheckbeat -c ../etc/uptimecheckbeat.conf
    ```
5. 若采集器进程正常启动，但前端仍显示拨测节点不可用，请尝试在前端重新部署节点，等待并观察是否有报错提示，提供截图。

6. 检查采集器是否已正确上报心跳信息。切换到采集器日志路径，找到最新的日志文件：
    ```bash
    cd /var/log/gse
    ls -rlt uptime* | tail -n1
    ```
    假定这里最新的日志文件是`uptimecheckbeat.1`，从中过滤是否有心跳上报关键字“Publish”，且上报时间是否是最新时间：

    ```
    tail -n100 uptimecheckbeat.1 | grep -A10 Publish
    ```
    正常情况下，可观察到最新1分内的采集器心跳上报情况：
![15456380115265](../media/15456380115265.jpg)

7. 检查异常节点系统时间是否同步。
8. 若以上检查点均正常，请打包以下文件并提供：
```bash
# 配置文件
/usr/local/gse/plugins/etc/uptimecheckbeat.conf
# 日志文件
/var/log/gse/uptimecheckbeat.*
```

-------
<br />


## 二. 拨测节点正常，保存拨测任务后显示“无数据”

> 可能原因：配置文件错误、采集器加载配置失败、数据上报异常等

### 排查思路

1. 登录到拨测节点所在的机器，切换到`/usr/local/gse/plugins/etc`路径下，使用 grep 命令找出拨测任务配置。若拨测任务的目标地址为“https://www.xxx.xxx.com/ ”，则命令如下：

    ```plain
    cd /usr/local/gse/plugins/etc
    grep -A3 https://www.xxx.xxx.com/ uptimecheckbeat.conf
    ```
    若拨测任务为 TCP/UDP 任务，则此处将目标地址替换为拨测目标 IP 即可。
    正常情况下，可观察到如下过滤结果
  ![20181227221111](../media/20181227221111.png)
    如果此处无结果输出，可判断最新的任务配置没有下发到拨测节点上，请尝试在前端页面重新保存拨测任务后再次观察。若还是无果，可联系技术人员反馈。

2. 若步骤 1 正常，请记录下步骤 1 中 grep 结果中的 task_id（如图中所示，task_id 为 22）。然后切换到采集器日志路径，找到最新的日志文件：
    ```plainplainplainplainplainplain
    cd /var/log/gse
    ls -rlt uptime* | tail -n1
    ```
    假定这里最新的日志文件是`uptimecheckbeat.1`，从中过滤是否有task_id为22的数据上报，且上报时间是否是最新时间：
    ```
    grep -B25 '"task_id": 22' uptimecheckbeat.1
    ```
    正常情况下会得到如图所示的结果：
![20181227221318](../media/20181227221318.png)

    若此处无结果输出，或输出结果中有类似“ERROR“，”Failed”等错误日志，请截图保存并提供给技术支持同学plainplainplainplain

## 三. 新建节点时，无法通过省份、运营商过滤到指定主机

> 可能原因：CMDB 信息同步异常，查询异常等

### 排查思路
1. 检查 CMDB 中保存的节点地理位置信息是否正确，重新在 CMDB 中保存，过一分钟后再次在监控中尝试拉取机器列表
2. 点击监控页面底部“蓝鲸监控运行状态”，检查 CMDB 接口状态是否有异常
![20181227221431](../media/20181227221431.png)

    如有接口状态异常，展示为红色，请点击接口名后的“？”，截图提供给技术人员plainplain
![15457188640688](../media/15457188640688.jpg)

3. 监控 saas 日志，观察是否有查询接口超时等异常。如查询接口超时，请调整 CMDB 查询时限
