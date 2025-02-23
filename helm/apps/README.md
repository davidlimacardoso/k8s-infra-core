# Helm Applications

## Create Template 

```bash
helm create conversao-temperatura
```

## Debug 

```bash
helm install conversao-temperatura ./helm/apps/conversao-temperatura/ --debug --dry-run
```

## Install 

```bash
helm install conversao-temperatura ./helm/apps/conversao-temperatura/ 
```

## List

```bash
helm list
```

# Test Upgrade Tag Version Image

```
helm upgrade conversao-temperatura ./helm/apps/conversao-temperatura/ --set app.image.tag=v2
```

# Test Forward Port

```bash
kubectl port-forward service/conversao-temperatura 3000:80 -n conversao
```

# Hystory Deploys
```bash
helm history conversao-temperatura
```
output:
```
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Sun Feb 23 11:37:32 2025        superseded      conversao-distancia-0.1.0       1.16.0          Install complete
2               Sun Feb 23 11:49:18 2025        deployed        conversao-distancia-0.1.0       1.16.0          Upgrade complete
```

# Rollback Deploy

```bash
helm rollback conversao-temperatura 1
```

# Uninstall

```bash
helm uninstall conversao-temperatura
```

# Install Dependencies

```bash
helm dependency build ./helm/apps/conversao/teste-conversao-dependencies
```
