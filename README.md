# Azure_ELK_demo
Azure Kubernetes ELK stack demo

### Create Azure kubenetes cluster 

```
# Login to Azure
az login

# Set the subscription you want to use
az account set --subscription="SUBSCRIPTION_ID"

# Create a resource group for the AKS cluster
az group create --name="myResourceGroup" --location="eastus"

# Create the AKS cluster
az aks create --resource-group "myResourceGroup" --name "myAKSCluster" --node-count 2 --generate-ssh-keys

```


### Installation the ELK stack using Helm on the AKS cluster

```

# Add the Elastic Helm chart repository
helm repo add elastic https://helm.elastic.co

# Update the local Helm chart repository
helm repo update

# Create a namespace for the ELK stack
kubectl create namespace logging

# Use Helm to install the Elasticsearch chart
helm install elasticsearch elastic/elasticsearch --namespace logging

# Use Helm to install the Logstash chart
helm install logstash elastic/logstash --namespace logging

# Use Helm to install the Kibana chart
helm install kibana elastic/kibana --namespace logging

# Check the status of the pods
kubectl get pods --namespace logging

# Check the status of the services
kubectl get svc --namespace logging


```



