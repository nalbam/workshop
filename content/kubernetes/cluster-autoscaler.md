---
title: cluster-autoscaler
weight: 24
---

> 환경 변수를 설정 합니다.

```
REGION="ap-northeast-2"
CLUSTER_NAME="workshop-eks"
```

> cluster-autoscaler 을 설치 합니다.

```
cat << EOF | helm upgrade --install cluster-autoscaler stable/cluster-autoscaler --namespace kube-system --values -
awsRegion: ${REGION}

autoDiscovery:
  enabled: true
  clusterName: ${CLUSTER_NAME}

extraArgs:
  v: 4
  stderrthreshold: info
  logtostderr: true
  expander: random
  scale-down-enabled: true
  skip-nodes-with-local-storage: false
  skip-nodes-with-system-pods: false

sslCertPath: /etc/ssl/certs/ca-bundle.crt

rbac:
  create: true
  pspEnabled: true
EOF
```
