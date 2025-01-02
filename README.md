# Kubernetes Auth Config
```
vault write auth/kubernetes/config \
    kubernetes_host=https://kubernetes.default.svc.cluster.local:443 \
    kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
    token_reviewer_jwt=@/var/run/secrets/kubernetes.io/serviceaccount/token
```
# Create Kubernetes Role
```
vault write auth/kubernetes/role/argocd \
    bound_service_account_names=argocd-repo-server \
    bound_service_account_namespaces=argocd \
    policies=argocd-policy \
    ttl=1h
```
# Needed Secret to Connect to Vault via Kubernetes
All data fields need to be base64 encoded.
```
apiVersion: v1
kind: Secret
metadata:
  name: vault-configuration
  namespace: argocd
type: Opaque
data:
  VAULT_ADDR: ""
  AVP_TYPE: ""
  AVP_AUTH_TYPE: ""
  AVP_K8S_ROLE: ""
```
