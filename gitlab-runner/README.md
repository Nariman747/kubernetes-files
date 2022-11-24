helm repo add gitlab https://charts.gitlab.io \
helm repo update \
mkdir gitlab-runner && cd gitlab-runner \
wget https://raw.githubusercontent.com/Nariman747/kubernetes-files/main/gitlab-runner/values.yaml \

vi secret-gitlab.yml
```
apiVersion: v1 
kind: Secret 
metadata: 
  name: gitlab-runner-token 
  namespace: gitlab-runner 
type: Opaque 
data: 
  runner-registration-token: R1IxMzQ4OTQxaFlDUnh6aFh5bno5V2t2dVdNqkE=
  runner-token: ""
```

Change in values.yaml
```
change:
tags: tag
gitlabUrl: https://repo.p-s.kz/
```

vi fix-sa-forbidden.yaml
```
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: gitlab-runner
  namespace: gitlab-runner

---
apiVersion: rbac.authorization.k8s.io/v1
kind: "ClusterRole"
metadata:
  name: gitlab-runner
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: "ClusterRoleBinding"
metadata:
  name: gitlab-runner
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: "ClusterRole"
  name: gitlab-runner
subjects:
- kind: ServiceAccount
  name: default
  namespace: gitlab-runner
```

```
helm upgrade --create-namespace --install gitlab-runner -f values.yaml gitlab/gitlab-runner -n gitlab-runner
kubectl apply -f secret-gitlab.yml
kubectl apply -f fix-sa-forbidden.yaml
```