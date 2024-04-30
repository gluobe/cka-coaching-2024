# Question 19 - Secrets

Create the namespace:
```
kubectl create ns secret
```

Change namepace from `todo` to `secret`:
```
vi /opt/course/19/secret1.yaml
```

Create secret1:
```
kubectl apply -f /opt/course/19/secret1.yaml
```

Create secret2:
```
kubectl create secret generic secret2 -n secret \
    --from-literal=user=user1 \
    --from-literal=pass='1234'
```

Use a pod manifest file with the following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
  namespace: secret
spec:
  volumes:
    - name: secret1
      secret:
        secretName: secret1
  containers:
  - name: secret-pod
    image: busybox:1.31.1
    command:
    - 'sleep'
    - '3600'
    env:
      - name: APP_USER
        valueFrom:
          secretKeyRef:
            name: secret2
            key: user
      - name: APP_PASS
        valueFrom:
          secretKeyRef:
            name: secret2
            key: pass
    volumeMounts:
      - name: secret-volume
        readOnly: true
        mountPath: "/tmp/secret1"
```



