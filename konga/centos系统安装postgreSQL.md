### Centos系统安装postgreSQL数据库方法
* 详见官网：https://www.postgresql.org/download/linux/redhat/
* 下载安装：
```bash
# 命令行下执行
# Install the repository RPM:
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-aarch64/pgdg-redhat-repo-latest.noarch.rpm

# Disable the built-in PostgreSQL module:
dnf -qy module disable postgresql

# Install PostgreSQL:
dnf install -y postgresql12-server

# Optionally initialize the database and enable automatic start:
# 初始化并启动
/usr/pgsql-12/bin/postgresql-12-setup initdb
systemctl enable postgresql-12
systemctl start postgresql-12
```
* 登录服务：
```bash
su - postgres
psql
```

### Centos系统本地配置kong方法
* 详见官网：https://docs.konghq.com/install/centos/_ga=2.67114521.921204694.1600237437-2010554078.1600237437#
* 在执行：$ kong start [-c /path/to/kong.conf]语句时可能会报错：用户kong "ident"认证失败。<br>
出错原因：本机开启了认证，需要把/var/lib/pgsql/12/data/pg_hba.conf 文件下的 ident改成trust即可。改法如下：<br>
详见：https://blog.csdn.net/sanbingyutuoniao123/article/details/52209653/#commentBox<br>

```
# IPv4 local connections:<br>
#host    all             all             127.0.0.1/32            ident
host    all             all             127.0.0.1/32            trust
```

### 安装kong的图形化界面kong-dashboard
* kong-dashboard不支持kong2.0版本及以上
```bash
# 安装 Kong Dashboard

npm install -g kong-dashboard

# 启动 Kong Dashboard

kong-dashboard start --kong-url http://localhost:8001

# 用自定义端口启动 Kong Dashboard 

# kong-dashboard start \ --kong-url http://localhost:8001 \ --port [port]

# 使用权限认证启动 Kong Dashboard

# kong-dashboard start \ --kong-url http://kong:8001 \ --basic-auth user1=password1 user2=password2

# Kong Dashboard 帮助文档

# kong-dashboard start --help
```

### 安装kong的图形化界面konga
* 详情参见：https://www.mayi888.com/archives/58889<br>
https://blog.csdn.net/fengxiaolu311/article/details/105115980
```bash
git clone https://github.com/pantsel/konga.git
cd konga
npm install --unsafe-perm=true --allow-root

# 启动konga
npm start
# 访问网址：
# http://localhost:1337
```
