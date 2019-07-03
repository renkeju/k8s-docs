[1]: deploy/README.md
[2]: deploy/A.1.ready-to-deploy-kubernetes-cluster.md
[3]: deploy/A.1.1.deploy-target.md
[4]: deploy/A.1.2.os-environment&deploy-preparation.md
[5]: deploy/A.2.deploy-kubernetes-cluster.md
[6]: deploy/A.2.1.setup-docker-runtime-environment.md
[7]: deploy/A.2.2.setup-kubernetes-cluster-node.md
[8]: deploy/A.2.3.cluster-initialization.md
[9]: deploy/A.2.4.setup-kubectl-configuration.md
[10]: deploy/A.2.5.deploy-network-plugin.md
[11]: deploy/A.2.6.add-node-to-the-cluster.md
[12]: deploy/A.2.7.get-cluster-status-information.md
[13]: deploy/A.3.remove-a-node-from-the-cluster.md
[14]: deploy/A.4.Regenerate-the-authentication-command-for-the-node-to-join-the-cluster.md
[15]: deploy/B-chapter-deploy-GlusterFS-and-Heketi.md
[16]: deploy/B.1.deploy-GlusterFS-cluster.md
[17]: deploy/B.2.deploy-Heketi.md
[18]: deploy/B.2.1.install-and-start-Heketi-server.md
[19]: deploy/B.2.2.setup-Heketi-system-topology.md 

![Kubernetes](/images/kubernetes-logo.png)

[![Build Status](https://travis-ci.com/renkeju/k8s-docs.svg?branch=master)](https://travis-ci.com/renkeju/k8s-docs)

* [A 部署 Kubernetes][1]
    * [A.1 准备部署 Kubernetes 集群][2]
        * [A.1.1 部署目标][3]
        * [A.1.2 系统环境及部署准备][4]
    * [A.2 部署 Kubernetes 集群][5]
        * [A.2.1 设定容器运行环境][6]
        * [A.2.2 设定 kubernetes 集群节点][7]
        * [A.2.3 集群初始化][8]
        * [A.2.4 设定 kubectl 的配置文件][9]
        * [A.2.5 部署网络插件][10]
        * [A.2.6 添加 Node 到集群中][11]
        * [A.2.7 获取集群状态信息][12]
    * [A.3 从集群中移除节点][13]
    * [A.4 重新生成用于节点加入集群的认证命令][14]
* [B 部署 GlusterFS 及 Heketi][15]
    * [B.1 部署 GlusterFS 集群][16]
    * [B.2 部署 Heketi][17]
        * [B.2.1 安装并启动 Heketi 服务器][18]
        * [B.2.2 设置 Heketi 系统拓扑][19]