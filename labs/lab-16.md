# Lab 16 - Services

## Documenation

* https://kubernetes.io/docs/concepts/services-networking/service/ (search: services)

## Task 1 - Expose the Pods from the User-db Deployment as an Internal Service

Examine the properties of the user-db deployment by using the command below:
```
kubectl get deployment user-db -o yaml
```

In the deployment properties, find the spec and look for the Pod template, paying particular attention to the labels, especially the label app: user-db.

Take note of which port(s) are exposed.

Start creating a Service that will expose its Pods to other components within the cluster by using vi user-db-svc.yml.

Define the Service by using:
```
apiVersion: v1 
kind: Service 
metadata: 
  name: user-db-svc 
spec: 
  type: ClusterIP 
  selector: 
    app: user-db 
  ports: 
  - protocol: TCP 
    port: 80 
    targetPort: 80
```

Finish creating the Service by using the command below:
```
kubectl create -f user-db-svc.yml
```

Test the Service to make sure it works by using the command below:
```
kubectl exec busybox -- curl user-db-svc
```

It may not work right away, so you may need to wait a few minutes and try again, but you should be able to see the Nginx welcome page.

## Task 2 - Expose the Pods from the Web-frontend Deployment as an External Service

Now, examine the properties of the frontend deployment by using the command below:
```
kubectl get deployment web-frontend -o yaml
```

Check the labels applied to the Pod template. You should see the label `app=web-frontend`.

Take note of which port(s) are exposed.

Start creating a Service that will expose its Pods on port 30080 of each cluster node by using vi web-frontend-svc.yml.

Define the Service by using:
```
apiVersion: v1 
kind: Service 
metadata: 
  name: web-frontend-svc 
spec: 
  type: NodePort 
  selector: 
    app: web-frontend 
  ports: 
  - protocol: TCP 
    port: 80 
    targetPort: 80 
    nodePort: 30080
```

Finish creating the Service by issuing the command below:
```
kubectl create -f web-frontend-svc.yml
```

Test the Service from outside the cluster (such as in a browser) by navigating to http://<PUBLIC_IP_ADDRESS>:30080.