# 解决：operation not permitted, unlink 'D:\study\dva-boot-admin-master\dva-boot-adm



![报错信息](../../%E5%9B%BE%E7%89%87/%E5%9B%BE%E7%89%87sasd)

我直接比较粗暴的 npm install -force 解决；

 1、卸载node.js重新安装（可以尝试升级或者降级，或者更改安装目录）；

 2、删除对应没有权限的文件夹，然后重新install；

 3、清除缓存npm cache clean --force；

 4、npm install -force；