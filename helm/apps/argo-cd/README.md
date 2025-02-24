

## Install

```
helm install argocd argo/argo-cd -f helm/apps/argo-cd/values.yaml -n argocd
```

# Installation When Error 

When you have the error below:
`
Error: INSTALLATION FAILED: Unable to continue with install: CustomResourceDefinition "applications.argoproj.io" in namespace "" exists and cannot be imported into the current release: invalid ownership metadata; label validation error: missing key "app.kubernetes.io/managed-by": must be set to "Helm"; annotation validation error: missing key "meta.helm.sh/release-name": must be set to "argocd"; annotation validation error: missing key "meta.helm.sh/release-namespace": must be set to "default"
`

Install this form:

```
helm install argocd argo/argo-cd -f helm/apps/argo-cd/values.yaml --set crds.install=false --set createClusterRoles=false
```