# docker_images_cleaner

## 解决Docker for Mac Docker.qcow2 文件强行占用磁盘空间的bug

Docker依赖Linux系统的cgroup实现，在Mac OS中运行时，Docker会启动一个虚拟机中的Linux内核，并硬盘上放一个Docker.qcow2磁盘镜像文件（默认位于/Users/Username/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/）。每次pull一个镜像的时候，这个文件就会变大，而且发现一个愚蠢的bug，即使删除不用的image，这个文件也不会缩小。也就是说，随着docker的使用，这文件会不断膨胀，一点一点地吞掉宝贵的硬盘资源。

无奈网上搜索，发现好多人在抱怨，幸好找到一个法国人写的脚本，基本原理是先用docker save命令保存需要保留的image，然后关闭Docker，删除Docker.qcow文件，之后再启动Docker，Docker会自动重建Docker.qcow，最后再用docker load命令恢复刚才保存的image。


