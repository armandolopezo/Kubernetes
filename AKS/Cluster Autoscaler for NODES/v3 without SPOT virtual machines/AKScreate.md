### Sign in to Azure Cloud Shell with the account you want to deploy resources into.
##### ( In the Cloud Shell window, select Settings > Go to Classic version.)

`export RESOURCE_GROUP=rg-contoso-video`               
`export CLUSTER_NAME=aks-contoso-video`                
`export LOCATION=eastus`               

### Run the az group create command to create a resource group. Deploy all resources into this new resource group.

`az group create --name=$RESOURCE_GROUP --location=$LOCATION`

### Run the az aks create command to create an AKS cluster.

###

```
az aks create \
--resource-group $RESOURCE_GROUP \
--name $CLUSTER_NAME \
--node-count 1 \
--generate-ssh-keys \
--node-vm-size Standard_B2S \
--enable-app-routing \
--vm-set-type VirtualMachineScaleSets \
--load-balancer-sku standard \
--enable-cluster-autoscaler \
--min-count 1 \
--max-count 3 
```
### PREVIOUS CONFIGURATION:
### ```

### az aks create \

### --resource-group $RESOURCE_GROUP \

### --name $CLUSTER_NAME \

### --node-count 1 \

### --generate-ssh-keys \

### --node-vm-size Standard_DS2_v2 \

### --enable-app-routing    

### ```

### --enable-app-routing    # THIS LINE WAS NOT IN THE ORIGINAL COMMAND. I ADDED IT AFTER CHECKING THE "HORIZONTAL POD AUTOSCALER" learning exercises.

### Update the cluster autoscaler profile using the az aks update ### command with the --cluster-autoscaler-profile flag

`az aks update --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME  --cluster-autoscaler-profile scan-interval=5s scale-down-unready-time=5m scale-down-delay-after-add=5m`

### Link your Kubernetes cluster with kubectl by running the following command in Cloud Shell.

`az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP`

### Verify that the cluster is running and that you can connect to it using the "kubectl get nodes" command.

`kubectl get nodes`

### Run the az aks nodepool add command to add another node pool 

`az aks nodepool add \
    --resource-group $RESOURCE_GROUP \
    --cluster-name $CLUSTER_NAME \
    --name batch200 \
    --enable-cluster-autoscaler \
    --max-count 3 \
    --min-count 1 \
    --eviction-policy Delete \
    --node-vm-size Standard_B2S \
    --no-wait`

### Previous configuration for uUSER NODE POOL: 

###    `az aks nodepool add \

###    --resource-group $RESOURCE_GROUP \

###    --cluster-name $CLUSTER_NAME \

###    --name batch200 \

###    --enable-cluster-autoscaler \

###    --max-count 3 \

###    --min-count 1 \

###    --eviction-policy Delete \

###    --node-vm-size Standard_DS2_v2 \

###    --no-wait` 


### Checking the NODEPOOLS

`az aks nodepool list  --resource-group $RESOURCE_GROUP  --cluster-name $CLUSTER_NAME`

### Run the az aks nodepool show command to show the details of the new spot node pool for the batch-processing service

`az aks nodepool show \
    --resource-group $RESOURCE_GROUP \
    --cluster-name $CLUSTER_NAME \
    --name batch200`

### Create the application namespace using the "kubectl create namespace" command

`kubectl create namespace costsavings`

### Deploy the application to the cluster using the "kubectl apply" command

`kubectl apply -f deployment.yaml`

### Create an Azure DNS zone using the "az network dns zone create" command. The following example creates a DNS zone named contoso-website.com:

### Check DEPLOYMENT

`kubectl get deployment -n costsavings`

### DNS ZONE creation for website name resolution.

`az network dns zone create --resource-group $RESOURCE_GROUP --name contoso-website.com`

### Get the resource ID for your DNS zone using the "az network dns zone show" command and save the output to a variable named DNS_ZONE_ID.

`DNS_ZONE_ID=$(az network dns zone show --resource-group $RESOURCE_GROUP --name contoso-website.com --query id --output tsv)`

### Update the application routing cluster add-on to enable Azure DNS integration using the az aks approuting zone command.

`az aks approuting zone add --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --ids=${DNS_ZONE_ID} --attach-zones`

### Create the service resource

`kubectl apply -f service.yaml`

# Check service for KUBERNETES (system mode)

`kubectl get service`

# Check service for CONTOSO-WEBSITE (user mode)

`kubectl get service -n costsavings`

### Deploy the ingress resource to the cluster using the "kubectl apply" command

`kubectl apply -f ingress.yaml`

### Check INGRESS in namespace COSTSAVINGS

`kubectl get ingress -n costsavings`

### Create a HorizontalPodAutoscaler 

`kubectl apply -f hpa.yaml`

### Check the results - Query the metrics and usage of the HPA using the "kubectl get hpa" command

`kubectl get hpa --namespace costsavings`

### Check the REPLICAS

`kubectl get rs -n costsavings`

### Check the pods

`kubectl get pods -n costsavings`

### Check the NODE POOLS

`az aks nodepool list --resource-group $RESOURCE_GROUP --cluster-name $CLUSTER_NAME --output table`

### Check the nodes in the user mode pool

`kubectl get nodes -l agentpool=batch200`

### Check the nodes in the SYSTEM mode pool

`kubectl get nodes -l agentpool=nodepool1`

### to Check the nodes details or description

`kubectl describe node <NodeName>`

`kubectl describe node aks-nodepool1-31272769-vmss000000`


### to Check other POD information

`kubectl describe pods -n costsavings` 

### to Check other DETAILS in deployments like CPU AND RAM requests, limits. Nodes , some scaling events.

`kubectl describe  deployment -n costsavings`