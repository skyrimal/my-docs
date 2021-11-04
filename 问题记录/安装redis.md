#安装

> 1 tar xzf redis-5.0.8.tar.gz
>
> 2 make all==>遇错后运行yum -y install gcc  ||  yum install gcc-c++
>
> 3 make install





# 解决redis启动时的三个警告

## 情景描述

我在CentOS 7.2中安装完redis，初次启动时出现了三个警告：

> 第一个警告：The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
>  第二个警告：overcommit_memory is set to 0! Background save may fail under low  memory condition. To fix this issue add 'vm.overcommit_memory = 1'  to/etc/sysctl.conf and then reboot or run the command 'sysctl  vm.overcommit_memory=1' for this to take effect.
>  第三个警告：you have Transparent Huge Pages (THP) support enabled in your  kernel. This will create latency and memory usage issues with Redis. To  fix thisissue run the command 'echo never >  /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain thesetting after a reboot. Redis must  be restarted after THP is disabled.

## 解决方案

考虑到redis一般都是部署在服务器上作为服务存在的。所以，本文的解决方案都是持久性配置，不是临时配置。

#### 第一个警告

将`net.core.somaxconn = 1024`添加到`/etc/sysctl.conf`中，然后执行`sysctl -p`生效配置。

#### 第二个警告

将`vm.overcommit_memory = 1`添加到`/etc/sysctl.conf`中，然后执行`sysctl -p`生效配置。

#### 第三个警告

将`echo never > /sys/kernel/mm/transparent_hugepage/enabled`添加到`/etc/rc.local`中，然后执行`source /etc/rc.local`生效配置。

## 总结

redis想的挺周全的，其实解决方案已经写在警告里了，除了第一个警告的解决方案稍作了一些修改外，其他警告的解决方案其实只是对redis的提示做了一个精简。