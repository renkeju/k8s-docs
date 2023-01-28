[1]: /images/deploy/GlusterFS-Arch.drawio.png

# B 部署 GlusterFS 及 Heketi

在实践 Kubernetes 的 StatefulSet 及各种需要持久存储数据的组件活功能时，通常会用到 PV 的动态供给功能，这就需要用到支持此类功能的存储系统。在各类支持PV动态供给功能的存储系统中，GlusterFS 的设定较为简单，因此本书用到的各类动态存储功能将以此为例进行说明。本章将试图为读者提供一个设置以满足此功能的 GlusterFS 存储系统的简单说明文档，而不是对 GlusterFS 进行全面介绍。

![GlusterFS 架构][1]

在本示例中，gfs01.renkeju.com、gfs02.renkeju.com 和 gfs03.renkeju.com 三个节点组成了 GlusterFS 存储集群（如图 B-1 所示），并将 gfs01 节点部署为 heketi 服务器。多节点上，sdb、sdc 和 sdd 用于为 GlusterFS 提供存储空间。

