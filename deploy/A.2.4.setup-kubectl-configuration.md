# A.2.4 设定 kubectl 的配置文件

kubectl 是执行 Kubernetes 集群管理的核心工具。默认情况下，kubectl 会从当前用户主目录（保存雨环境变量 HOME 中的值）中的隐藏目录 .kube 下名为 config 的配置文件中读取配置信息，包括要接入 Kubernetes 集群、以及用于集群认证的证书或令牌等信息。集群初始化时，kubeadm 会自动生成一个用于此类功能的配置文件  `/etc/kubernetes/admin.conf`，将它复制为用户的 $HOME/.kube/config 文件即可直接使用。这里以 Master 节点上的 root 用户为例进行操作，不过，在实践中应该以普通用户的身份进行：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command mkdir -p $HOME/.kube]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command cp -i /etc/kubernetes/admin.conf $HOME/.kube/config]
```

至此为止，一个 Kubernetes Master 节点已经基本配置完成。接下来即可通过 API Server 来验证其各组件的运行是否正常。kubectl 有着众多子命令，其中 `get compontsstatuses` 即能显示出集群组件当前的状态，也可使用其简写格式 `get cs`：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl get cs
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}]
```

若上面命令结果的 STATUS 字段为 “Healthy”，则表示组建处于健康运行状态，否则需要检查其错误所在，必要时可使用 “kubeadm reset” 命令重置之后重新进行集群初始化。

另外，使用 `kubectl get node` 命令能够获取集群节点的相关状态信息，如下命令结果显示了 Master 节点的状态为 “NotReady”（未就绪），这是因为集群中尚未安装网络插件所致，执行完后面的其他步骤后它即自行转为 “Ready”：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubectl get nodes
NAME                 STATUS   ROLES    AGE    VERSION
master.renkeju.com   NotReady    master   108s   v1.14.3]
```
