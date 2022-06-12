# pun-infra

## Try

```console
cat <<EOF | kubectl apply -f -
---
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: pun-fsa
  namespace: flux-system
spec:
  interval: 30s
  url: https://github.com/aorjoa/pun-infra
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: apps-pun-fsa
  namespace: flux-system
spec:
  prune: true
  interval: 2m
  path: "./apps"
  sourceRef:
    kind: GitRepository
    name: pun-fsa
  timeout: 3m
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: pun-fsa
  namespace: flux-system
spec:
  prune: true
  interval: 2m
  path: "./clusters/pun-cluster/flux-system"
  sourceRef:
    kind: GitRepository
    name: pun-fsa
  timeout: 3m
EOF
```
