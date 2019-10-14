# 开源配置平台替换社区版部署指南

- 本方案目前仅适用于社区版 `V5.0.` 或者更高版本。
- 替换前请务必手动备份 DB 数据 `MongoDB`。


## 背景
有许多用户是通过蓝鲸社区版开始接触 `bk-cmdb` 项目，随着对蓝鲸社区版使用逐渐深入之后，可能会遇到些功能特性需求或者需要修复的 bug，社区版 CMDB 需要随着蓝鲸智云整体发布流程出包，因此需求或 bugfix 不能快速的体现到社区版本出包上。

为了解决社区版不能 CMDB 不能快速迭代的问题，本文在此提出一种使用开源 CMDB 替换社区版 CMDB 的方案，开源版本 CMDB 从 Issue 提出到 pr 合入主干链路相比社区版本省去了很多耗时的流程，可以做到快速迭代。

## 开源版 CMDB 与社区版 CMDB
开源版本指的是 GitHub 上的 [开源 CMDB 项目](https://github.com/Tencent/bk-cmdb)，社区版指的是伴随 [蓝鲸智云](https://bk.tencent.com/) 产品一起发布出去的社区版。

两者采用的源代码是一致的，比如您在蓝鲸智云 [下载页面](https://bk.tencent.com/download/) 看到的 CMDB 最新版本是 `3.2.2`，它跟 GitHub 上 [release-v3.2.2](https://github.com/Tencent/bk-cmdb/releases/tag/release-v3.2.2) 采用相同代码编译结果。

主要区别在于两者的配置文件，社区版本 CMDB 是蓝鲸社区版的一部分，启动配置文件对接的也是蓝鲸社区版，比如登录对接的蓝鲸统一登录，比如即将推出的细粒度权限版本也会对接蓝鲸权限中心。相对而言，开源版本独立部署，没有蓝鲸统一登录与蓝鲸权限中心等依赖环境，因此二进制包中默认的配置是采用的无登录和内置鉴权方案。


## 替换有哪些好处
- 能够较快体验到最新的功能特性

- 可以及时得到 bugfix
- 掌握替换方法之后，方便基于开源版本进行二次开发

## 升级建议
- 做好备份

- 系统化测试

- 尽量在同一个大版本下进行升级，比如 `3.2.2` 升级到 `3.2.6`， 它们都是源于 `3.2.x` 分支的 release，相对来说没有大的功能变更，主要进行了一些优化及 bugfix 工作。

## 替换步骤
这部分内容需要操作社区版环境，这里先统一社区版部署根目录 `export BKCE=/data/bkce`

## 备份

### 备份 MongoDB

- `mongodump --host 127.0.0.1 --port 27017 --out backup/ --db cmdb`

- 详细请参考 [Back Up and Restore with MongoDB Tools](https://docs.mongodb.com/manual/tutorial/backup-and-restore-tools/)

### 备份 CMDB
如果磁盘空间足够的话，可以直接备份整个社区版。

## 下载编译 CMDB

- 前置条件：可从外网下载 node

如果只是使用 CMDB，建议从 GitHub 下载最新的 release 代码。

由于某些原因，较新版本的 release 里面，暂时没能提供二进制文件，需要自行编译。

```bash
workspace=tmp1
release=v3.2.8

mkdir -p ${workspace}
cd ${workspace}
export GOPATH=$(pwd)
wget https://github.com/Tencent/bk-cmdb/archive/release-${release}.tar.gz
tar -xf release-${release}.tar.gz
mkdir src
mv bk-cmdb-release-${release} src/configcenter
cd src/configcenter/src

GITHASH=${release} GITTAG=ocr-${release} VERSION=${release} make
```

## 生成配置

新版本中可能会进行相应的调整，比如新增一些配置字段，为了保障升级后的程序正常运行，最好基于新版本重新生成一份配置文件。

生成配置时需要用社区版的配置文件对应的配置参数重新生成配置，社区版 CMDB 的配置文件一般在目录 `${BKCE}/cmdb/server/conf/` 中。

```bash
[root@VM_1_210_centos ~]# ll ${BKCE}cmdb/server/conf/
total 44
-rw-r--r-- 1 root root   0 Apr 17 21:00 apiserver.conf
-rw-r--r-- 1 root root 188 Apr 17 21:00 auditcontroller.conf
-rw-r--r-- 1 root root 833 Apr 17 21:00 datacollection.conf
-rw-r--r-- 1 root root 292 Apr 17 21:00 eventserver.conf
-rw-r--r-- 1 root root 126 Apr 17 21:00 host.conf
-rw-r--r-- 1 root root 292 Apr 17 21:00 hostcontroller.conf
-rw-r--r-- 1 root root 473 Apr 17 21:00 migrate.conf
-rw-r--r-- 1 root root 355 Apr 17 21:00 objectcontroller.conf
-rw-r--r-- 1 root root 291 Apr 17 21:00 proc.conf
-rw-r--r-- 1 root root 292 Apr 17 21:00 proccontroller.conf
-rw-r--r-- 1 root root 202 Apr 17 21:00 topo.conf
-rw-r--r-- 1 root root 709 Apr 17 21:00 webserver.conf
[root@VM_1_210_centos ~]#
```

可参考如下生成配置示例进行相应调整：

```bash
cd bin/build/v3.2.8

./init.py --discovery 127.0.0.1:2181 \
--database cmdb \
--redis_ip 127.0.0.1 \
--redis_port 6379 \
--redis_pass 1234 \
--mongo_ip 127.0.0.1 \
--mongo_port 27017 \
--mongo_user cc \
--mongo_pass cc \
--blueking_cmdb_url http://bk.tencent.com \
--blueking_paas_url http://bk.tencent.com \
--listen_port 8083

➜  v3.2.8 tree cmdb_adminserver/configures
cmdb_adminserver/configures
├── apiserver.conf
├── auditcontroller.conf
├── datacollection.conf
├── eventserver.conf
├── host.conf
├── hostcontroller.conf
├── migrate.conf
├── objectcontroller.conf
├── proc.conf
├── proccontroller.conf
├── topo.conf
└── webserver.conf
```

升级前后的配置文件可以通过对比工具进一步查看升级前后配置的区别，保障生成正确的配置文件。一种比较有效的方式时通过 Git 来管理这些配置，通过 pr 的方式查看配置升级前后的区别。


## 停止 CMDB

```bash
supervisorctl -c ${BKCE}/etc/supervisor-cmdb-server.conf stop all
supervisorctl -c ${BKCE}/etc/supervisor-cmdb-server.conf status
```

## 更新二进制和配置文件

### 更新二进制

```bash
#!/bin/bash

cd tmp/src/configcenter/src/bin/build/v3.2.8
files="$(find . -type f -name "cmdb_*")"
for file in $files;do
	cp -f ${file} ${BKCE}/cmdb/server/bin/
done

cp -rf web ${BKCE}/cmdb/
```

### 更新国际化数据

```bash
cp -rf cmdb_adminserver/conf/errors ${BKCE}/cmdb/
cp -rf cmdb_adminserver/conf/language/ ${BKCE}/cmdb/
```

### 更新进程配置

```bash
cp -rf cmdb_adminserver/configures ${BKCE}/cmdb/server/conf
```

## 启动开源版本

```bash
supervisorctl -c ${BKCE}/etc/supervisor-cmdb-server.conf start all
supervisorctl -c ${BKCE}/etc/supervisor-cmdb-server.conf status
```

## migrate

开源版本中有个 `init_db.sh` 脚本用来升级数据库，该脚本其实是想 Adminserver 发起一个请求，由 Adminserver 执行升级操作。做 MongoDB 数据升级时，可以直接执行该脚本，或者直接调用 Adminserver 的 HTTP 接口。

```bash
curl -X POST -H 'Content-Type:application/json' -H 'BK_USER:migrate' -H 'HTTP_BLUEKING_SUPPLIER_ID:0' http://${adminserver}:60004/migrate/v3/migrate/community/0
```

## 验证
执行到这一步，说明升级操作流程基本执行完了，但是这并不意味着升级成功，只有经过您反复验证后的升级才算完成。

## 问题反馈
如果替换过程遇到无法进行下去，可以先到 GitHub CMDB 上搜索相关问题，可能你遇到的问题前人已经踩过坑了。如果仍然找不到答案，可以到蓝鲸 CMDB 开源开发交流 305496802 咨询，或者直接在 GitHub 多。比如现在用的哪个版本 CMDB，开源版本还是社区版本，版本号多少，遇到什么样的问题，期望得到什么样的效果等等，提问时你可以换个角度思考下，问题是否足够清晰，背景信息是否足够。参考 [如何在 GitHub 上正确的提 Issue？](https://www.zhihu.com/question/62887673)

## 更新版本号信息

- `VERSION`

- `projects.yaml`

主要更新各个 CMDB Server 的 version 字段

```yaml
- name: cmdb_coreservice
  module: cmdb
  project_dir: cmdb/server/bin
  alias: cmdb_coreservice
  role: "node"
  language: golang
  version: 3.2.8
  version_type: oc
```

## 常见问题解决
## make 时 git 命令执行报错

现象：

```bash
go build -i -ldflags "-X configcenter/src/common/version.CCVersion= -X configcenter/src/common/version.CCBuildTime=2019-05-09T14:35:01+0800 -X configcenter/src/common/version.CCGitHash= -X configcenter/src/common/version.CCTag=" -o tmp/src/configcenter/src/bin/build/cmdb_proccontroller/cmdb_proccontroller
fatal: not a git repository (or any of the parent directories): .git
fatal: not a git repository (or any of the parent directories): .git
fatal: not a git repository (or any of the parent directories): .git
fatal: not a git repository (or any of the parent directories): .git
```

原因：常规 make 时默认从 Git 版本中提取

解决办法：设置 GITHASH，GITTAG，VERSION 这三个编译环境变量，如 `GITHASH=xxx GITTAG=ocr-v3.2.8 VERSION=v3.2.8 make`


## make 时提示 `generate.py` 语法问题

出错现象如下：

```bash
go build -i -ldflags "-X configcenter/src/common/version.CCVersion=v3.2.8-t -X configcenter/src/common/version.CCBuildTime=2019-05-09T15:00:38+0800 -X configcenter/src/common/version.CCGitHash=xxx-t -X configcenter/src/common/version.CCTag=release-v3.2.8-t" -o tmp/src/configcenter/src/bin/build/v3.2.8-t/cmdb_webserver/cmdb_webserver
File "./generate.py", line 55
print 'generate.py -t <target>  -i <base_image> -o <output>'
                                                           ^
SyntaxError: Missing parentheses in call to 'print'. Did you mean print('generate.py -t <target>  -i <base_image> -o <output>')?
make: *** [default] Error 1
```

问题位置：

```bash
➜  src grep -n generate.py Makefile
13:	@cd $(SCRIPT_DIR) && python ./generate.py -t '$(BIN_PATH)' -i '${IMAGE}' -o '$(BIN_PATH)/docker'
23:	@cd $(SCRIPT_DIR) && python ./generate.py -t '$(BIN_PATH)' -i '${IMAGE}' -o '$(BIN_PATH)/docker'
38:	@cd $(SCRIPT_DIR) && python ./generate.py -t '$(BIN_PATH)' -i '${IMAGE}' -o '$(BIN_PATH)/docker'
```

解决办法：

- 方法一: 切换到 python 2 环境后重新编译: 通过 virtualenv

```bash
mkvirtualenv bk-cmdb -p /usr/local/bin/python2
```

- 方法二: 切换到 python 2 环境后重新编译: 直接修改 python 路径，比如改成 `/usr/local/bin/python2`

- 方法三: 如果不需要生成 dockerfile 文件，可以将上述三行代码删除了，再重新编译
