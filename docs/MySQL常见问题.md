#### 1.MySQL server has gone away（读取sql文件太大）

> 修改 max_allowed_packet 参数

````bash
#进入mysql 容器
docker exec -it mysql bash
# 进入 /etc/mysql 目录（如果不是docker 容器的 mysql，也可以找到这个目录，修改 my.cnf 文件）
cd /etc/mysql
#修改 my.cnf 文件(如果未安装vim，需要先安装 vim)
apt-get update
apt-get install vim
#安装复制粘贴工具
vim --version | grep "clipboard"
sudo apt-get install vim-gnome
#修改my.cnf 
vim my.cnf
:reg
#ctrl r *即可粘贴剪切板上的内容
#重启
docker restart mysql
#查看  max_allowed_pactet 是否修改成功
show VARIABLES like '%max_allowed_packet%';

````



````bash
 
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Custom config should go here
!includedir /etc/mysql/conf.d/
max_allowed_packet=200M
````

#### 2.mysql本机无法通过ip访问

````bash
mysql -uroot -proot;
use mysql;
update user set host='%' where user ='root'
#刷新权限（在不重启的情况下生效）
flush privileges;
````

#### 3.修改密码

````bash
update mysql.user set authentication_string=password('123456') where user='root';
flush privileges;
````

#### 4.无密码登录

````bash
vi my.cnf
################
[mysqld]
pid-file=/var/run/mysqld/mysqld.pid
socket=/var/run/mysqld/mysqld.sock
datadir=/var/lib/mysql
secure-file-priv=NULL
symbolic-links=0
max_allowed_packet=200M
skip-grant-tables

##############
````

