###### List of STEPS:
###### 1) Create Kubernetes Cluster with commands in AKSCREATE.MD
###### 2) Create a web app using the commands in DEPLOYAPPINAKS.MD that uses DEPLOYMENT.YAML file
###### 3) Look the following files in the subfolder SERVICE_AND_INGRESS_V1 inside the AKS folder (root folder for first files like this README.MD).
###### 4) Execute commands from README.MD in the subfolder with the following secuence:
###### 5) Deploy networking SERVICE with the SERVICE.YAML file.
###### 6) Create AZURE DNS ZONE with the commands in the file DNSZONE.MD.
###### 7) Deploy the networking INGRESS with the INGRESS.YAML file.

###### Some additional clarifications:
###### following was required to practicing AKS in Microsoft Learn (INTRODUCTION TO KUBERNETES ON AKS)
az provider register --namespace Microsoft.ContainerService

###### I have yo wait 30 minutos to use SPOT NODE POOLS because at the beginning the NGINX pod was in PENDING status and after 30 minutes
###### the pod changed to the desired RUNNING status. I was using AKS versión 1.30.

###### I had problems doing an exercise with AZURE POLICY for limiting AKS in the practices related with the LINK below:
######  https://learn.microsoft.com/en-us/training/modules/aks-optimize-compute-costs/7-exercise-resource-quota-azure-policy
######  The cause of the problem is that Ms learn don't explain that NGINX pod in the namespace COSTSAVING from previous exercise must be stopped 
######  (NGINX pod not running when doing exercise). To solve the problem I needed to do the following:
######  1) Use "kubectl get pods --name costsavings" to check if NGINX pod is running.
######  2) In case NGINX pod is running I need to stop it with "kubectl delete pod NGINX --name costsavings" 
######  3) Use "kubectl get pods --name costsavings" to check if NGINX pod is NOT running.
######  4) Test AZURE POLICY with: "kubectl apply --namespace costsavings -f test-policy.yaml"

