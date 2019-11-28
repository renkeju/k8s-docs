# B.2.1 安装并启动 Heketi 服务器

首先安装 Heketi。Heketi 程序可于 epel 仓库中获取，配置好相关的仓库后即可运行如下安装命令：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command yum install heketi heketi-client]
```

第二步，配置 Heketi 用户能够基于 SSH 密钥的认证方式连接至 GlusterFS 集群中的各节点，并拥有相应节点的管理权限：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command ssh-keygen -f /etc/heketi/heketi_key -t rsa -N '']
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command chown heketi:heketi /etc/heketi/heketi_key*]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command for host in gfs01 gfs02 gfs03; do ssh-copy-id -i /etc/heketi/heketi_key.pub root@${host}.renkeju.com; done]
```

第三步，设置 Heketi 的主配置文件 /etc/heketi/heketi.json，定义服务监听的端口、认证及连接 Gluster 存储集群的方式。一个配置示例如下：

```json
{
    "port": "8080",
    "user_auth": false,
    "jwt": {
        "admin": {
            "key": "admin Secret"
        },
        "user": {
            "key": "user Secret"
        }
    },
    "glusterfs": {
        "executor": "ssh",
        "sshexec": {
            "keyfile": "/etc/heketi/heketi_key",
            "user": "root",
            "port": "22",
            "fstab": "/etc/fstab"
        },
        "db": "/var/lib/heketi/heketi.db",
        "loglevel": "debug"
    }
}
```

若要启用连接 Heketi 的认证，则需要将“use_auth”参数的值设置为“true”，并在“jwt{}”配置段中为各用户设定相应的密码，用户名和密码都可以自定义。“glusterfs{}“ 配置段用于指定接入 Gluster 存储集群的认证方式及信息。

> 若启用了认证功能，则于 Kubernetes 集群中配置存储类时需要设置相应的认证信息。

第四步，启动 Heketi 服务：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command systemctl enable heketi]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command systemctl start heketi]
```

需要注意的是，将 Gluster 存储集群的功能托管于 Heketi 之后便不能够再于集群中使用命令管理存储卷，以免于 Heketi 数据库中存储的信息不一致。

第五步，向 Heketi 发起访问测试请求，无须认证时，使用 curl 命令既能完成测试：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command curl http://gfs01.renkeju.com:8080/hello]
Hello from Heketi
```

若 Heketi 启用了认证功能，则需要使用 heketi-cli 命令进行测试，命令格式如下：

```
**[terminal]
[**[prompt root@gfs01]**[path  ~]]**[delimiter  # ]**[command heketi-cli --server http://<server:port> --user <user> --secret <secret> cluster list]
```
