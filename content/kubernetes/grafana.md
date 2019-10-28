---
title: grafana
weight: 32
---

> 환경 변수를 설정 합니다.

```
PASSWORD="password"

SERVICE_TYPE="ClusterIP"
INGRESS_ENABLED=true
INGRESS_DOMAIN="grafana-monitor.demo.nalbam.com"
```

> grafana 을 설치 합니다.

```
cat << EOF | helm upgrade --install grafana stable/grafana --namespace monitor --values -
adminUser: admin
adminPassword: ${PASSWORD}

service:
  type: ${SERVICE_TYPE}

ingress:
  enabled: ${INGRESS_ENABLED}
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    external-dns.alpha.kubernetes.io/hostname: "${INGRESS_DOMAIN}."
  hosts:
    - ${INGRESS_DOMAIN}
  tls:
    - secretName: grafana-tls
      hosts:
        - ${INGRESS_DOMAIN}

env:
  GF_SERVER_ROOT_URL: https://${INGRESS_DOMAIN}

persistence:
  enabled: true
  accessModes:
    - ReadWriteOnce
  size: 5Gi
  storageClassName: "efs"

datasources:
  datasources.yaml:
    apiVersion: 1
    datasources:
      - name: Prometheus
        type: prometheus
        url: http://prometheus-server
        access: proxy
        isDefault: true

dashboardProviders:
  dashboardproviders.yaml:
    apiVersion: 1
    providers:
      - name: "default"
        orgId: 1
        folder: ""
        type: file
        disableDeletion: false
        editable: true
        options:
          path: /var/lib/grafana/dashboards/default

dashboards:
  default:
    kube-cluster:
      url: https://raw.githubusercontent.com/nalbam/kops-cui/master/templates/grafana/kube-cluster.json
      datasource: Prometheus
    kube-deployment:
      url: https://raw.githubusercontent.com/nalbam/kops-cui/master/templates/grafana/kube-deployment.json
      datasource: Prometheus
EOF
```
