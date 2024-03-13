# Lab 02 - Kubernetes Namespaces

## Documentaton

* https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/ (search: namespaces)

## Task 1 - Create dev namespace

```
kubectl create namespace dev
```

or

```
kubectl create ns dev
```

or

```
cat <<EOF | sudo tee dev-namespace.yml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
EOF
```

```
kubectl apply -f dev-namespace.yml
```

## Task 2 - Create file with all namespaces

```
kubectl get namespace > namespaces.txt
```

```
cat namespaces.txt
```

### Task 3 - Find namespace where `quark` pod is running in

```
kubectl get pod --all-namespaces | grep quark
```

or

```
kubectl get po -A | grep quark
```