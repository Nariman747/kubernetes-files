helm repo add gitlab https://charts.gitlab.io
helm repo update
wget https://raw.githubusercontent.com/Nariman747/kubernetes-files/main/gitlab-runner/values.yaml

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

helm upgrade --create-namespace --install gitlab-runner -f values.yaml gitlab/gitlab-runner -n gitlab-runner
kubectl apply -f secret-gitlab.yml