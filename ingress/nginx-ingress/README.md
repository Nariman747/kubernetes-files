helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx \
helm repo update \
mkdir ingress && cd ingress \
wget https://raw.githubusercontent.com/Nariman747/kubernetes-files/main/ingress/nginx-ingress/values.yaml \
helm upgrade --create-namespace --install ingress-nginx -f values.yaml ingress-nginx/ingress-nginx -n ingress-nginx