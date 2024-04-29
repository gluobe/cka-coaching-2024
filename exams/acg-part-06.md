# Drain the Worker1 Node

Switch to the acgk8s context:
```
kubectl config use-context acgk8s
```

Attempt to drain the worker1 node:
```
kubectl drain acgk8s-worker1
```

Does the node drain successfully?

Override the errors and drain the node:
```
kubectl drain acgk8s-worker1 --delete-local-data --ignore-daemonsets --force
```

Check the status of the exam objectives:
```
./verify.sh
```

# Create a Pod That Will Only Be Scheduled on Nodes with a Specific Label

Add the disk=fast label to the worker2 node:
```
kubectl label nodes acgk8s-worker2 disk=fast
```

Create a YAML file named fast-nginx.yml:
```
vim fast-nginx.yml
```

In the file, paste the following:
```
apiVersion: v1
kind: Pod
metadata:
  name: fast-nginx
  namespace: dev
spec:
  nodeSelector:
    disk: fast
  containers:
  - name: nginx
    image: nginx
```

Create the fast-nginx pod:
```
kubectl create -f fast-nginx.yml
```

Check the status of the pod:
```
kubectl get pod fast-nginx -n dev -o wide
```

Check the status of the exam objectives:
```
./verify.sh
```