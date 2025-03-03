
# following was required to practicing AKS in Microsoft Learn (INTRODUCTION TO KUBERNETES ON AKS)
az provider register --namespace Microsoft.ContainerService

# I have yo wait 30 minutos to use SPOT NODE POOLS because at the beginning the NGINX pod was in PENDING status and after 30 minutes
# the pod changed to the desired RUNNING status. I was using AKS versión 1.30.



