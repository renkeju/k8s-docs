# A.2.3 集群初始化

一旦 Master 和各个 Node 的 Docker 及 kubelet 设置完成后，接着便可以在 Master 节点上执行 “kubeadm init” 命令进行集群初始化。kubeadm init 命令支持两种初始化方式，一是通过命令行选项传递关键的参数设定，另一个是基于 yaml 格式的专用配置文件设定更详细的配置参数。下面分别给出了两种实现方式的配置步骤，建议读者采用第二种方式。

## Master 初始化方式一

运行下面的命令，便可完成 Master 的初始化：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubeadm init \
  --kubernetes-version=v1.12.1 \
  --pod-network-cidr=10.244.0.0/16 \
  --service-cidr=10.96.0.0/12 \
  --apiserver-advertise=0.0.0.0 \
  --ignore-preflight-errors=Swap]
```

上面命令的选项及参数设定决定了集群运行环境的众多特性设定，这些设定对于此后在集群中部署运行应用程序至关重要。

* --kubernetes-version
  
  正在使用的 Kubernetes 程序组件的版本号，需要与 kubelet 的版本号相同。

* --pod-network-cidr
 
  Pod 网络的地址范围，其值为 CIDR 格式的网络地址；使用 flannel 网络插件时，其默认地址为 10.244.0.0/16。

* --service-cidr
 
  Service 的网络地址范围，其值为 CIDR 格式的网络地址，默认地址为 10.96.0.0/12。

* --apiserver-advertise-address

  API server 通告给其他组件的 IP 地址，一般应该为 Master 节点的 IP 地址，0.0.0.0 表示节点上所有可用的地址。

* --ignore-preflight-errors

  忽略哪些运行时的错误信息，其值为 Swap 时，表示忽略因 Swap 未关闭而导致的错误。

> 更多的参数请参考 kubeadm 的文档，链接地址为 https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/ 。

## Master 初始化方式二

kubeadm init 也可通过配置文件加载配置，以定制更丰富的部署选项。以下是符合前述命令设定方式的使用示例，不过，它明确定义了 kubeProxy 的模式为 ipvs，并支持通过修改 imageRepository 的值来修改获取系统镜像时使用的镜像仓库。

```yaml
apiVersion: kubeadm.k8s.io/v1alpha2
kind: MasterConfiguration
kubernetesVersion: v1.12.1
api:
  advertiseAddress: 172.20.0.70
  bindPort: 6443
  controlPlaneEndpoint: ""
imageRepository: k8s.gcr.io
kubeProxy:
  config:
    mode: "ipvs"
    ipvs:
      ExcludeCIDRs: null
      minSyncPeriod: 0s
      scheduler: ""
      syncPeriod: 30s
kubeletConfiguration:
  baseConfig:
    cgroupDriver: cgroupfs
    clusterDNS:
    - 10.96.0.10
    clusterDomain: cluster.local
    failSwapOn: false
    resolvConf: /etc/resolv.conf
    staticPodPath: /etc/kubernetes/manifests
networking:
  dnsDomain: cluster.local
  podSubnet: 10.244.0.0/16
  serviceSubnet: 10.96.0.0/12
```

将上面的内容保存于配置文件中，如 kubeadm-config.yaml ，而后执行相应的命令即可完成 Master 初始化：

```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command kubeadm init --config kubeadm-config.yaml --ignore-preflight-errors=Swap]
```

无论使用上述哪种方法，命令的执行过程都会执行众多部署操作并生成相关的信息，如生成配置文件，标示主节点、生成 bootstrap token 及部署核心附加组件 kube-proxy 和 kube-dns 等，尤其是为集群通信安全于 `/etc/kubernetes/pki` 目录中生成的一众密钥和数字证书，并在最后给出了 kubectl 客户端工具的配置文件生成方式以及随后将 Node 加入集群时使用的引导认证令牌（bootstarp token）等，后续需要加入集群的各个 Node 都将使用该引导认证令牌加入集群。不同版本的 kubeadm 其输出结果或许略有不同。本示例特定将需要注意的部分以粗体格式予以标示，它们是后续步骤的重要提示信息。命令执行的结果如下所示：

```

```