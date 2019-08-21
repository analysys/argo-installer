## 系统升级
  * 系统升级包含大版本、小版本两种升级
  * 大版本升级主要做架构调整，新功能发布。 如 4.3.1 升级到 4.3.4
  * 小版本升级主要做 bug 修复，系统参数调整。 如 4.3.4009 升级到 4.3.4012
## 升级前准备
1. 低于 4.3.5 版本需要创建 argo 用户
    1. 使用 root 操作
    1. 下载构建包 `$ wget http://arkinstall.analysys.cn/upgrade/go.tar.gz`
    1. 解压构建包 (解压到当前目录且不要删除 go.tar.gz ) `$ tar xzf go.tar.gz` 
    1. 拷贝配置文件模板到当前目录  `$ cp go/files/scatter/sys.conf .` 
    1. 修改配置参数 (IP:内网IP | PORT:ssh 端口 | USER:ROOT | PASSWD:root 用户的密码) `$ vim sys.conf`
    1. 进如工作目录 `$ cd go`
    1. 测试 `$ bin/python3 tools/pre.py -t -c ../sys.conf`  结果为 pass 方可继续操作
    1. 构建 argo 用户 `$ bin/python3 tools/pre.py -u argo -c ../sys.conf`
1. 如果使用离线升级需要提前下载相应的升级包
    1. 下载大版本升级包 `$ wget http://arkinstall.analysys.cn/upgrade/ma/argoma.n.n.n.tar.gz`
    1. 下载小版本升级包 `$ wget http://arkinstall.analysys.cn/upgrade/mi/argomi.n.n.nxxx.tar.gz`
1. 升级操作需要切换到 argo 用户操作 (`$ su - argo`)
## 大版本升级
#### `$ su - argo` 
#### 1. 在线升级  
`$ upgrader -ma`    自动升级到最新版本 n.n.x000
#### 2. 离线升级  
`$ upgrader -ma -l argoma.x.x.x.tar.gz`
## 小版本升级
#### `$ su - argo`
#### 1. 在线升级  
`$ upgrader -mi`     自动升级到最新版本 n.n.nxxx
#### 2. 离线升级  
`$ upgrader -mi -l argomi.x.x.x.tar.gz`  
## 一些规则
1. 日志文件 /tmp/upgrade.log
## 问题汇总
1. 由于版本变动原因 部分环境升级4.3.4后存在 redis 启动失败的情况，可以升级到特定小版本 4.3.4001 解决  `$ upgrader -mi -v 4.3.4001` 

