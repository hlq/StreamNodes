#[nginx安装](https://www.nginx.com/resources/wiki/start/topics/tutorials/install)
## [Ubuntu PPA](http://www.ubuntuupdates.org/ppas)

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

## nginx插入外部配置
```bash
include /etc/nginx/access.log;
```

