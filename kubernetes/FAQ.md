#### Master初始化  
><font color="#dd0000">"[ERROR CRI]: container runtime is not running"</font>  
> 解决办法：  
> 删除config.toml文件  
> rm /etc/containerd/config.toml  
> 重启服务  
> systemctl restart containerd  
>   
> 然后重新初始化：  
> sudo kubeadm init --pod-network-cidr=10.244.0.0/16  
> 
#### Master安装pod网络插件flannel  
> <font color="#dd0000">"coredns dial tcp 10.96.0.1:443 connect: no route to host"</font>  
> 解决办法：  
> 可能是防火墙（iptables）规则错乱或者缓存导致的，可以依次执行以下命令进行解决：  
> sudo systemctl stop kubelet  
> sudo systemctl stop docker  
> sudo iptables --flush  
> sudo iptables -tnat --flush  
> sudo systemctl start kubelet  
> sudo systemctl start docker  
> 
> 然后删除kube-flannel网络插件  
> kubectl delete pod coredns-787d4945fb-b5s6h -n kube-system  
> kubectl delete pod coredns-787d4945fb-zzdnm -n kube-system  

#### Node加入集群  
> <font color="#dd0000">”container runtime is not running”</font>  
> 解决办法:  
> 删除config.toml文件  
> `sudo rm /etc/containerd/config.toml`  
> 重启服务  
> `sudo systemctl restart containerd`

#### 安装kubectl、kubelet、kubeadm出现的签名问题  
> <font color="#dd0000">"W: GPG error: https://packages.cloud.google.com/apt kubernetes-xenial InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY B53DC80D13EDEF05 NO_PUBKEY FEEA9169307EA071
E: The repository 'http://apt.kubernetes.io kubernetes-xenial InRelease' is not signed."</font>
> 
> 解决办法：  
> `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys FEEA9169307EA071`