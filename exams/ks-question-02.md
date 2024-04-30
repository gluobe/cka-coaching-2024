# Question 02 - Schedule pod on a master node

Create a pod manifest file with the following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: pod1
  namespace: default
spec:
  containers:
  - name: pod1-container
    image: httpd:2.4.41-alpine
  nodeSelector:
    node-role.kubernetes.io/control-plane: ""
  tolerations:
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"
```

> NOTE 1: to get labels of a node `kubectl get nodes --show-labels` or `kubectl describe node <NODE_NAME>`
> NOTE 2: we need to add a tolerations, otherwise the pod would not get scheduled (check the taint using `kubectl describe node <NODE_NAME>`)