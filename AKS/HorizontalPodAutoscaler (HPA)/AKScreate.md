### Sign in to Azure Cloud Shell with the account you want to deploy resources into.
##### ( In the Cloud Shell window, select Settings > Go to Classic version.)

`export RESOURCE_GROUP=rg-contoso-video`               
`export CLUSTER_NAME=aks-contoso-video`                
`export LOCATION=eastus`               

### Run the az group create command to create a resource group. Deploy all resources into this new resource group.

`az group create --name=$RESOURCE_GROUP --location=$LOCATION`

### Run the az aks create command to create an AKS cluster.

```
az aks create \
--resource-group $RESOURCE_GROUP \
--name $CLUSTER_NAME \
--node-count 2 \
--generate-ssh-keys \
--node-vm-size Standard_B2s \
--enable-app-routing    
```
### --enable-app-routing    # THIS LINE WAS NOT IN THE ORIGINAL COMMAND. I ADDED IT AFTER CHECKING THE "HORIZONTAL POD AUTOSCALER" learning exercises.

### Link your Kubernetes cluster with kubectl by running the following command in Cloud Shell.

`az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP`

### Verify that the cluster is running and that you can connect to it using the "kubectl get nodes" command.

`kubectl get nodes`

### Create the application namespace using the "kubectl create namespace" command

`kubectl create namespace hpa-contoso`

### Deploy the application to the cluster using the "kubectl apply" command

`kubectl apply -f deployment.yaml`

### Create an Azure DNS zone using the "az network dns zone create" command. The following example creates a DNS zone named contoso-website.com:

`az network dns zone create --resource-group $RESOURCE_GROUP --name contoso-website.com`

### Get the resource ID for your DNS zone using the "az network dns zone show" command and save the output to a variable named DNS_ZONE_ID.

`DNS_ZONE_ID=$(az network dns zone show --resource-group $RESOURCE_GROUP --name contoso-website.com --query id --output tsv)`

### Update the application routing cluster add-on to enable Azure DNS integration using the az aks approuting zone command.

`az aks approuting zone add --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --ids=${DNS_ZONE_ID} --attach-zones`

### Create the service resource

`kubectl apply -f service.yaml`

### Update the "INGRESS.YAML" with the DNS ZONE RECENTLY CREATED.

### Deploy the ingress resource to the cluster using the "kubectl apply" command

`kubectl apply -f ingress.yaml`

### Create a HorizontalPodAutoscaler 

`kubectl apply -f hpa.yaml`

### Check the results - Query the metrics and usage of the HPA using the "kubectl get hpa" command

`kubectl get hpa --namespace hpa-contoso`

### Check the REPLICAS

`kubectl get rs -n hpa-contoso`