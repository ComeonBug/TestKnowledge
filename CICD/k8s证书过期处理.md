### 证书续约

这个是老大难问题了，k8s 希望通过证书，强制定期升级 k8s 版本。但是我司技术保守，锁定了版本就不会换。所以开发环境也是尽可能维持版本。那每年就必须人手更换升级证书。先说解法：

1. 检查过期时间：

```shell
[root@n1 wuziming] kubeadm certs check-expiration
[check-expiration] Reading configuration from the cluster... 
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml' W0228 11:17:31.214122  103782 utils.go:69]
The recommended value for "clusterDNS" in "KubeletConfiguration" is: [10.233.0.10]; 
the provided value is: [169.254.25.10]  CERTIFICATE EXPIRES RESIDUAL TIME  CERTIFICATE AUTHORITY  EXTERNALLY MANAGED admin.conf Jul 06, 2023 09:12 UTC  128d ca no apiserver Jul 06, 2023 09:12 UTC 128d ca no apiserver-kubelet-client  Jul 06, 2023 09:12 UTC  128d       ca no controller-manager.conf Jul 06, 2023 09:12 UTC 128d ca no front-proxy-client Jul 06, 2023 09:12 UTC  128d front-proxy-ca no scheduler.conf Jul 06, 2023 09:12 UTC  128d       ca no  CERTIFICATE AUTHORITY  EXPIRES RESIDUAL TIME  EXTERNALLY MANAGED ca Jul 03, 2032 09:12 UTC  9y  no front-proxy-ca Jul 03, 2032 09:12 UTC  9y no  
```

可以看到 9 年后，无论如何，这个集群都无法人手维持了。如果想继续用在 Jul 06, 2023 09:12 前，我们必须人手续约一次证书。

2. 看 k8s 的官方文档是没有用的，因为 k8s 更新太快，文档已经不适用了。这里是我的神秘代码：

```shell
sudo su  # 只需要在 master 节点运行，我们开发环境三台全是 master 呢  
systemctl stop kubelet # 关闭 k8s  
# 如果你想保存一下之前的证书  
cd /etc/kubernetes/pki/ 
mv {apiserver.crt,apiserver-etcd-client.key,apiserver-kubelet-client.crt,front-proxy-ca.crt,front-proxy-client.crt,front-proxy-client.key,front-proxy-ca.key,apiserver-kubelet-client.key,apiserver.key,apiserver-etcd-client.crt} ~/{{ 备份文件夹 }}  
# 这里就是网上不会教你的，这些是只有自己安装的集群才知道要加的证书特殊 ip 地址 
# 127.0.0.1 是 loopback，需要加上 
# 10.233.0.1 是我为选定的 cni (calico) 定下的网关地址 
# 这个命令会刷新你的所有证书，但是不会更新 k8s apiserver/kubelet 等的配置文件  
kubeadm init phase certs all --apiserver-advertise-address {{ 机器 ip }} --apiserver-cert-extra-sans={{ 机器 ip }},127.0.0.1,10.233.0.1
```



理论上，这几个 ip 就够了，保险一点的做法是

```shell
cd /etc/kubernetes/pki 
openssl x509 -in apiserver.crt -text -noout 

Certificate:
   [隐藏涉密内容]
   Signature Algorithm: sha256WithRSAEncryption
      [隐藏涉密内容]
      X509v3 extensions:
         [隐藏涉密内容]       
         X509v3 Subject Alternative Name:
            DNS:kubernetes, DNS:kubernetes.default, DNS:kubernetes.default.svc, DNS:kubernetes.default.svc.cluster.local, DNS:lb-apiserver.kubernetes.local, DNS:localhost, DNS:n1, DNS:n1.dc1.domain, DNS:n2, DNS:n2.dc1.domain, DNS:n3, DNS:n3.dc1.domain, DNS:sh-idc1-10-142-6-33, DNS:sh-idc1-10-142-6-34, DNS:sh-idc1-10-142-6-35, IP Address:10.233.0.1, IP Address:10.142.6.33, IP Address:127.0.0.1, IP Address:10.142.6.34, IP Address:10.142.6.35
   [隐藏涉密内容]
```



把这里的每一个 SAN 都抄写进去，讲道理不用，有些应该是 k8s 自己负责加的。

```shell
# 如果你想保存一下之前的配置文件  
cd /etc/kubernetes/ 
mv {admin.conf,controller-manager.conf,kubelet.conf,scheduler.conf} ~/{{ 备份文件夹 }}  
# 更新所有配置  kubeadm init phase kubeconfig all  
# 把新的 admin 配置拷贝到本地，这样你的 kubectl 才能动呢  
cp /etc/kubernetes/admin.conf ~/.kube/config  
systemctl start kubelet 
# 重启 k8s  
# 请用 docker 看看 kube-apiserver/kube-controller-manager 的状态好了没，日志正常了没 
# 正常了之后 
# kubectl 随便干点什么看看，nodes里，没执行的 master 节点应该是 not ready 的  
kubectl get nodes  
# 看看证书信息 
kubeadm certs check-expiration
```



最后记得更新 cd-k8s 的开发环境 k8s config