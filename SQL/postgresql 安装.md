# postgresql 安装

**Ubuntu 14.04下安装**
- Create the file /etc/apt/sources.list.d/pgdg.list, and add a line for the repository
		
		deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main
- Import the repository signing key, and update the package lists
		
		wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
			sudo apt-key add -
		sudo apt-get update
		apt-get install postgresql

- 设置密码

		sudo -u postgres psql
		postgres=# ALTER USER postgres WITH PASSWORD ‘postgres’;
		postgres=# \q
		
		sudo passwd -d postgres
		sudo -u postgres passwd

- 配置外网
	```bash
	vi /etc/postgresql/9.5/main/postgresql.conf
	          
	listen_addresses = ‘*’ 
	password_encryption = on
	vi /etc/postgresql/9.5/main/pg_hba.conf
	host all all 0.0.0.0/0 md5
	
	```
- 重启
	
	```bash
	/etc/init.d/postgresql restart
	psql -U postgres -h 127.0.0.1
	```

- 卸载

	```bash
	dpkg --list | grep postgresql 
	dpkg --purge <insert packagehere>
	```

