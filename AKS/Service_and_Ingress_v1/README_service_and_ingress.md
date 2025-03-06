### I will test SERVICE and INGRESS deployments from the following web page that
### I liked and I wanted to test to try to practice and learn AKS with the first
### lab tests to use Kubernetest with a sample web app with external access.
### the page of interest is:
### https://www.goglides.dev/roshan_thapa/deploy-a-application-on-azure-kubernetes-service-2hg0
### 
### The SERVICE and INGRESS deployments requires previous AKS cluster creation from the parent folder.
###
### I created this subfolder for "SERVICE and INGRESS" deployments tests because may be I will add other subfolders for
### TESTING DIFFERENT NETWORKING SOLUTIONS (for example, using AZURE PRIVATE DNS ZONES AND/or LOAD BALANCERS ) 
###

### to deploy the service I have to use the following command combined with the SERVICE.YAML file

`kubectl apply -f ./service.yaml`

### Run the "kubectl get service" command to check if the deployment was successful.

```diff
+ output sample:
+ NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
+ contoso-website   ClusterIP   10.0.158.189   <none>        80/TCP    42s
```
### I have to create an link a AZURE DNS ZONE with AKS. See and execute the COMMANDS from "DnsZone.md" to cree the required DNS ZONE.

### I will execute the following two commands in AZURE CLOUD SHELL to test DNS ZONE (I don't know if they work.)

`az network dns zone list`

```
az aks show \
  -g $RESOURCE_GROUP \
  -n $CLUSTER_NAME \
  -o tsv \
  --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName
```
### After knowing tha ACTUAL DNS ZONE, I have to change the host  field (hostname) with  the actual FQDN modifying the INGRESS.YAML before the deployment of INGRESS using de CHANGED ingress.yaml file.

### After the required changes in the INGRESS.YAML file I should made the deployment with the Cloud Shell, running the "kubectl apply" command to submit the ingress manifest to your cluster with the following command.

`kubectl apply -f ./ingress.yaml`

### Now, Run the "kubectl get ingress" command to check if the deployment was successful.

`kubectl get ingress contoso-website`

```diff
+ output sample:
+ NAME              HOSTS                                           ADDRESS        PORTS   AGE
+ contoso-website   contoso.5cd29ec927f24764b052.eastus.aksapp.io   52.226.96.30   80      4m44s
```
### Most probably HOSTS is not like the previous output, but * that is RIGHT. I also have to wait some SECONDS OR MINUTES FOR UPDATES
### of the IP ADDRESS AND FQDN IN THE AZURE DNS ZONE. FOR EXAMPLE FOR THE INGRESS.YAMLfile updated today march 5, 2025 there is a 
### creation of the TYPE A RECORD: WWW in the zone CONTOSOALO.COM that corresponds to the registration of HOSTNAME WWW.CONTOSOALO.COM 
### created in INGRESS.YAML (ANNOTATIONS:)

### Finally, I have to Open a browser, and go to the FQDN described in the output or use the PUBLIC IP ADDRESS. You should see a websiteo