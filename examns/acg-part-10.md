# Determine What Is Wrong with the Broken Node

Switch to the acgk8s context:
```
kubectl config use-context acgk8s
```

Check the current status of the nodes:
```
kubectl get nodes
```

Identify the broken node and save the name to a file named broken-node.txt:
```
echo acgk8s-worker2 > /k8s/0004/broken-node.txt
```

Attempt to determine the cause of the issue:
```
kubectl describe node acgk8s-worker2
```

# Fix the Problem

Access the node using ssh:
```
ssh acgk8s-worker2
```

Check the kubelet log:
```
sudo journalctl -u kubelet
```

Check the last entry in the log.

Check the kubelet status:
```
sudo systemctl status kubelet
```

Enable kubelet:
```
sudo systemctl enable kubelet
```

Start kubelet:
```
sudo systemctl start kubelet
```

Verify that kubelet started successfully:
```
sudo systemctl status kubelet
```

Return to the control plane node:
```
exit
```

Check the status of the nodes:
```
kubectl get nodes
```

Check the status of the exam objectives:
```
./verify.sh
```