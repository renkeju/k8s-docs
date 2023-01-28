[1]: /images/deploy/heketi-arch.drawio.png

# B.2 部署 Heketi

Heketi 为管理 GlusterFS 存储卷的生命周期提供一个 RESTful 管理接口，借助于 Heketi，像 OpenStack Manila、Kubernetes 和 OpenShift 这样的云服务可以动态调配 Gluster 存储卷。Heketi 能够自动确定整个集群的 brick 位置，并确保将 brick 及其副本放置在不同的故障域中。另外，Heketi 还支持任意数量的 Gluster 存储集群，并支持云服务提供网络文件存储。

有了 Heketi，存储管理员不必再管理或配置 brick、磁盘或可信存储池（trusted pool），Heketi 服务将为管理员管理所有硬件，并使其能够按需分配存储。不过，在 Heketi 中注册的任何磁盘都必须以原始格式提供，而不能是创建过文件系统的磁盘分区。Heketi 架构如下图：

![Heketi 架构][1]

本部署示例将 gfs01.renkeju.com 节点用作 Heketi 服务器，因此以下所有步骤均在 gfs01 上执行。  