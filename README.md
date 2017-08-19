<img src="https://i.imgur.com/gMGVimd.png" width="500">

> Simple as a shell script. It allow you to deploy easily k8s for test or dev purposes.

Simplekube a way to install Kubernetes on a single Linux server without have to plug with any cloud provider or Hypervisor. Just take a Linux empty box, clone the git repo, launch the script and have fun with k8s !

### How-to use it ?

##### 1- Tweak the head of `install_k8s.sh`
 
 ```
k8sVersion="v1.7.3"
etcdVersion="v3.2.5"
dockerVersion="17.05.0-ce"
cniVersion="v0.5.2"
calicoCniVersion="v1.10.0"
calicoctlVersion="v1.3.0"
cfsslVersion="v1.2.0"
helmVersion="v2.6.0"
hostIP="__PUBLIC_OR_PRIVATE_IPV4__"
clusterDomain="cluster.local"
 ```
##### 2- Launch the script as user (with sudo power)

`./install_k8s.sh --master`

##### 3- You now play with kubectl, helm, calicoctl (...)

```
$- kubectl get cs 

NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health": "true"}

$- kubectl get pod --all-namespaces
NAMESPACE     NAME                                        READY     STATUS    RESTARTS   AGE
kube-system   calico-policy-controller-4180354049-63p5v   1/1       Running   0          4m
kube-system   kube-dns-1822236363-zzkdq                   4/4       Running   0          4m
kube-system   kubernetes-dashboard-3313488171-lff6h       1/1       Running   0          4m
kube-system   tiller-deploy-1884622320-0glqq              1/1       Running   0          4m

$- calicoctl get ippool
CIDR
192.168.0.0/16
fd80:24e2:f998:72d6::/64
```
##### 4- Cluster's integrated components :

  - KubeDNS
  - HELM ready
  - KubeDashboard
  - RBAC enabled by default
  - Calico CNI plugin
  - Calico Policy controller 
  - Calicoctl
  - UFW to secure access (can be disabled)

##### 5- Expose services :

You can expose easily your services with :

  - Only reachable on the machine : `ClusterIP`
  - Expose on high TCP ports : `NodePort`
  - Expose publicly : Service's `ExternalIPs`

##### 6- Add new node if needed :

You can easily add a new to your cluster by launching `./install_new_worker.sh`

Before launch the script, be sure to tweak the script config :

```
nodeIP="__PUBLIC_OR_PRIVATE_IPV4__"
setupFirewall="True"
CAcountry="US"
```

### Requirements

This script download each k8s components with `wget` and launch k8s with `systemd units`. 
You will need `socat` installed and `git` to fetch the Git repo !

Feel free to open an Issue if you need assistance !

