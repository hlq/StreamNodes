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

