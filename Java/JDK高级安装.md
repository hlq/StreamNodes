# JDK高级安装
```bash
$ sudo apt-get update
$ sudo apt-get upgrade

$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```
安装 Java 8 需要接受许可，如果你想自动安装，那么可以在安装之前运行：
```bash
echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
```
查看 java 安装路径：
```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

查看所有 jdk 安装版本：
```bash
sudo update-java-alternatives -l
```
设置 Java 8 环境变量：
```bash
sudo apt-get install oracle-java8-set-default
```
切换为 Java 7 ：
```bash
sudo update-java-alternatives -s java-7-oracle
```

再切换为 Java 8：
```bash
sudo update-java-alternatives -s java-8-oracle
```

**验证码无法显示：Could not initialize class sun.awt.X1 解决方案**
在catalina.sh里加上一句
> -Djava.awt.headless=true \


修改后内容如下：
```bash
    exec "$_RUNJAVA" $JAVA_OPTS $CATALINA_OPTS \
      -Djava.endorsed.dirs="$JAVA_ENDORSED_DIRS" -classpath "$CLASSPATH" \
      -Dcatalina.base="$CATALINA_BASE" \
      -Dcatalina.home="$CATALINA_HOME" \
      -Djava.io.tmpdir="$CATALINA_TMPDIR" \
      -Djava.awt.headless=true \
```
以tomcat8为例，总共有八处这样的地方，修改好后即可。

解决生成图片问题
```bash
apt-get install libxrender-dev 
apt-get install libxtst-dev
```
