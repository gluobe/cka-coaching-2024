# Question 23 - SSL certificates (part 2)

SSH into the node:
```
ssh clustet2-worker1
```

Get certificate location
```
cat /etc/kubernetes/kubelet.conf | grep -i client-certificate
```

Use the commands below:
```
openssl x509 --noout --text -in /var/lib/kubelet/pki/kubelet-client-current.pem | grep -i "Issuer"
openssl x509 --noout --text -in /var/lib/kubelet/pki/kubelet-client-current.pem | grep -i "Extended Key Usage" -A 10

openssl x509 --noout --text -in /var/lib/kubelet/pki/kubelet.crt | grep -i "Issuer"
openssl x509 --noout --text -in /var/lib/kubelet/pki/kubelet.crt | grep -i "Extended Key Usage" -A 10
```