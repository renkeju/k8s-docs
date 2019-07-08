# A.2.2 设定 kubernetes 集群节点

kubelet 使运行于集群中每个节点之上的 Kubernetes 代理程序，它的核心功能在于通过 API Server 获取调度至自身运行的 Pod 资源的 PodSpec 并依之运行 Pod 对象。事实上，以自托管的方式部署的 Kubernetes 集群，除了 kubelet 和 Docker 之外的所有组件均以 Pod 对象的形式运行。

## 1. 安装 kubelet 及 kubeadm

安装 kubelet 的常用方式包含如下几种。快速迭代期内，Linux 发行商提供的安装包通常版本较低，因此建议采用下列方式的第一种或第二种，本章采用第二种方式。

* Kubernetes 提供的二进制格式的 tar 包。
* Google 的 yum 仓库中提供的 rpm 包，可通过国内的镜像站点获取，例如阿里云镜像站。
* OS 发行商提供的安装包，例如 CentOS 7 Extras 仓库中的 Kubernetes 相关程序包。

> 对于 rpm 方式的安装来说，kubelet、kubeadm he kubectl 等是各自独立的程序包，Master 及各 Node 至少应该安装 kubelet 和 kuebadm，而 kubectl 则只需要安装于客户端主机即可，不过，由于依赖关系它通常也会自动安装。另外，Google 提供的 kubelet rpm 包的 yum 仓库托管于 Google 站点的服务器主机之上，目前访问起来略有不便。幸运的是，目前国内阿里云等镜像 (https://opsx.alibaba.com/mirror) 对此项目也有镜像提供。

首先设定用于安装 kubelet、kubeadm 和 kubectl 等组件的 yum 仓库，编辑配置文件 `/etc/yum.repos.d/kubenetes.repo`，内容如下：

```ini
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
```

而后执行如下命令即可安装相关的程序包：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command yum install kubeadm kubelet kubectl]
```

## 2. 配置 kubelet

Kubernetes 自 1.8 版本其强制要求关闭系统的交换分区（Swap），否则 kubelet 将无法启动。当然，用户也可以通过将 kubelet 的启动参数 “--fail-swap-on” 设置为 “false” 忽略此限制，尤其使系统上运行有其他重要进程且系统内存资源稍显不足时建议保留交换分区。

编辑 kubelet 的配置文件 `/etc/sysconfig/kubelet`，设置其配置参数如下，以忽略禁止使用 Swap 的限制：

```ini
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
```

待配置文件的修改完成后，需要设定 kubelet 服务开机自动启动，这也是 kubeadm 的强制要求：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command systemctl enable kubelet.service]
```