# Question 21 - Static Pod & NodePort

## Create static pod

Create manifest:
```
ssh cluster3-master1
vi /etc/kubernetes/manifests/question21.yaml
```

With following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: my-static-pod
  namespace: default
  label:
    app: my-static-pod
spec:
  containers:
  - name: my-static-pod
    image: nginx:1.16-alpine
    resources:
      requests:
        cpu: 10m
        memory: 20Mi
```

## Create NodePort Service

Use manifest below:
```
apiVersion: v1
kind: Service
metadata:
  name: static-pod-service
  namespace: default
spec:
  type: NodePort
  selector:
    app: my-static-pod
  ports:
    - port: 80
      targetPort: 80
```