### Sign in to Azure Cloud Shell with the account you want to deploy resources into.
##### ( In the Cloud Shell window, select Settings > Go to Classic version.)

`export RESOURCE_GROUP=rg-contoso-video`               
`export CLUSTER_NAME=aks-contoso-video`                
`export LOCATION=westus3`               

### Run the az group create command to create a resource group. Deploy all resources into this new resource group.

`az group create --name=$RESOURCE_GROUP --location=$LOCATION`

### Run the az aks create command to create an AKS cluster.

### 

```
az aks create \
--resource-group $RESOURCE_GROUP \
--name $CLUSTER_NAME \
--node-count 2 \
--generate-ssh-keys \
--node-vm-size Standard_B2S \
--enable-app-routing \
--vm-set-type VirtualMachineScaleSets \
--load-balancer-sku standard \
--enable-cluster-autoscaler \
--min-count 2 \
--max-count 2 
```

### Link your Kubernetes cluster with kubectl by running the following command in Cloud Shell.

`az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP`

### Verify that the cluster is running and that you can connect to it using the "kubectl get nodes" command.

`kubectl get nodes`

### AKS cluster system node to force POD REPLICAS to use NODEPOOL=nodepool2 (creation below)
 
### Cordon the existing SYSTEM nodes: Cordoning marks specified nodes as unschedulable and prevents any more pods from being added to the nodes.

`kubectl cordon <node-names>`

### example below:

### kubectl cordon aks-nodepool

### Run the az aks nodepool add command to add another node pool 

`az aks nodepool add \
    --resource-group $RESOURCE_GROUP \
    --cluster-name $CLUSTER_NAME \
    --name nodepool2 \
    --enable-cluster-autoscaler \
    --max-count 3 \
    --min-count 1 \
    --eviction-policy Delete \
    --node-vm-size Standard_A2_V2 \
    --no-wait`

### Checking the NODEPOOLS

`az aks nodepool list  --resource-group $RESOURCE_GROUP  --cluster-name $CLUSTER_NAME`




### Run the az aks nodepool show command to show the details of the new spot node pool for the USER WORKLOADS

`az aks nodepool show \
    --resource-group $RESOURCE_GROUP \
    --cluster-name $CLUSTER_NAME \
    --name nodepool2`

### Create the application namespace using the "kubectl create namespace" command

`kubectl create namespace costsavings`

### Deploy the application to the cluster using the "kubectl apply" command

`kubectl apply -f deployment.yaml`

#### Check DEPLOYMENT

`kubectl get deployment -n costsavings`

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

`kubectl get pods -n costsavings -o wide|more`

### Check the NODE POOLS

`az aks nodepool list --resource-group $RESOURCE_GROUP --cluster-name $CLUSTER_NAME --output table`

### Check the nodes in the user mode pool

`kubectl get nodes -l agentpool=nodepool2`

### Check the nodes in the SYSTEM mode pool

`kubectl get nodes -l agentpool=nodepool1`

### to Check the nodes details or description

`kubectl describe node <NodeName>`

### Example below

`kubectl describe node aks-nodepool1-31272769-vmss000000`


### to Check other POD information

`kubectl describe pods -n costsavings` 

### Ver los detalles de los PODS o los PODS dentro de los NODES con los siguientes commandos
`kubectl get pods -n costsavings -o wide|more`
`kubectl get pods --all-namespaces -o wide`
`kubectl get pods -o wide`
`kubectl get pods --all-namespaces`


### to Check other DETAILS in deployments like CPU AND RAM requests, limits. Nodes , some scaling events.

`kubectl describe  deployment -n costsavings`

### Antes de aplicar CORDON - NODES en las siguientes lineas pude observar SCALE DOWN del NODEPOOL2 que empezó con 3 nodos y como
### tarde en aumentar la carga entre 15 minutos a 1 hora observé como se borraron 2 de los 3 VIRTUAL MACHINE SCALE SETS
### por la falta de carga de CONTOSO WEB-SITE. Para ver esto me metí en las herramientas GRAFICAS del portal de AZURE 
### (aunque también se puede hacer con KUBECTL LOGS) busque los eventos AKS y FILTRÉ LOS EVENTOS CON SOURCE = cluster-autoscaler
### y pude observar eventos DE SCALE DOWN Y ELIMINACION DE 2 NODOS porque un sólo NODO CON UN POD PODIA MANEJAR LA CARGA DE LA APLICACION


### ( 2-5-2025) Cordon unique AKS cluster system node to force POD REPLICAS to use NODEPOOL=nodepool2userworkload 
 
### Cordon the existing SYSTEM nodes: Cordoning marks specified nodes as unschedulable and prevents any more pods from being added to the nodes.

`kubectl cordon <node-names>`

### example below:

### kubectl cordon aks-nodepool1-31721111-vmss000000 aks-nodepool1-31721111-vmss000001 aks-nodepool1-31721111-vmss000002

### Después del cambio de nodo el siguiente comando debe decir "SCHEDULING DISABLED" en el node que se aplico Cordon
### con lo anterior logro que si se requieren nuevos PODS sólo TENGA el NODEPOOL2 disponible para probar CLUSTER autoscaler.
### Si hice lo anterior y tengo un sólo nodo disponible en NODEPOOL2 (como sucedio por inactividad después de SCALE DOWN)
### puede genera replicas de los PODS con EL COMANDO DE KUBECTL SCALE o generar carga con APACHE JMETER para CREAR MAS NODOS CON SCALE UP

`kubectl get nodes`


### Below I create 100 forcing creation of REPLICAS to INDUCE CLUSTER AUTOSCALING with de creation of more NODES to service app: CONTOSO-website

`kubectl scale --replicas=100 deploy contoso-website -n costsavings`

### Check the REPLICAS

`kubectl get rs -n costsavings`

### Después del comando anterior deben aparecer algunos PODS RUNNING y otros PENDING como se puede ver con el siguiente comando:

`kubectl get pods -n costsavings -o wide|more`

### With the following command below I could do a SCALE DOWN again

`kubectl scale --replicas=1 deploy contoso-website -n costsavings`

### With the following command below I could do a SCALE UP again

`kubectl scale --replicas=50 deploy contoso-website -n costsavings`


