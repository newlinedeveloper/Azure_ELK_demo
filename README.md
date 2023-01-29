# Azure_ELK_demo
Azure Kubernetes ELK stack demo

### Create Azure kubenetes cluster 

```
# Login to Azure
az login

# Create a resource group for the AKS cluster
az group create --name="Veera-elk-demo" --location="eastus"

# Create the AKS cluster
az aks create --resource-group "Veera-elk-demo" --name "DemoCluster" --node-count 2 --generate-ssh-keys

# Configure kubectl to connect to your Kubernetes cluster using the az aks get-credentials command
az aks get-credentials --resource-group "Veera-elk-demo" --name "DemoCluster"

```


### Installation the ELK stack using Helm on the AKS cluster

```

# Add the Elastic Helm chart repository
helm repo add elastic https://helm.elastic.co

# Update the local Helm chart repository
helm repo update

# Create a namespace for the ELK stack
kubectl create namespace monitoring

# Use Helm to install the Elasticsearch chart
helm install elasticsearch ./elasticsearch --namespace monitoring

# Get username and password of Elastic search credentials
kubectl get secrets --namespace=monitoring elasticsearch-master-credentials -ojsonpath='{.data.username}' | base64 -d
kubectl get secrets --namespace=monitoring elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d


# elastic searh port forwarding
kubectl port-forward svc/elasticsearch-master 9200 -n monitoring

#check cluster health
curl -u elastic:<password> https://localhost:9200 -k


# Use Helm to install the filebeat
helm install filebeat elastic/filebeat --namespace monitoring

# Use Helm to install the metricbeat
helm install metricbeat elastic/metricbeat --namespace monitoring

# Get indices of beats
https://localhost:9200/

# Use Helm to install the Logstash chart
helm install logstash elastic/logstash --namespace monitoring

# Use Helm to install the Kibana chart
helm install kibana elastic/kibana --namespace monitoring

#kibana dashboard port forwarding
kubectl port-forward svc/kibana-kibana 5601 -n monitoring

# Check the status of the pods
kubectl get pods --namespace monitoring

# Check the status of the services
kubectl get svc --namespace monitoring

```


### Deploy sample voting app on the AKS cluster

```
# Create a namespace for the Voting App
kubectl create namespace app

# Create a namespace for the application
kubectl apply -f ./azure-vote.yaml --namespace app

# Check the status of the service
kubectl get svc --namespace app

```


### Delete resources

```
# Delete resource group
az group delete --name Veera-elk-demo --yes --no-wait

```



