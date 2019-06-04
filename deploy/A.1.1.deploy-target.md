[1]: /images/deploy/Kubernetes-cluster-deploy-target-viewer.png "Kubernetes 集群部署目标示意图"

# A.1.1 部署目标

图 A-1 给出了本节要部署的目标集群的基本环境，它拥有一个 Master 主机和三个 Node 主机。各 Node 主机的配置方式基本相同。

![Kubernets 集群部署目标示意图][1]

各个主机上采用的容器运行时环境为 Docker，为 Pod 提供网络功能的 CNI 是 flannel，它运行为托管于 kubernetes 之上的 Pod 对象，另外，基础附件还包括 KubeDNS（或 CoreDNS）用于名称解析或服务发现。

