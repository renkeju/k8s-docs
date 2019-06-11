# A.2.5 部署网络插件

为 Kubernetes 提供 Pod 网络的插件有很多，目前最为流行的是 flannel 和 Cailco。相比较来说，flannel 以其简单、易部署、易用性等特性广受用户喜欢。基于 kubeadm 部署时，flannel 同样运行为 Kubernetes 集群的附件，以 Pod 的形式部署运行于每个集群节点上以接受 Kubernetes 集群管理。事实上，也可以直接将 flannel 程序包装并以守护进程的方式运行于集群节点上，即以非托管的方式运行。部署方式既可以是获取其资源配置清单于本地而后部署于集群中，也可以直接在线进行应用部署。部署命令是 “kubectl apply” 或 “kubectl create”，例如，下面的命令将直接使用在线的配置清淡进行 flannel 部署：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml]
```

kubectl 可根据定义资源对象的清单文件将其提交给 API Server 以管理对象，如使用 “kubectl apply -f /PATH/TO/MANIFEST” 命令即可根据清单设置资源的目标状态。kubectl 的具体使用会再后面的章节进行详细的介绍。

配置 flannel 网络插件时，Master 节点上的 Docker 首先会去获取 flannel 的镜像文件，而后根据镜像文件启动相应的 Pod 对象。待其运行完成后再次查看集群中的节点状态可以看出 Master 已经变为 “Ready” 状态：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl get nodes
NAME                 STATUS   ROLES    AGE   VERSION
master.renkeju.com   Ready    master   45h   v1.14.3]
```

> "kubectl get pods -n kube-system | grep flannel" 命令的结果显示 Pod 的状态为 Running 时即表示网络插件 flannel 部署完成。

