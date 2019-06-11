# A.3 从集群中移除节点

运行过程中，若有节点需要从正常运行的集群中移除，则可使用如下步骤来进行。

1. 在 Master 上使用如下命令 “排干”（迁移至集群中的其他节点）当前节点之上的 Pod 资源并移除 Node 节点：

    ```
    **[terminal]
    **[path  ~]]**[delimiter  # ]**[command kubectl drain NODE_ID --delete-local-data --force --ignore-daemonsets]
    **[path  ~]]**[delimiter  # ]**[command kubectl delete NODE_ID]
    ```

2. 而后在要删除的 Node 上执行如下命令重置系统状态便可完成移除操作：

    ```
    **[terminal]
    **[path  ~]]**[delimiter  # ]**[command kubectl delete node NODE_ID]
    ```