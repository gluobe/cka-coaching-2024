# Question 22 - SSL certificates

## OpenSSL

SSH into the node:
```
ssh cluster2-master1
```

Use the following command:
```
openssl x509 --noout --text -in /etc/kubernetes/pki/apiserver.crt | grep Validity -A 2
```

## kubeadm

Use the following command:
```
kubeadm certs check-expiration
```

## Renew apiserver certs

Use the command below:
```
kubeadm certs renew apiserver
```