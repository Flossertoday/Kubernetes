## Reference

http://oudezhinu.site/centos8%e5%8d%87%e7%ba%a7containerd-io/

## Background

CentOS had containerd.io v1.2.2 preinstalled, but the installation of docker-ce requires higher version of containerd.io. 

## Solution

    yum remove docker-ce
    dnf config-manager --add-repo=http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    dnf -y --allowerasing install http://mirrors.aliyun.com/docker-ce/linux/centos/8/x86_64/stable/Packages/containerd.io-1.4.3-3.1.el8.x86_64.rpm
    dnf -y install docker-ce
