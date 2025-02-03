# non-admin-api-allow.yaml
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: front-end
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: back-end-api
  egress:
  - to:
    - podSelector:
        matchLabels:
          role: back-end-api
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-admin-frontend-backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: admin-front-end
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: admin-back-end-api
  egress:
  - to:
    - podSelector:
        matchLabels:
          role: admin-back-end-api
