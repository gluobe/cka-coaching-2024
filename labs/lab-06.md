# Lab 06 - RBAC

## Documenation

* https://kubernetes.io/docs/reference/access-authn-authz/rbac/ (search: RBAC)

## Task 1 - Check permissions

```
kubectl get pods -n beebox-mobile --kubeconfig dev-k8s-config
```

```
kubectl auth can-i get pod -n beebox-mobile --kubeconfig dev-k8s-config
```

## Task 2 - Create Role

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: beebox-mobile
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "watch", "list"]
```

```
kubectl apply -f pod-reader-role.yml
```

## Task 2 - Create RoleBinding

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
  namespace: beebox-mobile
subjects:
- kind: User
  name: dev
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

```
kubectl apply -f pod-reader-rolebinding.yml
```

## Task 3 - Testing

```
kubectl get pods -n beebox-mobile --kubeconfig dev-k8s-config                 # works!
kubectl logs beebox-auth -n beebox-mobile --kubeconfig dev-k8s-config         # works!
kubectl delete pod beebox-auth -n beebox-mobile --kubeconfig dev-k8s-config   # works!
```

```
kubectl auth can-i get pod -n beebox-mobile --kubeconfig dev-k8s-config
kubectl auth can-i delete pod -n beebox-mobile --kubeconfig dev-k8s-config
```