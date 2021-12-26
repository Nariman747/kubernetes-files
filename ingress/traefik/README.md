helm repo add traefik https://helm.traefik.io/traefik
helm repo update
mkdir ingress && cd ingress
wget https://raw.githubusercontent.com/Nariman747/kubernetes-files/main/ingress/traefik/values.yaml
kubectl create ns traefik && helm install traefik -f values.yaml traefik/traefik -n traefik
