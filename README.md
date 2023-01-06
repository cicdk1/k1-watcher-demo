# k1-watcher-demo

- Exploring watcher events and argo


# Sample Kubefirst Local

```yaml 
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: watcher-sample
spec:
  generators:
  - list:
      elements:
        - cluster: sample
          url: https://kubernetes.default.svc
          env: k1
  template:
    metadata:
      name: 'k1-watcher-demo'
    spec:
      project: default
      source:
        repoURL: https://github.com/6za/k1-watcher-demo.git
        targetRevision: HEAD
        path: watcher
      destination:
        server: '{{url}}'
        namespace: watcher-system
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
          - CreateNamespace=true
        retry:
          limit: 5
          backoff:
            duration: 5s
            maxDuration: 5m0s
            factor: 2   
```

# Sample Kubefirst AWS-GITHUB

```yaml 
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: watcher-sample
spec:
  generators:
  - list:
      elements:
        - cluster: sample
          url: https://kubernetes.default.svc
          env: k1
  template:
    metadata:
      name: 'k1-watcher-demo'
    spec:
      project: default
      source:
        repoURL: https://github.com/6za/k1-watcher-demo.git
        targetRevision: HEAD
        path: watcher-aws-github
      destination:
        server: '{{url}}'
        namespace: watcher-system
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
          - CreateNamespace=true
        retry:
          limit: 5
          backoff:
            duration: 5s
            maxDuration: 5m0s
            factor: 2   
```


# Sample Kubefirst AWS-GITLAB

```yaml 
apiVersion: v1
kind: Secret
metadata:
  name: argocd-notifications-secret
  namespace: default
  annotations:
    argocd.argoproj.io/hook: PreSync
type: Opaque
data:
  slack-token: <SLACK_TOKEN_BASE64>
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: default-rbac
  annotations:
    argocd.argoproj.io/hook: PreSync
subjects:
  - kind: ServiceAccount
    # Reference to upper's `metadata.name`
    name: default
    # Reference to upper's `metadata.namespace`
    namespace: default
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: watcher-sample
spec:
  generators:
  - list:
      elements:
        - cluster: sample
          url: https://kubernetes.default.svc
          env: k1
  template:
    metadata:
      name: 'k1-watcher-demo'
    spec:
      project: default
      source:
        repoURL: https://github.com/cicdk1/k1-watcher-demo.git
        targetRevision: HEAD
        path: watcher-aws-gitlab
      destination:
        server: '{{url}}'
        namespace: watcher-system
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
          - CreateNamespace=true
        retry:
          limit: 5
          backoff:
            duration: 5s
            maxDuration: 5m0s
            factor: 2
```
