# Edit the web-frontend Deployment to Expose the HTTP Port

Switch to the appropriate context with kubectl:
```
kubectl config use-context acgk8s
```

Edit the web-frontend deployment in the web namespace:
```
kubectl edit deployment -n web web-frontend
```

Change the Pod template to expose port 80 on our NGINX containers:
```
spec:
  containers:
  - image: nginx:1.14.2
    ports:
    - containerPort: 80
```

# Create a Service to Expose the web-frontend Deployment's Pods Externally

Open a web-frontend service file:
```
vi web-frontend-svc.yml
```

Define the service in the YAML document:
```
apiVersion: v1
kind: Service
metadata:
  name: web-frontend-svc
  namespace: web
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

Press Esc and enter :wq to save and exit.

Create the service:
```
kubectl create -f web-frontend-svc.yml
```

# Scale Up the Web Frontend Deployment

Scale up the deployment:
```
kubectl scale deployment web-frontend -n web --replicas=5
```

> NOTE: `--record`

# Create an Ingress That Maps to the New Service

Create a web-frontend-ingress file:
```
vi web-frontend-ingress.yml
```

Define an Ingress in the YAML document:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-frontend-ingress
  namespace: web
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-frontend-svc
            port:
              number: 80
```

Press Esc and enter :wq to save and exit.

Create the Ingress:
```
kubectl create -f web-frontend-ingress.yml
```