

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

## Argo CD
In order to access the server UI you have the following options:

1. kubectl port-forward service/argocd-server -n argocd 8080:443

    and then open the browser on http://localhost:8080 and accept the certificate

2. enable ingress in the values file `server.ingress.enabled` and either
      - Add the annotation for ssl passthrough: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-1-ssl-passthrough
      - Set the `configs.params."server.insecure"` in the values file and terminate SSL at your ingress: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-2-multiple-ingress-objects-and-hosts


After reaching the UI the first time you can login with username: admin and the random password generated during the installation. You can find the password by running:

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d