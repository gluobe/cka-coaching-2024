# Create a Service Account

Switch to the appropriate context with kubectl:
```
kubectl config use-context acgk8s
```

Create a service account:
```
kubectl create sa webautomation -n web
```

# Create a ClusterRole That Provides Read Access to Pods

Create a pod-reader.yml file:
```
vi pod-reader.yml
```

Define the ClusterRole:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

Press Esc and enter:wq to save and exit.

Create the ClusterRole:
```
kubectl create -f pod-reader.yml
```

# Bind the ClusterRole to the Service Account to Only Read Pods in the web Namespace

Create the rb-pod-reader.yml file:
```
vi rb-pod-reader.yml
```

Define the RoleBinding:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: rb-pod-reader
  namespace: web
subjects:
- kind: ServiceAccount
  name: webautomation
roleRef:
  kind: ClusterRole
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

Press Esc and enter:wq to save and exit.

Create the RoleBinding:
```
kubectl create -f rb-pod-reader.yml
```

Verify the RoleBinding works:
```
kubectl get pods -n web --as=system:serviceaccount:web:webautomation
```