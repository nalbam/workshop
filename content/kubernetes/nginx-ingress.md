---
title: nginx-ingress
weight: 12
---

> Nmaespace 를 생성 합니다.

```
kubectl create namespace kube-ingress
```

> nginx-ingress 를 DaemonSet 으로 설치 합니다.

```
cat << EOF | helm upgrade --install nginx-ingress stable/nginx-ingress --namespace kube-ingress --values -
controller:
  kind: DaemonSet
  config:
    use-forwarded-headers: "true"
  service:
    annotations:
      service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "3600"
EOF
```

> 잠시 후, ELB 를 확인 합니다.

```
kubectl get svc -n kube-ingress -o wide | grep LoadBalancer | grep nginx-ingress-controller | awk '{print $4}'
```

> ELB 주소를 Route53 에서 원하는 도메인의 CNAME 로 등록 합니다.
