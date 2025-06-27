# kubernetes-argocd-angular-javasprintboot

pour que sa fonctione je doit creer un namespace feature dans argo c est tous et lancer le manifeste et m'assure d'etre sur la branche main

```yaml
kubectl apply -f - <<EOF
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: angular-new-project
  namespace: argocd
spec:
  description: Projet Angular + Java Spring Boot
  clusterResourceWhitelist:
  - group: ""
    kind: '*'
  destinations:
  - namespace: feature
    server: https://kubernetes.default.svc
  namespaceResourceWhitelist:
  - group: ""
    kind: '*'
  - group: "apps"
    kind: "Deployment"
  sourceRepos:
  - https://github.com/stevymonkam/argo-kubernetes-angular-spring-qui-fonctione-bien-pour-mes-presentation.git
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: angular-new-app
  namespace: argocd
spec:
  project: angular-new-project
  source:
    repoURL: https://github.com/stevymonkam/argo-kubernetes-angular-spring-qui-fonctione-bien-pour-mes-presentation.git
    targetRevision: HEAD
    path: .
  destination:
    server: https://kubernetes.default.svc
    namespace: feature
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
EOF
```
