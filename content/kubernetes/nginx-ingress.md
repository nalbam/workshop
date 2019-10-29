---
title: nginx-ingress
weight: 12
---

> Nmaespace 를 생성 합니다.

```
kubectl create namespace kube-ingress
```

> nginx-ingress 를 **DaemonSet** 으로 설치 합니다.

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
ELB_DOMAIN=$(kubectl get svc -n kube-ingress -o wide | grep LoadBalancer | grep nginx-ingress-controller | awk '{print $4}')
echo ${ELB_DOMAIN}
```

> ELB 주소를 Route53 에서 원하는 도메인의 CNAME 으로 등록 합니다.

> **mzdev.be** 라는 도메인이 있을때 다음과 같은 방법으로도 등록 할수 있습니다.

```
ROOT_DOMAIN="mzdev.be"
BASE_DOMAIN="demo.${ROOT_DOMAIN}"

# HostedZoneId
HostedZoneId=$(aws route53 list-hosted-zones | ROOT_DOMAIN=${ROOT_DOMAIN}. jq -r '.HostedZones[] | select(.Name==env.ROOT_DOMAIN) | .Id' | cut -d'/' -f3)
echo ${HostedZoneId}

# ELB domain info
ELB_SUB=$(echo ${ELB_DOMAIN} | cut -d'-' -f1)

CanonicalHostedZoneNameID=$(aws elb describe-load-balancers --load-balancer-name a948c3847da3011e993e406e2a4fc6f1 | jq -r '.LoadBalancerDescriptions[] | .CanonicalHostedZoneNameID')
DNSName=$(aws elb describe-load-balancers --load-balancer-name a948c3847da3011e993e406e2a4fc6f1 | jq -r '.LoadBalancerDescriptions[] | .DNSName')
```

```
cat << EOF | aws route53 change-resource-record-sets --hosted-zone-id ${HostedZoneId} --change-batch -
{
  "Changes": [
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "*.${BASE_DOMAIN}",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "${HostedZoneId}",
          "DNSName": "${ELB_DOMAIN}",
          "EvaluateTargetHealth": false
        }
      }
    }
  ]
}
EOF
```
