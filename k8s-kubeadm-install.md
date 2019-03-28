## kubernetes
### Debian / Ubuntu
```
apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF  
apt-get update
#apt-get install -y kubelet kubeadm kubectl
apt install kubelet=1.11.3-00
apt install kubectl=1.11.3-00
apt install kubeadm=1.11.3-00
```
### CentOS / RHEL / Fedora
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
setenforce 0
yum install -y kubelet kubeadm kubectl
systemctl enable kubelet && systemctl start kubelet
```

由于国内网络原因，kubernetes的镜像托管在google云上，无法直接下载，所以直接把把镜像搞下来有个技术大牛把gcr.io的镜像
每天同步到https://github.com/anjia0532/gcr.io_mirror这个站点，因此，如果需要用到gcr.io的镜像，可以执行如下的脚本进行镜像拉取

```
vim pullimages.sh

#!/bin/bash
images=(kube-proxy-amd64:v1.11.1 kube-scheduler-amd64:v1.11.1 kube-controller-manager-amd64:v1.11.1
kube-apiserver-amd64:v1.11.1 etcd-amd64:3.2.18 coredns:1.1.3 pause:3.1 )
for imageName in ${images[@]} ; do
docker pull anjia0532/google-containers.$imageName
docker tag anjia0532/google-containers.$imageName k8s.gcr.io/$imageName
docker rmi anjia0532/google-containers.$imageName
done

sh pullimages.sh

vim kubeadm.yaml 

apiVersion: kubeadm.k8s.io/v1alpha1
kind: MasterConfiguration
controllerManagerExtraArgs:
  horizontal-pod-autoscaler-use-rest-clients: "true"
  horizontal-pod-autoscaler-sync-period: "10s"
  node-monitor-grace-period: "10s"
apiServerExtraArgs:
  runtime-config: "api/all=true"
kubernetesVersion: "v1.11.1"

sudo kubeadm init --config kubeadm.yaml

$ kubectl taint nodes --all node-role.kubernetes.io/master-
```

- To start using your cluster, you need to run the following as a regular user:
```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
- 部署网络插件：(确保能连到grc.io下载所需镜像，或者自己下载镜像）
```
 $ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
```

- 当然，如果你就是想要一个单节点的 Kubernetes，删除...
```
$ kubectl taint nodes --all node-role.kubernetes.io/master-
```

### dashboard install 
- [blog] "https://blog.csdn.net/KingBoyWorld/article/details/79889209"
```
docker pull siriuszg/kubernetes-dashboard-amd64:v1.8.3
docker tag siriuszg/kubernetes-dashboard-amd64:v1.8.3 k8s.gcr.io/kubernetes-dashboard-amd64:v1.8.3
kubectl create -f kubernetes-dashboard.yaml
```

- 部署https通信的Dashboard

openssl可以实现：秘钥证书管理、对称加密和非对称加密 。
-in filename：指定要加密的文件存放路径
-out filename：指定加密后的文件存放路径
1. 生成证书

a) 生成密钥 
```
$ (umask 077; openssl genrsa -out dashboard.key 2048)
  - umask命令来设置生成的密钥文件的权限
  - numbits：指定生成私钥的大小，默认是2048
```
b) 生成证书 

```
$ openssl req -new -key dashboard.key -out dashboard.csr -subj "/O=iLinux/CN=dashboard"
```
- req 子命令为，其为证书请求及生成的工具
  ```
    -new：表示生成一个新的证书签署请求；
    -x509：专用于生成CA自签证书；
    -key：指定生成证书用到的私钥文件；
    -out FILNAME：指定生成的证书的保存路径；
    -days：指定证书的有效期限，单位为day，默认是365天；
    -subj: set or modify request subject
  ```
c) 颁发证书 
```
$ openssl x509 -req -in dashboard.csr -CA /etc/kubernetes/pki/ca.crt \
    -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out dashboard.crt -days 3650
    
    -CA 子命令CA，用于在CA服务器上签署或吊销证书
      -CAkey: set the CA key, must be PEM format
                   missing, it is assumed to be in the CA file.
      -CAcreateserial: create serial number file if it does not exist
      -days: How long till expiry of a signed certificate - def 30 days
    -X509 子命令x509
```
2. 基于生成的私钥和证书文件创建名为kubernetes-dashboard-certs的Opaque类型的Secrets对象，其键名分别为dashboard.key和dashboard.crt 
```
$ kubectl create secret generic kubernetes-dashboard-certs \
    -n kube-system --from-file=dashboard.crt=./dashboard.crt \
    --from-file=dashboard.key=./dashboard.key -n kube-system
```    
- Secrets对象准备完成后即可部署Dashboard。

在线直接创建：
```
$ docker pull mirrorgooglecontainers/kubernetes-dashboard-amd64:v1.10.1
$ docker tag mirrorgooglecontainers/kubernetes-dashboard-amd64:v1.10.1 k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

$ kubectl patch svc kubernetes-dashboard -p '{"spec":{"type":"NodePort"}}' -n kube-system
```
- 通过nodeip+nodeport在集群外通过浏览器访问：
```
$ kubectl get svc kubernetes-dashboard -n kube-system
```

3. 配置token认证
a)创建serviceaccount,并完成集群角色绑定
```
$ kubectl create serviceaccount dashboard-admin -n kube-system
$ kubectl create clusterrolebinding dashboard-admin --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin
```
b)获取token
```
$ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-token | awk '{print $1}') 

Name:         dashboard-admin-token-xs4zb
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=dashboard-admin
              kubernetes.io/service-account.uid=4a8611e3-47c3-11e9-8cd2-00163e0c585a

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYXNoYm9hcmQtYWRtaW4tdG9rZW4teHM0emIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGFzaGJvYXJkLWFkbWluIiwia3ViZXJ   
```

### Question

- Q: node execute command "kubeadm join ..." the error:
```
    Unfortunately, an error has occurred:
      timed out waiting for the condition

    This error is likely caused by:
      - The kubelet is not running
      - The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

    If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
      - 'systemctl status kubelet'
      - 'journalctl -xeu kubelet'
    timed out waiting for the condition
```
- A: sudo apt-get remove kubelet --purge and then install it again.
---
- Q: helm install  occur " has no deploy release.."
- A: helm delete --purge {helm.release}
---

- Q: Error: object is being deleted: customresourcedefinitions.apiextensions.k8s.io "alertmanagers.monitoring.coreos.com" already exists
- A: 
```
$ helm delete my-release

kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
```
---
- Q:Error: UPGRADE FAILED: configmaps is forbidden: User "system:serviceaccount:kube-system:default" cannot list configmaps in the namespace "kube-system"
- A: 
```
kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default
```

