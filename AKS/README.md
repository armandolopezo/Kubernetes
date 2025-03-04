
# following was required to practicing AKS in Microsoft Learn (INTRODUCTION TO KUBERNETES ON AKS)
az provider register --namespace Microsoft.ContainerService

# I have yo wait 30 minutos to use SPOT NODE POOLS because at the beginning the NGINX pod was in PENDING status and after 30 minutes
# the pod changed to the desired RUNNING status. I was using AKS versión 1.30.

# I had problems doing an exercise with AZURE POLICY for limiting AKS in the practices related with the LINK below:
#  https://learn.microsoft.com/en-us/training/modules/aks-optimize-compute-costs/7-exercise-resource-quota-azure-policy
#  The cause of the problem is that Ms learn don't explain that NGINX pod in the namespace COSTSAVING from previous exercise must be stopped 
#  (NGINX pod not running when doing exercise). To solve the problem I needed to do the following:
#  1) Use "kubectl get pods --name costsavings" to check if NGINX pod is running.
#  2) In case NGINX pod is running I need to stop it with "kubectl delete pod NGINX --name costsavings" 
#  3) Use "kubectl get pods --name costsavings" to check if NGINX pod is NOT running.
#  4) Test AZURE POLICY with: "kubectl apply --namespace costsavings -f test-policy.yaml"

