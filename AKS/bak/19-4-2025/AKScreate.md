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

### Run the az aks nodepool add command to add another node pool that uses the default Linux operating system

```
az aks nodepool add \
--resource-group $RESOURCE_GROUP \
--cluster-name $CLUSTER_NAME \
--name userpool --node-count 2 \
--node-vm-size Standard_B2s
```
### Link your Kubernetes cluster with kubectl by running the following command in Cloud Shell.

`az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP`

### Run the kubectl get nodes command to check that you can connect to your cluster, and confirm its configuration

`kubectl get nodes`

```diff
- output sample:
- NAME                                STATUS   ROLES   AGE    VERSION
- aks-nodepool1-21895026-vmss000000   Ready    agent   245s   v1.23.12
- aks-nodepool1-21895026-vmss000001   Ready    agent   245s   v1.23.12
- aks-userpool-21895026-vmss000000    Ready    agent   105s   v1.23.12
- aks-userpool-21895026-vmss000001    Ready    agent   105s   v1.23.12
```
