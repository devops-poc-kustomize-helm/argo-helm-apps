# 🚀 Argo Helm Apps — ArgoCD Deployment

This repository contains a **parent Helm chart** (`argo-helm-apps`) that deploys multiple ArgoCD Applications using the **App-of-Apps pattern**.  
Each application is defined via a Helm sub-chart and synchronized automatically by ArgoCD.

---

## 📁 Directory Structure

```

argo-helm-apps/
├── Chart.yaml
├── templates/
│   └── applications.yaml
├── values-dev.yaml
├── values-test.yaml
└── README.md
helm-template/
├── Chart.yaml
├── templates/
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   └── service.yaml
└── values.yaml

````

- **argo-helm-apps**: Parent Helm chart managing multiple ArgoCD Applications  
- **helm-template**: Base Helm chart for individual applications  

---

## 🧩 ArgoCD Application Definitions

These are the ArgoCD **Application manifests** to deploy all apps for **dev** and **test** environments.

### Dev Environment

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argo-helm-apps-dev
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/your-org/argo-helm-apps.git
    targetRevision: HEAD
    path: argo-helm-apps
    helm:
      valueFiles:
        - values-dev.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: dev
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
````

### Test Environment

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argo-helm-apps-test
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/your-org/argo-helm-apps.git
    targetRevision: HEAD
    path: argo-helm-apps
    helm:
      valueFiles:
        - values-test.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: test
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

---

## 📦 Usage

1. Clone the repository:

   ```bash
   git clone https://github.com/your-org/argo-helm-apps.git
   cd argo-helm-apps
   ```

2. Preview Helm manifests locally:

   ```bash
   helm template . -f values-dev.yaml
   helm template . -f values-test.yaml
   ```

3. Deploy via ArgoCD by creating Applications using the YAML snippets above.

---
