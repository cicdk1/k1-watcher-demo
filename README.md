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
