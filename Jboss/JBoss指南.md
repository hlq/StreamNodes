#### 添加用户  
add-user.bat / add-user.sh
    
#### 目录结构
|||
|:---|:---|
|bin|Unix和Windows环境下的启动脚本和启动配置文件|
|bundles|存放OSGI bundle|
|docs/schema|存放XML schema定义文件|
|domain|domain模式的配置文件、部署内容和可写区域等|
|modules|存放各种模块，AS 7是基于模块化的类加载架构|
|standalone|standalone模式的配置文件、部署内容和可写区域等|
|welcome-content|欢迎页面 |

|standalone目录结构 ||
|:---|:---|
|configuration|Standalone模式的配置文件，所有配置信息都存放于此|
|data|服务器写入的持久化信息，比如通过web管理控制台或CLI部署的项目存放在content目录下|
|deployments|用户部署内容存放目录,服务器运行时能自动侦测和部署这些内容|
|lib/ext|利用扩展列表机制安装的library jar的存放位置|
|log|日志文件|
|tmp|临时文件|

|domain目录结构||
|:---|:---|
|configuration|domain 模式的配置文件，所有配置信息都存放于此|
|data/content|主机控制器内部工作区。内部存储部署内容的地方，用户不能操作这个目录注意：域模式不支持扫描文件系统来部署内容|
|lib/ext|利用扩展列表机制安装的library jar的存放位置|
|log|日志文件|
|servers|应用服务器实例可写区域。每一个应用服务器实例都有它自己的子目录，当服务器第一次启动时创建。在每个服务器的目录内包括以下的子目录：|

**data** 服务器写入信息区
**log** 日志文件
**tmp** 临时文件

### domain 模式
> JBoss AS7加入了域domain的概念，目的是使多台JBoss AS服务器的配置可以集中于一点，统一配置、统一部署，从而实现在管理多台JBoss AS服务器时，实现集中管理。
  域的目的是将多台服务器组成一个服务器组，并为一个服务器组内的多台主机提供：
  1.单点集中配置（通过一个域控制器，即Domain Controller，实现组内主机的统一配置）
  2.单点统一部署，通过域控制器将项目一次部署至组内全部主机

#### start/stop

standalone模式运行服务器： 
```
<JBOSS_HOME>\bin\standalone.bat     (Windows)  
<JBOSS_HOME>/bin/standalone.sh      (Unix / Linux)  
```

domain模式运行服务器：
``` 
<JBOSS_HOME>\bin\domain.bat     (Windows)
<JBOSS_HOME>/bin/domain.sh      (Unix / Linux) 
```
关闭
```
<JBOSS_HOME>/bin/jboss-cli.sh --connect --command=:shutdown  //jboss7.1.x 
<JBOSS_HOME>/bin/jboss-cli.bat --connect --command=:shutdown  //jboss7.1.x 
```
