# Argocd
## To create new app

1. create your app deployment files
2. create an application.yaml file for argocd in the same folder
3. push everything to git
4. k apply -f the application.yaml file
5. wait for app to be created

## Create application.yaml

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: mealie
  namespace: argocd #keep this as argocd 
spec:
  project: default
  source:
    repoURL: https://gitlab.jhart.tech/jordan/kub-mkdocs.git
    targetRevision: HEAD
    path: mealie
  destination:
    server: https://kubernetes.default.svc
    namespace: mealie

  syncPolicy:
    #syncOptions:
    #- CreateNamespace=true # create namespace if it doesnt exist 
    automated:
      selfHeal: true
      prune: true
```


## Helm Apps in Argocd
