# B.2.2 设置 Heketi 系统拓扑

拓扑信息用于让 Heketi 确认可使用的节点、磁盘和集群，管理员必须自行确定节点故障域和节点集群。故障域时赋予一组节点的整数值，这组节点共享共同的交换机、电源或其他任何会导致他们同时失效的组件。管理员必须确定哪些节点构成一个集群，Heketi 使用这些信息来确保跨故障域中创建副本，从而提供数据的冗余能力。Heketi 支持多个 Gluster 存储集群，这为管理员提供了创建 SSD、SAS、SATA 或为用户提供特定服务质量的任何其他类型的集群的选项。

命令行客户端 heketi-cli 通过加载预定义的集群拓扑，从而添加节点到集群中，以及将磁盘关联到节点上。要使用 heketi-cli 加载拓扑文件，可通过以下命令完成：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command export HEKETI_CLI_SERVER=http://<heketi_server:port>]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli topology load --json=<topology_file>]
```

一个适用于当前配置环境的示例配置如下所示 `/etc/heketi/topology_demo.json` ，它根据 Gluster 存储集群的实际环境把 gfs01、gfs02 和 gfs03 三个节点定义在同一个集群中并指明各节点上可用于提供存储空间的磁盘设备：

```json
{
    "clusters": [
        {
            "nodes": [
                {
                    "node": {
                        "hostnames": {
                            "manage": [
                                "172.16.0.10"
                            ],
                            "storage": [
                                "172.16.0.10"
                            ]
                        },
                        "zone": 1
                    },
                    "devices": [
                        "/dev/vdb"
                    ]
                },
                {
                    "node": {
                        "hostnames": {
                            "manage": [
                                "172.16.0.11"
                            ],
                            "storage": [
                                "172.16.0.11"
                            ]
                        },
                        "zone": 1
                    },
                    "devices": [
                        "/dev/vdb"
                    ]
                },
                {
                    "node": {
                        "hostnames": {
                            "manage": [
                                "172.16.0.12"
                            ],
                            "storage": [
                                "172.16.0.12"
                            ]
                        },
                        "zone": 1
                    },
                    "devices": [
                        "/dev/vdb"
                    ]
                }
            ]
        }
    ]
}
```

而后运行如下命令加载拓扑信息，从而完成集群配置。此命令会生成一个集群，并为其添加的各节点生成随机 ID 号：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command export HEKETI_CLI_SERVER=http://gfs01.renkeju.com:8080]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli topology load --json=topology_demo.json]
```

而后运行如下命令查看集群的状态信息：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli cluster info f97313b9354866c7dc1b9f21a341605c
Cluster id: f97313b9354866c7dc1b9f21a341605c
Nodes:
62db8b2082e97bf5408b258d6a61c2a8
8dbb0b2812f2c05653a7fdcc3c510df0
f6f0e94620ed747b97f13df537423836
Volumes:

Block: true

File: true] 
```

`heketi-cli volume create --size=<size in Gb> [options]` 能够创建存储卷，例如，下面的命令测试即用测试即用于创建一个存储卷：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli volume create --size=2
Name: vol_a7b311867ae269d1e705f8a772e2b253
Size: 2
Volume Id: a7b311867ae269d1e705f8a772e2b253
Cluster Id: f97313b9354866c7dc1b9f21a341605c
Mount: 172.16.0.10:vol_a7b311867ae269d1e705f8a772e2b253
Mount Options: backup-volfile-servers=172.16.0.11,172.16.0.12
Block: false
Free Size: 0
Reserved Size: 0
Block Hosting Restriction: (none)
Block Volumes: []
Durability Type: replicate
Distributed+Replica: 3]
```

而后在要使用远程存储卷的节点上安装 GlusterFS 和 glusterfs-fuse 的程序包，提供 GlusterFS 客户端驱动及对 GlusterFS 文件系统的运行，并基于 GlusterFS 文件系统类型挂载使用确认无误后即可删除测试卷。删除 Heketi 卷的命令为 `heketi-cli volume delete <vol_id>`，如何删除前面创建的存储卷，可使用以下的命令：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli volume delete a7b311867ae269d1e705f8a772e2b253 
Volume a7b311867ae269d1e705f8a772e2b253 deleted]
```

至此为止，一个支持动态存储卷配置的 GlusterFS 存储集群即设置完成，用户即可于 Kubernetes 中通过 PVC 请求使用某事先创建完成的 GlusterFS 存储卷，也可把 Heketi 配置为存储类，而后提供 PV 的动态供给功能。