# A.2 部署 Kubernetes 集群

kubeadm 是用于快速构建 Kubernetes 集群的工具，随着 Kubernetes 的发行版本而提供，使用它构建集群时，大致可分为如下几步：

1. 在 Master 及各 Node 安装 Docker、kubelet 及 kubeadm。并以系统守护进程的方式启动 Docker 和 kubelet 服务。
2. 在 Master 节点上通过 kubeadm init 命令进行集群初始化。
3. 各 Node 通过 kubeadm join 命令加入初始化完成的集群中。
4. 在集群上部署网络附件，如 flannel 或 Calico 等以提供 Service 网络及Pod网络。

为了简化部署过程，kubeadm 使用一组固定的目录及文件路径存储相关的配置及数据文件，其中 `/etc/kubernetes` 目录使所有文件或目录的统一存储目录。它使用 `/etc/kubernetes/manifests` 目录存储各静态 Pod 资源的配置清单，用到的文件有 etcd.yaml、kube-apiserver.yaml、kube-controller-manager.yaml 和 kube-scheduler.yaml 四个，它们的作用基本能够见文件，如 kubelet.conf、controller-manager.conf、scheduler.conf 和 admin.conf 等，它们分别为相关的的组件提供接入 API Server 的认证信息等。此外，它还会在 `/etc/kubernetes/pki` 目录中存储若干私钥和证书文件。

