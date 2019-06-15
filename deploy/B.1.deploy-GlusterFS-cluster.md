# B.1 部署 GlusterFS 集群

首先，分别在三个节点上安装 glusterfs-server 程序包，并启动 glusterfsd 服务，命令如下：

```
**[terminal]
**[path  ~]]**[delimiter  # ]**[command yum install centos-release-glusterfs]
**[path  ~]]**[delimiter  # ]**[command yum --enbalerepo=centos-gluster*-test install glusterfs-server]
**[path  ~]]**[delimiter  # ]**[command systemctl start glusterd.service]
```

第二步，在任一节点上使用 `glusterfs peer povbe` 命令“发现”其他节点，组建 GlusterFS 集群。命令格式为`peer probe {<HOSTNAME>|<IP-address>}`，例如，在 gfs01 上运行如下命令：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command gluster peer probe gfs02.renkeju.com]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command gluster peer probe gfs03.renkeju.com]
```

第三步，通过节点状态命令 `gluster peer status` 确认各节点已经加入同一个可信池中（trusted pool）:

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command gluster peer probe gfs03.renkeju.com
peer probe: success.
[root@gfs01 ~]# gluster peer status
Number of Peers: 2

Hostname: gfs02.renkeju.com
Uuid: d3621cd7-c621-4a2b-88c2-dc3a18b29d5b
State: Peer in Cluster (Connected)

Hostname: gfs03.renkeju.com
Uuid: e1b8caf5-6f58-4082-b492-9f63ee6d5704
State: Peer in Cluster (Connected)]
```