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
[20]: chapter_2/2.kubernetes_quick_start.md
[21]: chapter_2/2.1.core_object_of_kubernetes.md
[22]: chapter_2/2.1.1.pod-resource-object.md
[23]: chapter_2/2.1.2.controller.md
[24]: chapter_2/2.1.3.service.md
[25]: chapter_2/2.1.4.deploy_the_main_process_of_the_application.md
[26]: chapter_2/2.2.deploy_kubernetes_cluster.md
[27]: chapter_2/2.2.1.kubeadm_deploy_tool.md
[28]: chapter_2/2.2.2.cluster_cluster_operation_mode.md
[29]: chapter_2/2.2.3.prepare_a_clustered_environment_for_hands-on_operations.md
[30]: chapter_2/2.2.4.get_information_about_the_cluster_environment.md
[31]: chapter_2/2.3.kubectl_use_the_basics_and_examples.md
[32]: chapter_2/2.4.imperative_container_application_orchestration.md
[33]: chapter_2/2.4.1.deploy_applications.md

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
* [2 kubernetes 快速入门][20]
    * [2.1 Kubernetes 的核心对象][21]
        * [2.1.1 Pod 资源对象][22]
        * [2.1.2 Controller][23]
        * [2.1.3 Service][24]
        * [2.1.4 部署应用程序的主体过程][25]
    * [2.2 部署 Kubernetes 集群][26]
        * [2.2.1 kubeadm 部署工具][27]
        * [2.2.2 集群运行模式][28]
        * [2.2.3 准备用于实践操作的集群环境][29]
        * [2.2.4 获取集群环境相关的信息][30]
    * [2.3 kubectl 使用基础与示例][31]
    * [2.4 命令式容器应用编排][32]
        * [2.4.1 部署应用（Pod）][33]