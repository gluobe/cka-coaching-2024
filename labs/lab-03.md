# Lab 03 - k8s cluster upgrade (kubeadm)

## Documenation

* https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/ (search: kubeadm upgrade clusters)
* https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/upgrading-linux-nodes/ (search: kubeadm upgrade linux nodes)

## Step 1 - Upgrading control plane nodes

### Task 1.0 - Add correct repositories

```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

> NOTE: for additional details: https://forum.linuxfoundation.org/discussion/864693/the-repository-http-apt-kubernetes-io-kubernetes-xenial-release-does-not-have-a-release-file

### Task 1.1 - Upgrading kubeadm package

```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.28.1-*' && \
sudo apt-mark hold kubeadm
```

### Task 1.2 - Verify kubeadm version

```
kubeadm version
```

### Task 1.3 - Plan the upgrade

```
sudo kubeadm upgrade plan
```

### Task 1.4 - Upgrade cluster

```
sudo kubeadm upgrade apply v1.28.1
```

### Task 1.5 - Drain the node

```
kubectl drain k8s-control --ignore-daemonsets
```

### Task 1.6 - Upgrade kubelet & kubectl

```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.28.1-*' kubectl='1.28.1-*' && \
sudo apt-mark hold kubelet kubectl
```

```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

### Task 1.7 - Uncordon the node

```
kubectl uncordon k8s-control
```

### Task 1.8 - Verify version

```
kubectl get nodes
```

## Step 2 - Upgrading worker nodes

### Task 2.0 - Add correct repositories

```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

> NOTE: for additional details: https://forum.linuxfoundation.org/discussion/864693/the-repository-http-apt-kubernetes-io-kubernetes-xenial-release-does-not-have-a-release-file

### Task 2.1 - Upgrading kubeadm package

```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.28.1-*' && \
sudo apt-mark hold kubeadm
```

### Task 2.2 - Verify kubeadm version

```
kubeadm version
```

### Task 2.3 - Upgrade node

```
sudo kubeadm upgrade node
```

### Task 2.4 - Drain the node

```
kubectl drain k8s-worker1 --ignore-daemonsets
```

### Task 2.5 - Upgrade kubelet & kubectl

```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.28.1-*' kubectl='1.28.1-*' && \
sudo apt-mark hold kubelet kubectl
```

```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

### Task 2.6 - Uncordon the node

```
kubectl uncordon k8s-worker1
```

### Task 2.7 - Verify version

```
kubectl get nodes
```