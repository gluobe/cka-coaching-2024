# Question 18 - Kubelet troubleshooting

Identify faulty node:
```
kubectl get nodes
```

SSH into faulty node:
```
ssh <NODE_NAME>
```

Check kubelet status:
```
systemctl status kubelet
journalctl -u kubelet
```

> NOTE: the kubelet binary location is incorrect

Find correct kubelet path:
```
which kubelet
```

Change binary location:
```
vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

Reload unit file & restart service:
```
systemctl daemon-reload
systemctl restart kubelet
```