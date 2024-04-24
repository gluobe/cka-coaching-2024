# Lab 21 - Troubleshooting a Kubernetes cluster

## Documenation

* https://kubernetes.io/docs/tasks/debug/debug-cluster/ (search: journalctl)

## Task 1 - Determine What is Wrong with the Cluster

Find out which node is having a problem by using:
```
kubectl get nodes
```

Identify if a node is in the **NotReady** state.

Get more information on the node by using:
```
kubectl describe node <NODE_NAME>
```

Look for the Conditions section of the Node Information and find out what is affecting the node's status, causing it to fail.

Log in to the worker 2 node server using the credentials provided:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

Look at the kubelet logs of the worker 2 node by using:
```
sudo journalctl -u kubelet
```

Go to the end of the log by pressing Shift + G and see the error messages stating that kubelet has stopped.

Look at the status of the kubelet status by using:
```
sudo systemctl status kubelet
```

Verify if the kubelet service is running or not.

## Task 2 - Fix the Problem

In order to fix the problem, we need to not only start the server but also enable kubelet to ensure that it continues to work if the server restarts in the future. 

Use clear to clear the service status, and then start and enable kubelet by using:
```
sudo systemctl enable kubelet
sudo systemctl start kubelet
```

Check if kubelet is active by using:
```
sudo systemctl status kubelet
```

Verify that the service is listed as active (running).

Return to the control plane node.

Check if all nodes are now in a **Ready** status by using:
```
kubectl get nodes
```

You may have to wait and try the command a few times before all nodes appear as **Ready**.