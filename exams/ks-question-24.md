# Question 24 - Network policy

Use the network policy below:
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np-backend
  namespace: project-snake
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
    - Egress
  egress:
    -
      to:
      - podSelector:
          matchLabels:
            app: db1
      ports:
      - protocol: TCP
        port: 1111
    - 
      to:
      - podSelector:
          matchLabels:
            app: db2
      ports:
      - protocol: TCP
        port: 2222
```