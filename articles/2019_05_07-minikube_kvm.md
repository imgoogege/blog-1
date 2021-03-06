# 在KVM里安装Minikube

我要在本地搭建一个Kubernetes，所以要在本地安装minikube，这是一个相对简单也节省资源的方案，但是minikube默认使用virtualbox
做虚拟机，而virtualbox的效率有点太低了，我要使用KVM。

首先安装 `kubectl` 和 `minikube`:

```bash
$ sudo pacman -Syu minikube kubectl
```

然后安装对应的虚拟机驱动:

```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-kvm2
$ sudo chown root:root docker-machine-driver-kvm2
$ sudo chmod +x docker-machine-driver-kvm2
$ sudo mv docker-machine-driver-kvm2 /usr/local/bin
```

然后就开始安装minikube:

> 如果没有设置代理，在国内可能会因为拉取不下镜像而导致失败，因此可以设置一个代理，例如：
> export HTTP_PROXY="http://<你的局域网ip>:8123/" 当然，前提时你在这个地址设置了http代理。

```bash
$ export HTTP_PROXY=192.168.100.24:8123
$ export HTTPS_PROXY=http://192.168.100.24:8123
$ export NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.99.0/24,192.168.39.0/24
$ minikube start --vm-driver kvm2 --memory 4096 --iso-url file:////home/jiajun/Images/minikube-v1.0.1.iso --docker-env=HTTP_PROXY=$HTTP_PROXY --docker-env HTTPS_PROXY=$HTTPS_PROXY   --docker-env NO_PROXY=$NO_PROXY --disk-size 40g
```

由于网络问题，一般来说都要等很久，就只能慢慢等了。

安装好之后，就可以使用了：

```bash
$ kubectl cluster-info 
Kubernetes master is running at https://192.168.39.205:8443
KubeDNS is running at https://192.168.39.205:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

---

- https://github.com/kubernetes/minikube/blob/master/docs/http_proxy.md
