# 虚拟机与容器

虚拟机在运行时会占用宿主机的资源
虚拟机通常比在裸金属上运行要慢

我在 VMware Workstation Pro 17.6.4 安装 Debian13

什么是 fork 炸弹

```bash
:(){ :|: &};:
```

注意 { : 要分开
实际上是定义了一个 `:` 函数

```bash
:() {
    : | : &
};
:
```

C 语言实现

```c
#include <unistd.h>
int main() {
    while(true) fork();
}
```

由于fork炸弹的操作模式是基于创建新进程 我们使用 `ulimit -u 30` 限制用户
或者在支持 PAM 系统使用 /etc/security/limits.conf
cgroup 也可以来进行资源控制
sudo usermod -aG 组名 用户名
sudo gpasswd -a 用户名 组名
sudo 权限需要进 sudo 组

快照可以用来恢复状态

本地连接到虚拟机(使用NAT) 安装openssh-server 然后 ip a 找到 ip地址即可

与运行整个操作系统副本的虚拟机不同，容器与主机共享 Linux 内核。然而请注意，如果你在 Windows/macOS 上运行 Linux 容器，则需要一个 Linux 虚拟机作为两者之间的中间层

docker run OPTION CONTIANER COMMMAND
docker run -it --rm gcc:12 /bin/bash
-it 是 -i interactive -t allocate tty

docker 支持 包管理器的https
apt install -y ca-certificates

--net=host（也可以写成 --network=host）是一个非常霸道的网络参数。它告诉 Docker：“不要给这个容器创建独立的网络孤岛，直接让它和宿主机（物理机）共用同一个网络命名空间。”
但是会导致他的主机名和宿主机一致
在远程服务器上启动容器，把容器的 SSH (22) 端口映射到服务器的 2222 端口：
Bash

docker run  -p 2222:22 --name my_ruby_server ruby:3.2
在你的本地电脑上，直接跨网连接：
Bash

ssh root@192.168.223.133 -p 2222

wsl 设置在 WSL settings
