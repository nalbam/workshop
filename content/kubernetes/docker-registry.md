---
title: docker-registry
weight: 43
---

> 환경 변수를 설정 합니다.

```
SERVICE_TYPE="ClusterIP"
INGRESS_ENABLED=true
INGRESS_DOMAIN="docker-registry-devops.spot.mzdev.be"
```

> docker-registry 을 설치 합니다.

```
cat << EOF | helm upgrade --install docker-registry stable/docker-registry --namespace devops --values -
service:
  type: ${SERVICE_TYPE}

ingress:
  enabled: ${INGRESS_ENABLED}
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: 500m
    ingress.kubernetes.io/proxy-body-size: 500m
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    external-dns.alpha.kubernetes.io/hostname: "${INGRESS_DOMAIN}."
  hosts:
    - ${INGRESS_DOMAIN}
  path: /
  tls:
    - secretName: docker-registry-tls
      hosts:
        - ${INGRESS_DOMAIN}

persistence:
  enabled: true
  accessMode: ReadWriteOnce
  size: 20Gi
  storageClass: "efs"
EOF
```
