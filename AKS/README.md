###### List of STEPS:
###### 1) Create Kubernetes Cluster with commands in AKSCREATE.MD
###### 2) Create a web app using the commands in DEPLOYAPPINAKS.MD that uses DEPLOYMENT.YAML file
###### 3) Look the following file README_service_and_ingress.MD (we can ommit the DNS instructions if we test with PUBLIC IP ADDRESS of the web page).
###### 5) Deploy networking SERVICE with the SERVICE.YAML file.
###### 6) Deploy the networking INGRESS with the INGRESS.YAML file.
###### 7) Deploy HORZONTAL POD AUTOSCALER with hpa.yaml file .

######  The following commands are very useful for checking the differents steps of AKS clusters (asuming i am using a namespace called "hpa-contose" for the load:
######
###### `kubectl get nodes`
###### `kubectl get pods`
###### `kubectl apply -f ?????????.yaml`
###### `kubectl get deploy contoso-website`
###### `kubectl get deployment contoso-website --namespace hpa-contoso`
###### `kubectl get service contoso-website --namespace hpa-contoso`
###### `kubectl get ingress contoso-website --namespace hpa-contoso`
###### `kubectl get hpa contoso-website --namespace hpa-contoso`
###### `kubectl get rs -n hpa-contoso`
###### `kubectl describe hpa --namespace hpa-contoso`
######

######   For the first successful testing of Horizontal Pod autoscaling (UP and DOWN) i used Apache JMeter with the TEST PLAN contained in TEST6.JMX with
######   the following initial configuration: 1) Simulation of 10,000 users. 2) Destination web request to the public ip address of the INGRESS service 
######   of AKS. 3) RAmp-Up time: 600 seconds (10 minutes) . 4) Infinite loop (that was not ideal contition and I had to finish manually. 5) I created
######   a test plan with only one job and the following elements in the job: a) http requests default b) http cookie manager. c) http request 1 d) http request 2
######   e) backend listener.

###### -----------------------------------------------------------------------------------------------------------------------------------------------------------------------
###### Some additional clarifications for practicing MICROSOFT LEARN:
###### following was required to practicing AKS in Microsoft Learn (INTRODUCTION TO KUBERNETES ON AKS)
az provider register --namespace Microsoft.ContainerService
h 
###### I have yo wait 30 minutos to use SPOT NODE POOLS because at the beginning the NGINX pod was in PENDING status and after 30 minutes
###### the pod changed to the desired RUNNING status. I was using AKS versión 1.30.

###### I had problems doing an exercise with AZURE POLICY for limiting AKS in the practices related with the LINK below:
######  https://learn.microsoft.com/en-us/training/modules/aks-optimize-compute-costs/7-exercise-reshe ource-quota-azure-policy
######  The cause of the problem is that Ms learn don't explain that NGINX pod in the namespace COSTSAVING from previous exercise must be stopped 
######  (NGINX pod not running when doing exercise). To solve the problem I needed to do the following:
######  1) Use "kubectl get pods --name costsavings" to check if NGINX pod is running.
######  2) In case NGINX pod is running I need to stop it with "kubectl delete pod NGINX --name costsavings" 
######  3) Use "kubectl get pods --name costsavings" to check if NGINX pod is NOT running.
######  4) Test AZURE POLICY with: "kubectl apply --namespace costsavings -f test-policy.yaml"

