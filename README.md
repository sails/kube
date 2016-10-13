# kube
由于不能从google container上直接pull镜像，所以这里通过dockerfile直接获取到docker hub的服务器上，然后再从它们上面拉取.

##	kube 1.4需要的镜像:
```
gcr.io/google_containers/kube-proxy-amd64                v1.4.0
gcr.io/google_containers/kube-discovery-amd64            1.0
gcr.io/google_containers/kubedns-amd64                   1.7
gcr.io/google_containers/kube-scheduler-amd64            v1.4.0
gcr.io/google_containers/kube-controller-manager-amd64   v1.4.0
gcr.io/google_containers/kube-apiserver-amd64            v1.4.0
gcr.io/google_containers/etcd-amd64                      2.2.5
gcr.io/google_containers/kube-dnsmasq-amd64              1.3
gcr.io/google_containers/exechealthz-amd64               1.1
gcr.io/google_containers/pause-amd64                     3.0
```

## docker hub上设置
由于docker hub不能后期更改一个image的tag，所以每次更新kubernetes时，都在build settings中，手动增加一个版本对应文件

## 更改tag
```
images=(kube-proxy-amd64:v1.4.0 kube-discovery-amd64:1.0 kubedns-amd64:1.7 kube-scheduler-amd64:v1.4.0 kube-controller-manager-amd64:v1.4.0 kube-apiserver-amd64:v1.4.0 etcd-amd64:2.2.5 kube-dnsmasq-amd64:1.3 exechealthz-amd64:1.1 pause-amd64:3.0)
for imageName in ${images[@]} ; do
  docker pull  sails/$imageName
  docker tag  sails/$imageName gcr.io/google_containers/$imageName
done
```


## 通过kubeadm安装
```
kubeadm init --use-kubernetes-version v1.4.0
```
