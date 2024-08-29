1、到某个目录。

```bash
docker run --restart=always -d \\
-v /home/theblind/mysql/conf/my.cnf:/etc/mysql/my.cnf \\
-v /home/theblind/mysql/logs:/logs \\
-v /home/theblind/mysql/data:/var/lib/mysql \\
-p 3310:3306 \\
--name mysql8 \\
-e MYSQL_ROOT_PASSWORD='123123' \\
--privileged=true --restart unless-stopped mysql:8.0 \\
--character-set-server=utf8mb4
```

**docker 部署mysql8.0 [https://blog.51cto.com/u_15384850/5434502**](https://blog.51cto.com/u_15384850/5434502**)

```bash
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
character-set-server=utf8
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
# Custom config should go here
!includedir /etc/mysql/conf.d/
```