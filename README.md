az login
az account set --subscription 
az aks get-credentials --resource-group test --name consul1 --overwrite-existing
helm repo add hashicorp https://helm.releases.hashicorp.com
helm install --values values-v1.yaml consul hashicorp/consul --create-namespace --namespace consul --version "1.2.0"
helm status consul --namespace consul
helm get all consul --namespace consul
export CONSUL_HTTP_TOKEN=$(kubectl get --namespace consul secrets/consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)
export CONSUL_HTTP_ADDR=https://$(kubectl get services/consul-ui --namespace consul -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export CONSUL_HTTP_SSL_VERIFY=false
echo $CONSUL_HTTP_ADDR