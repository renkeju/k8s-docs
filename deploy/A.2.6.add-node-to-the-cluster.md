# A.2.6 添加 Node 到集群中

Master 各组建运行正常后即可将各 Node 添加至集群中。配置节点时，需要事先参考前面 “设置容器运行环境” 和 “设定 Kubernetes 集群节点” 两节中的配置过程设置好 Node 主机，而后即可再 Node 主机上使用 “kubeadm join” 命令将其加入集群中。不过，为了系统安全起见，任何一个试图加入到集群中的节点都需要先经由 API Server 完成认证，其认证方法和认证信息在 Master 上运行 `kubernetes init` 命令执行初始化时将于输出结果信息的最后一部分中提供。

例如，类似如下命令即可将 node01.renkeju.com 加入集群中，它使用集群初始化时生成的认证令牌（token）信息进行认证。另外，同样出于为操作系统及其他应用保留交换分区之目的，也可以在 kubeadm join 命令上添加 `--ignore-preflight-errors=Swap` 选项：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubeadm join 172.16.0.6:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:79f6a440a71e21bce3379b13fdb0931957225fd250334dddf7d978c4c8d979a3]
```

提供给 API Server 的 bootstrap token 认证完成后，kubeadm join 命令会为后续 Master 与 Node 组件间的双向 ssl/tls 认证生成私钥及证书签署请求，并由 Node 在首次加入集群时提交给 Master 端的 CA 进行签署。默认情况下，kubeadm 配置 kube-apiserver 启用了 bootstrap TLS 功能，并支持证书的自动签署。于是，kubelet 及 kube-proxy 等组件的相关私钥和证书文件在命令执行结束后便可自动生成，它们默认保存与 `/var/lib/kubelet/pki` 目录中。

在每个节点上重复上述步骤就能够将其加入集群中。所有节点加入完成后，即可使用 `kubectl get nodes` 命令验证集群的节点状态，包括各节点的名称、状态就绪与否、角色（是为节点 Master）、加入集群的时长以及程序的版本等信息：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl get nodes]
NAME                 STATUS   ROLES    AGE   VERSION
master.renkeju.com   Ready    master   45h   v1.14.3
node01.renkeju.com   Ready    <none>   45h   v1.14.3
node02.renkeju.com   Ready    <none>   45h   v1.14.3
node03.renkeju.com   Ready    <none>   45h   v1.14.3
```

到此为止，使用 kubeadm 构建 Kubernetes 集群已经完成。后续若有 Node 需要加入，其方式均可使用此节介绍的方式来进行。


