az login <br>
az account set --subscription <br>
az aks get-credentials --resource-group test --name consul1 --overwrite-existing<br>
helm repo add hashicorp https://helm.releases.hashicorp.com<br>
helm install --values values-v1.yaml consul hashicorp/consul --create-namespace --namespace consul --version "1.2.0"<br>
helm status consul --namespace consul<br>
helm get all consul --namespace consul<br>
export CONSUL_HTTP_TOKEN=$(kubectl get --namespace consul secrets/consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)<br>
export CONSUL_HTTP_ADDR=https://$(kubectl get services/consul-ui --namespace consul -o jsonpath='{.status.loadBalancer.ingress[0].ip}')<br>
export CONSUL_HTTP_SSL_VERIFY=false<br>
echo $CONSUL_HTTP_ADDR<br>
