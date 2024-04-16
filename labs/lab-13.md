# Lab 13 - Static Pods

## Documenation

* https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/ (search: daemonset)

## Task 1 - Create a Manifest for a Static Pod

Create a static pod manifest file:
```
sudo vi /etc/kubernetes/manifests/beebox-diagnostic.yml
```

Add the following into the file:
```
apiVersion: v1
kind: Pod
metadata:
  name: beebox-diagnostic
spec:
  containers:
  - name: beebox-diagnostic
    image: acgorg/beebox-diagnostic:1
    ports:
    - containerPort: 80
```

## Task 2 - Start Up the Static Pod

Restart kubelet to start the static pod:
```
sudo systemctl restart kubelet
```

In a new terminal session, log in to the Control Plane Node server:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

Check the status of your static Pod:
```
[cloud_user@k8s-control]]$ kubectl get pods
```

Attempt to delete the static Pod using the k8s API:
```
[cloud_user@k8s-control]]$ kubectl delete pod beebox-diagnostic-k8s-worker1
```

Check the status of the Pod:
```
[cloud_user@k8s-control]]$ kubectl get pods
```

We'll see the Pod was immediately re-created, since it is only a mirror Pod created by the worker kubelet to represent the static Pod.