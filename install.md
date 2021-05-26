# 简述
为了便于Argo用户更好的体验, 本版简化了安装步骤，但切记按照要求准备配置环境并严格按照文档执行。
# 须知
1. 要格外注意root与Argo用户的切换；
1. Argo不支持系统使用中文语言，需要用英文 LANG=en_US.UTF-8 ，否则会在部署或运行中出现不兼容问题。一些同学的电脑 ssh 的工具系统语言是中文, 例如 MAC 自带的ssh工具是中文时候,连接服务器会把当前连接 session 系统语言修改成中文，导致创建 argo用户异常，做法是登录上服务器后执行 echo $LANG ，如果发现是（zh_CN.UTF-8）中文，则执行 export LANG==en_US.UTF-8  ，再使用  echo $LANG 是不是改为了（en_US.UTF-8 ）英文；
1. 感谢社区小伙伴录制的Argo部署视频，希望对大家的安装有帮助 https://ark.analysys.cn/video-detail.html?id=93

## 安装前服务器检测和配置
1. 服务器最低配置要求. 一台纯净的服务器，系统：centos7.4|6|7 , CPU:4核8线, 支持 avx 指令集, 内存:16g, 系统盘大于200G, 数据盘 data1大于500G ,且不要使用磁盘分区
1. 使用 root 用户操作（在/root目录操作）
1. 下载构建包 `$ wget http://repo.analysysdata.com/upgrade/go.tar.gz`
1. 解压构建包 (解压到当前目录且不要删除 go.tar.gz ) `$ tar zxf go.tar.gz` 
1. 拷贝配置文件模板到默认目录  `$ cp go/files/scatter/sys.conf /tmp` (如果使用其他路径在后续的命令中用-c /path/sys.conf 参数指定)
1. 修改配置参数 (IP:内网IP | PORT:ssh 端口 | USER:root | PASSWD:root 用户的密码) `$ vim /tmp/sys.conf`
1. 进入工作目录 `$ cd go`
1. 测试ssh连接 `$ bin/python3 tools/pre.py -t`  结果为 pass 方可继续操作
1. 修改主机名和hosts `$ bin/python3 tools/pre.py -s` 主机名会被改成ark1 
1. 构建 argo 用户 `$ bin/python3 tools/pre.py -u argo`
1. 切换到 argo 用户 `$ su - argo`
1. 挂载数据盘 /dev/sdb -> /data1 (默认配置如有特殊可修改 /tmp/sys.conf) `$ python3 go/tools/pre.py -md`
1. 检测 `$ python3 go/tools/pre.py -x` 结果均为 pass 方可继续操作 如有疑问, 截图检测结果到社区群，咨询技术小伙伴
1. 环境检测均通过后初始化环境`$ python3 go/tools/pre.py -init`
     
## 开始安装
1. 切换到 argo 用户 `$ su - argo`
1. 下载安装包`$ wget http://repo.analysysdata.com/argo.4.6.tar.gz`  
1. 解压`$ tar zxf argo.4.6.tar.gz`  
1. 复制安装文件 `$ sudo cp argo.4.6/* /opt/soft`
1. 进入安装目录 `$ cd /opt/soft`
1. 安装`$ sudo sh standalone_offline_installer.sh`  为避免安装过程断网导致安装失败, 建议使用screen命令，如遇到问题可先自行查看日志 /tmp/pre.log ，如有疑问可前往 https://ark.analysys.cn/forum/ 在论坛搜索或提问，如对安装文档有疑问、欢迎和我们共建更好用的文档 https://www.analysysdata.com/forum/topic/293
    
## 开始使用
1. 部署完成后，登录平台管理员帐号（admin 111111），根据页面引导，前往相应位置获取并输入 License 以激活系统，随后进入项目管理页面，即可开始创建项目。
1. 在集成SDK、完成埋点后可开始利用方舟产品能力创建分析模型，开启分析之旅。

## 升级
目前对免费版用户我们开放了4.6版本，基于4.6版本后续做了迭代，欢迎使用者自行升级至最新版4.6，升级方法参考首页 upgrade 文档
另，如对官方5.1版本功能感兴趣，可发邮件至jiangzhenxing@analysys.com.cn

在安装使用过程中如有问题，请优先从已有文档中查找答案：
1. 官方产品文档：https://docs.analysys.cn/ark/ 
2. 论坛：https://www.analysysdata.com/forum/
