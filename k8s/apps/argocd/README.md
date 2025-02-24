# ARGO CD

## Prerequisites
- Kubernetes cluster
- ArgoCD installed on the cluster
- kubectl CLI tool
- Git

## Installation

1. Install ArgoCD in your cluster:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2. Get Argo Secret
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```
