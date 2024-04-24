# Lab 18 - Ingress

## Documenation

* https://kubernetes.io/docs/concepts/services-networking/ingress/ (search: ingress)

## Task 1 - Create a Service to Expose the web-auth Deployment

Check out the deployment:
```
kubectl get deployment web-auth -o yaml
```

Note the web-auth label on our Pods. We'll use this label to select these Pods using our Service.

Create a web-auth-svc.yml file:
```
vi web-auth-svc.yml
```

Paste the following YAML definitions:
```
apiVersion: v1
kind: Service
metadata:
  name: web-auth-svc
spec:
  type: ClusterIP
  selector:
    app: web-auth
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
```

Create the Service:
```
kubectl create -f web-auth-svc.yml
```

## Task 2 - Create an Ingress That Maps to the New Service

Create a web-auth-ingress.yml file:
```
vi web-auth-ingress.yml
```

Add the following YAML definitions:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-auth-ingress
spec:
  rules:
  - http:
      paths:
      - path: /auth
        pathType: Prefix
        backend:
          service:
            name: web-auth-svc
            port:
              number: 80
```

Create the Ingress:
```
kubectl create -f web-auth-ingress.yml
```

Check the status of the Ingress:
```
kubectl describe ingress web-auth-ingress
```

Note the service endpoints in the Backends section of the output. You have successfully created an Ingress that maps to the backend Service.