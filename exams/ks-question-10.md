# Question 10 - RBAC

## Create ServiceAccount

Use the following manifest:
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: processor
  namespace: project-hamster
```

## Create Role

Use the follwoing manifest:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: project-hamster
  name: processor
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["configmaps","secrets"]
  verbs: ["create"]
```

## Create RoleBinding

Use the follwoing manifest:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: project-hamster
  name: processor
subjects:
- kind: ServiceAccount
  name: processor
  namespace: project-hamster
roleRef:
  kind: Role
  name: processor
  apiGroup: rbac.authorization.k8s.io
```