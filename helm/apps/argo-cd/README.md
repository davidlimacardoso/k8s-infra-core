

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

## Throubleshoot

If you encounter the error during the creation of a new application in Argo CD: `"Unable to create application: error creating application: create not allowed while custom resource definition is terminating"`

This issue typically arises when some Argo applications were installed before the Argo server was deleted. To resolve this, you need to remove the last remaining CRD (Custom Resource Definition) related to Argo CD from your Kubernetes cluster.

1. Check Existing CRDs
First, list the CRDs in the argocd namespace:
```
$ kubectl get crd -n argocd

NAME                                         CREATED AT
applications.argoproj.io                     2025-02-21T19:11:39Z
applicationsets.argoproj.io                  2025-02-24T16:32:19Z
appprojects.argoproj.io                      2025-02-24T16:32:20Z

```

2. Remove the CRD
To delete a specific CRD, use the following command:
```
kubectl delete CRD_NAME
```

3. Handle Finalizers (If Necessary)
If the deletion is not occurring, you may need to remove the finalizers associated with your application or the problematic resource. Replace APP_NAME and CRD_NAME with the appropriate names:

```
kubectl patch app APP_NAME -p '{"metadata": {"finalizers": null}}' --type merge
kubectl patch crd CRD_NAME -p '{"metadata": {"finalizers": null}}' --type merge
```