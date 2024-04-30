# Question 08 - Core system services

```
kubelet: process
kube-apiserver: static-pod
kube-scheduler: static-pod
kube-controller-manager: static-pod
etcd: static-pod
dns: pod coredns
```

Check if `process`:
```
systemctl | grep -i <SERVICE_NAME>
```

Check if `static-pod`:
```
ls -al /etc/kubernetes/manifests/
```

Check if `pod`, first validate it is not a `static-pod`:
```
kubectl get all -n kube-system
```