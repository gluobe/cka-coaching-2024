# Question 09 - Pod scheduling

## Temporarily stop `kube-scheduler`

Issue the following commands:
```
cd /etc/kubernetes/manifests/
mv kube-scheduler.yaml ..
```

## Manually schedule pod

Use the follwoing manifest:
```
apiVersion: v1
kind: Pod
metadata:
  name: manual-schedule
  namespace: default
spec:
  containers:
  - name: manual-schedule
    image: httpd:2.4-alpine
  nodeName: cluster2-master1
```

## Automatically schedule pod

Start `kube-scheduler`:
```
cd /etc/kubernetes/manifests/
mv ../kube-scheduler.yaml .
```

Use the follwoing manifest:
```
apiVersion: v1
kind: Pod
metadata:
  name: manual-schedule
  namespace: default
spec:
  containers:
  - name: manual-schedule
    image: httpd:2.4-alpine
```