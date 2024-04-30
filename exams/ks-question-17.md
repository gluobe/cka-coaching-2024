# Question 17 - crictl

Use a pod manifest file with the following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: tigers-reunite
  namespace: default
  labels:
    pod: container
    container: pod
spec:
  containers:
  - name: tigers-reunite
    image: httpd:2.4.41-alpine
```

Get the node where the pod is running on:
```
kubectl get pods -n default -o wide
```

SSH into the node and use crictl to get container info:
```
ssh <NODE_NAME>
crictl ps
crictl logs <CONTAINER_ID>
```