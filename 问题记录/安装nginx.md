# Ubuntu下安装nginx时遇到的问题



1.pcre错误

意思就是重写需要pcre的支持，而我又没有安装pcre，可以加上–without-http_rewrite_module参数屏蔽重写功能！

   解决办法：安装pcre

​     apt-get install libpcre3 libpcre3-dev

 

2.zlib库错误

​     解决：安装zlib

​        \1. apt-get install ruby 安装ruby

​        \2. apt-get install zlib1g 安装zlib

​        \3. apt-get install zlib1g-dev 安装zlib-dev

 

1.进入src目录(下载存放目录)

   cd /usr/local/src/

2.wget下载：http://nginx.org/en/download.html(nginx官网)

   wget http://nginx.org/download/nginx-1.10.3.tar.gz

3.解压

   tar zxvf nginx-1.10.3.tar.gz

4.进入

   cd nginx-1.10.3

5.设置安装目录(这样的话可能会报一些错，这里也是检测是否有问题的关键步骤)

   ./configure --prefix=/usr/local/nginx

6.安装

   make && make install

7.启动

​	/usr/local/nginx/sbin目录下 ./nginx







错误

今天在一台新的服务器上 准备安装nginx 一开始装的扩展什么的都很顺利 但是make的时候出了问题 我确定所有需要的扩展都已经安装好了，出现问题如下：

root@ubuntu:/home/softpackage/nginx-1.9.9# make

cc1: all warnings being treated as errors
 objs/Makefile:458: recipe for target 'objs/src/core/ngx_murmurhash.o' failed
 make[1]: *** [objs/src/core/ngx_murmurhash.o] Error 1
 make[1]: Leaving directory '/nginx-1.9.9'
 Makefile:8: recipe for target 'build' failed

make: *** [build] Error 2

解决办法：
 将对应的makefile文件夹中（如本文中在 /nginx-1.9.9/objs/Makefile） 找到 -Werror 并去掉 在重新make即可

查了-Werror意思之后 发现原来它要求GCC将所有的警告当成错误进行处理 所有导致错误输出 并不能进行下一步