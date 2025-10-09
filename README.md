# Argo Helm Apps

This repository contains a **parent Helm chart** for deploying multiple ArgoCD Applications using the **App-of-Apps pattern**.  
Each application is managed via a Helm chart and synchronized automatically by ArgoCD.

---

## Current Application

The repository already includes the following ArgoCD Application:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argo-helm-apps-test
  namespace: argocd
spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  source:
    path: argo-apps
    repoURL: https://github.com/devops-poc-kustomize-helm/argo-helm-apps.git
    targetRevision: HEAD
    helm:
      valueFiles:
        - values-test.yaml
  sources: []
  project: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
