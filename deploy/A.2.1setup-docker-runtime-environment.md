# A.2.1 设定容器运行环境

Kubernetes 支持多种容器运行时环境，例如 Docker、RKT 和 Frakti 等，本书将采用其中目前最为流行的、接受程度最为广泛的 Docker，它的常用部署方式有两种，具体如下。
* 由系统发行版的程序包仓库提供，如 CentOS 7 Extras 仓库中的 Docker。
* Docker 官方仓库中的程序包，以 CentOS 7 为例，它通常能够提供较 Extras 仓库中更新版本的程序包，获取地址为 https://download.docker.com/ 。

本文采用的是第二种方式，不过，Kubernetes 认证的 Docker 版本通常忽略低于其最新版本，因此生产环境部署时应该尽可能部署经过认证的版本。考虑到 Kubernetes 版本迭代周期较短，它对 Docker 版本的支持也会快速变化，因此本示例讲直接使用 Docker 仓库中的最新版本。部署时，Docker 需要安装于 Master 及各 Node 主机之上，安装方式相同，其步骤如下：

```
**[terminal]
**[path  ~]]**[delimiter  # ]**[command wget https://download.docker.com/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo]
**[path  ~]]**[delimiter  # ]**[command yum install docker-ce]
```

> kubeadm 构建集群的过程需要到 gcr.io 中获取 Docker 镜像，因此必须确保 Docker 主机能够正常访问到此站点，否则，就得配置 Docker 以代理的方式访问 gcr.io，或者配置 kubeadm 从其他 Registry 获取相关的镜像。代理的方法是在 [service] 配置段中添加类似如下格式的配置项：`Environment="HTTP_PROXY=http://IP:PORT"` 或 `Environment="HTTPS_PROXY=https://IP:PORT"` 。

另外，Docker 自 1.13 版起会自动设置 iptables 的 FORWARD 默认策略为 DROP，这可能会影响 Kubernetes 集群依赖的报文转发功能，因此，需要在 docker 服务启动后，重新将 FORWARD 链的默认策略设置为 ACCEPT，方式是修改 `/usr/lib/systemd/system/docker.service` 文件，在 “ExecStart=/usr/bin/dockerd” 一行之后新增一行如下内容：

```ini
ExecStartPost=/usr/sbin/iptables -P FORWARD ACCEPT
```

上面各步骤设置完成后即可启动 docker 服务，并将其设置为随系统引导而自动启用，相关命令如下：

```
**[terminal]
**[path  ~]]**[delimiter  # ]**[command systemctl daemon-reload]
**[path  ~]]**[delimiter  # ]**[command systemctl start docker.service]
**[path  ~]]**[delimiter  # ]**[command systemctl enable docker.service]
```

> 国内访问 DockerHub 下载镜像的速度较缓慢，建议使用国内的镜像对其进行加速，如 https://registry.docker-cn.com ，另外，中国科技大学也提供了公共可用的镜像加速服务，其 URL 为 https://docker.mirrors.ustc.edu.cn ，将其定义在 daemon.json 中重启 Docker 即可使用。
