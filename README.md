### Create private and public key
```shell
age-keygen -o age.agekey
```

```yaml
# .sops.yaml
creation_rules:
  - path_regex: .*.ya?ml
    encrypted_regex: ^(data|stringData)$
    age: Paste it here!

```

```shell
cat age.agekey | kubectl create secret generic sops-age --namespace=flux-system --from-file=age.agekey=/dev/stdin
```

---
### SOPS Example

```yaml
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
    name: secret-name
    namespace: namespace
type: Opaque
data:
  KEY: BASE64_ENCODED_VALUE
```

```shell
# encrypt
sops -e -i secret.yaml
```

```shell
# decrypt
sops -i -d secret.yaml
```

---

### Installation
```shell
flux bootstrap github
    --token-auth
    --owner=masterbpro
    --repository=iac
    --branch=main
    --path=./kubernetes/flux
    --components-extra=image-reflector-controller,image-automation-controller
    --version=latest
    --personal
```
