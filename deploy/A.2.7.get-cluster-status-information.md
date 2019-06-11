# A.2.7 获取集群状态信息

Kubernetes 集群以及部署的插件提供了多种不同的服务，如此前部署过的 API Server、Kube-dns 等。API 客户端访问集群时需要事先知道 API Server 的通告地址，管理员可使用 “kubectl cluster-info” 命令了解到这些信息：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl cluster-info
Kubernetes master is running at https://172.16.0.6:6443
KubeDNS is running at https://172.16.0.6:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.]
```

Kubernetes 集群 Server 端和 Client 端的版本等信息可以使用 “kubectl version” 命令进行查看：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl version --short=true
Client Version: v1.14.3
Server Version: v1.14.0]
```