  ps -l  列出与本次登录有关的进程信息；
   ps -aux  查询内存中进程信息；
   ps -aux | grep *   查询*进程的详细信息；
   top  查看内存中进程的动态信息；
   kill -9 pid  杀死进程。

sudo killall apt * 杀死全部进程*

查看ip ifconfig -a

1 #rpm -i example.rpm 安装 example.rpm 包；
2 #rpm -iv example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息；
3 #rpm -ivh example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度

sudo dpkg -i  mysql-common_5.7.29-1ubuntu18.0.4_amd64.deb
sudo dpkg -i  libmysqlclient20_5.7.29-1ubuntu18.0.4_amd64.deb
sudo dpkg -i  libmysqlclient-dev_5.7.29-1ubuntu18.0.4_amd64.deb
sudo dpkg -i  libmysqld-dev_5.7.29-1ubuntu18.0.4_amd64.deb

sudo dpkg -i  mysql-community-client_5.7.18-1ubuntu16.10_amd64.deb
sudo dpkg -i  mysql-client_5.7.18-1ubuntu16.10_amd64.deb
sudo dpkg -i  mysql-community-source_5.7.18-1ubuntu16.10_amd64.deb

sudo apt-get  -f  install
