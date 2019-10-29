---
title: helm tiller
weight: 10
---

> ServiceAccount 와 ClusterRoleBinding 을 생성 합니다.

```
cat << EOF | kubectl apply -n kube-system -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin:kube-system:tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: tiller
  namespace: kube-system
EOF
```

> helm 을 설치 합니다.

```
helm init --upgrade --service-account=tiller
```
