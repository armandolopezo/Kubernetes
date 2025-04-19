### To Deploy Web App in AKS I have to use "KUBELET APPLY" with de "DEPLOYMENT.YAML" file 

### In Cloud Shell, run the kubectl apply command to submit the deployment manifest to your cluster.

`kubectl apply -f ./deployment.yaml`

```diff
- output sample:
- deployment.apps/contoso-website created
```
### Run the "kubectl get deploy" command to check if the deployment was successful.

`kubectl get deploy contoso-website`

```diff
+ output sample:
+ NAME              READY   UP-TO-DATE   AVAILABLE   AGE
+ contoso-website   0/1     1            0           16s
```

### Run the "kubectl get pods" command to check if the pod is running.

`kubectl get pods`

```diff
! output sample: 
! NAME                               READY   STATUS    RESTARTS   AGE
! contoso-website-7c58c5f699-r79mv   1/1     Running   0          63s
```
