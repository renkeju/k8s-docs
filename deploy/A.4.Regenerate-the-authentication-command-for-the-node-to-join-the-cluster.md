# A.4 重新生成用于节点加入集群的认证命令

 如果忘记了记录 Master 主机的 kubeadm init 命令执行结果中用于让节点加入集群的 kubeadm join 命令及其认证信息，则需要分别通过 kubectl 获取认证令牌及验证 CA 公钥的哈希值。

 `kubeadm token list` 能够获取到集群上存在认证令牌，定位到其中 DESCRIPTION 字段中标示为由 `kubeadm init` 命令生成的行，其第一字段 TOKEN 中的令牌即为认证令牌。而验证CA公钥的哈希值（discovery-token-ca-cert-hash）的获取命令则略微复杂，其完成格式如下所示：

 ```
**[terminal]
[**[prompt root@master]**[path  ~]]**[delimiter  # ]**[command openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2> /dev/null | openssl dgst -sha256 -hex | sed 's/^.* //']
 ```

而后，将上述两个命令生成的结果合成如下格式的 kubeadm join 命令即可用于让 Node 加入集群中，其中的 TOKEN 可替换为上面第一个命令的生成结果，HASH 可替换为第二个命令的生成结果：

```
kubeadm join 172.16.0.6:6443 --token TOKEN --discover-token-ca-cert-hash HASH
```

最后，需要提供读者注意的是，Kubernetes 项目目前处于快速迭代时期，其版本号演进速度较快，因此，读者在测试阶段试默认安装的程序版本与本书使用的极有可能存在着不同，甚至连部署步骤都可能会发生改变。