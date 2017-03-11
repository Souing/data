# Node应用进程管理器pm2的使用

## 主要特性

>* 内建负载均衡（使用Node cluster 集群模块） 
>* 后台运行 
>* 0秒停机重载，我理解大概意思是维护升级的时候不需要停机. 
>* 具有Ubuntu和CentOS 的启动脚本 
>* 停止不稳定的进程（避免无限循环） 
>* 控制台检测 
>* 提供 HTTP API 
>* 远程控制和实时的接口API ( Nodejs 模块,允许和PM2进程管理器交互 )
>
>

## 安装 

>npm install -g pm2

## 用法

> $ npm install pm2 -g     # 命令行安装 pm2 
> $ pm2 start app.js -i 4 后台运行pm2，启动4个app.js     也可以把'max' 参数传递给 start   正确的进程数目依赖于Cpu的核心数目
> $ pm2 start app.js --name my-api # 命名进程
> $ pm2 list               # 显示所有进程状态
> $ pm2 monit              # 监视所有进程
> $ pm2 logs               #  显示所有进程日志
> $ pm2 stop all           # 停止所有进程
> $ pm2 restart all        # 重启所有进程
> $ pm2 reload all         # 0秒停机重载进程 (用于 NETWORKED 进程)
> $ pm2 stop 0             # 停止指定的进程
> $ pm2 restart 0          # 重启指定的进程
> $ pm2 startup            # 产生 init 脚本 保持进程活着
> $ pm2 web                # 运行健壮的 computer API endpoint (http://localhost:9615)
> $ pm2 delete 0           # 杀死指定的进程
> $ pm2 delete all         # 杀死全部进程
> $ pm2 start ./bin/www --watch  	#应用代码发生变化时，pm2会帮你重启服务



## 运行进程的不同方式

> $ pm2 start app.js -i max  # 根据有效CPU数目启动最大进程数目
> $ pm2 start app.js -i 3      # 启动3个进程
> $ pm2 start app.js -x        #用fork模式启动 app.js 而不是使用 cluster
> $ pm2 start app.js -x -- -a 23   # 用fork模式启动 app.js 并且传递参数 (-a 23)
> $ pm2 start app.js --name serverone  # 启动一个进程并把它命名为 serverone
> $ pm2 stop serverone       # 停止 serverone 进程
> $ pm2 start app.json        # 启动进程, 在 app.json里设置选项
> $ pm2 start app.js -i max -- -a 23                   #在--之后给 app.js 传递参数
> $ pm2 start app.js -i max -e err.log -o out.log  # 启动 并 生成一个配置文件
> 你也可以执行用其他语言编写的app  ( fork 模式):
> $ pm2 describe 查看某个进程的信息
> $ pm2 start my-bash-script.sh    -x --interpreter bash
> $ pm2 start my-python-script.py -x --interpreter python

##  config.json

```
{
  "name"             : "node-app",
  "cwd"              : "/srv/node-app/current",
  "args"             : ["--toto=heya coco", "-d", "1"],
  "script"           : "bin/app.js",
  "node_args"        : ["--harmony", " --max-stack-size=102400000"],
  "log_date_format"  : "YYYY-MM-DD HH:mm Z",
  "error_file"       : "/var/log/node-app/node-app.stderr.log",
  "out_file"         : "log/node-app.stdout.log",
  "pid_file"         : "pids/node-geo-api.pid",
  "instances"        : 6, //or 0 => 'max'
  "min_uptime"       : "200s", // 200 seconds, defaults to 1000
  "max_restarts"     : 10, // defaults to 15
  "max_memory_restart": "1M", // 1 megabytes, e.g.: "2G", "10M", "100K", 1024 the default unit is byte.
  "cron_restart"     : "1 0 * * *",
  "watch"            : false,
  "ignore_watch"      : ["[\\/\\\\]\\./", "node_modules"],
  "merge_logs"       : true,
  "exec_interpreter" : "node",
  "exec_mode"        : "fork",
  "autorestart"      : false, // enable/disable automatic restart when an app crashes or exits
  "vizion"           : false, // enable/disable vizion features (versioning control)
  // Default environment variables that will be injected in any environment and at any start
  "env": {
    "NODE_ENV": "production",
    "AWESOME_SERVICE_API_TOKEN": "xxx"
  }
  "env_*" : {
    "SPECIFIC_ENV" : true
  }
}
apps：json结构，apps是一个数组，每一个数组成员就是对应一个pm2中运行的应用
name：应用程序的名称
cwd：应用程序所在的目录
script：应用程序的脚本路径
exec_interpreter：应用程序的脚本类型，这里使用的shell，默认是nodejs
min_uptime：最小运行时间，这里设置的是60s即如果应用程序在60s内退出，pm2会认为程序异常退出，此时触发重启max_restarts设置数量
max_restarts：设置应用程序异常退出重启的次数，默认15次（从0开始计数）
exec_mode：应用程序启动模式，这里设置的是cluster_mode（集群），默认是fork
error_file：自定义应用程序的错误日志文件
out_file：自定义应用程序日志文件
pid_file：自定义应用程序的pid文件
watch：是否启用监控模式，默认是false。如果设置成true，当应用程序变动时，pm2会自动重载。这里也可以设置你要监控的文件。
```

### 注意：

> 0秒停机重载：这项功能允许你重新载入代码而不用失去请求连接。
> 仅能用于web应用 
> 运行于Node 0.11.x版本 
> 运行于 cluster 模式（默认模式） 
> $ pm2 reload all

### 参考：

http://blog.csdn.net/abld99/article/details/53540958

https://segmentfault.com/a/1190000006793571#articleHeader4

http://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/