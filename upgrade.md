# 系统升级
* 系统升级包含大版本、小版本两种升级
* 目前仅 4.3.1 及以上版本支持升级  
* 大版本升级主要做架构调整，新功能发布。 如 4.3.x 升级到 4.4.X
* 小版本升级主要做 bug 修复，系统参数调整。 如 4.3.4009 升级到 4.3.4012
## 升级前准备
#### 初次升级版本需要创建 Argo 用户
1. 使用 root 操作
1. 下载构建包 `$ wget http://arkinstall.analysys.cn/upgrade/go.tar.gz`
1. 解压构建包 (解压到当前目录且不要删除 go.tar.gz ) `$ tar xzf go.tar.gz` 
1. 拷贝配置文件模板到当前目录  `$ cp go/files/scatter/sys.conf .` 
1. 修改配置参数 (IP:内网IP | PORT:ssh 端口 | USER:ROOT | PASSWD:root 用户的密码) `$ vim sys.conf`
1. 进如工作目录 `$ cd go`
1. 测试 `$ bin/python3 tools/pre.py -t -c ../sys.conf`  结果为 pass 方可继续操作
1. 构建 Argo 用户 `$ bin/python3 tools/pre.py -u argo -c ../sys.conf`
#### 如果使用离线升级需要提前下载相应的升级包
1. 下载大版本升级包 `$ wget http://arkinstall.analysys.cn/upgrade/ma/argoma.n.n.n.tar.gz`
1. 下载小版本升级包 `$ wget http://arkinstall.analysys.cn/upgrade/mi/argomi.n.n.nxxx.tar.gz`
#### 升级操作需要切换到 Argo 用户操作 (`$ su - argo`)
## 升级信息查看
`$ upgrader`    查看远程升级信息  如果离线升级访问 http://ark_install.analysys.cn/version/ 查看升级信息
## 大版本升级
#### 目前一键升服务还在不断优化中，大版本升级前可以先做小版本升级（用于更新到最新相关升级脚本）
#### 在线升级  
`$ upgrader -ma`    自动升级到最新版本 n.n.x000
#### 离线升级  
`$ upgrader -ma -l /tmp/argoma.x.x.x.tar.gz`  升级包绝对路径  
#### 校验
登录 ambrari 管理界面查看服务状态
## 小版本升级
#### 在线升级  
`$ upgrader -mi`     自动升级到最新版本 n.n.nxxx
#### 离线升级  
`$ upgrader -mi -l /tmp/argomi.x.x.x.tar.gz`  升级包绝对路径 
## 升级命令相关参数
1. -pa  + passwd  提示密码错误时 用于指定 ambari 密码  
1. -pm  + passwd 提示密码错误时 用于指定 mysql 密码
1. -d  高级操作需了解升级原理 系统升级过程会记录升级进度 , 如果出错会可以用 debug 高级模式 续上一次升级未成功的步骤 修改进度文件控制升级流程 
1. -f  高级操作需了解升级原理 强制升级 删掉进度文件 重新开始升级
## 一些规则
1. 日志文件 `/tmp/upgrade.log`
## 问题汇总
部分版本大版本升级到 4.3.4 会有 redis 启动失败的情况。
解决办法：
#### 方法一
1. 手动 kill redis 进程 `$ ps -ef | grep redis-server | grep src | awk '{ print $2}' | xargs sudo kill -9`  
1. 登录 ambari 启动 redis   
#### 方法二
1. 升级指定的小版本 `$ upgrader -mi -v 4.3.4001`
