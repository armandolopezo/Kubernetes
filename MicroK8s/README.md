```diff
+ I began to learn and practicing KUBERNETES with Microsoft Learn with the following learning path:
+    INTRODUCTION TO KUKERNETES ON AZURE 
+ I am emphasizing in practicing kubernetes with AKS (Azure Kubernetes Service)
```

##### MicroK8s is an option for deploying a single-node Kubernetes cluster as a single package to target workstations 
##### and Internet of Things (IoT) devices. Canonical, the creator of Ubuntu Linux, originally developed and currently maintains MicroK8s. 
#####   I tested the Linux version with Ubuntu 24.04 LTS in a VM inside HYPER-V
##### 
#####  Installing MicroK8s on Linux

```
sudo snap install microk8s --classic
```
#####  output EXAMPLE:
#####      2020-03-16T12:50:59+02:00 INFO Waiting for restart...
#####       microk8s v1.17.3 from Canonical✓ installed
#####
#####   Preparing the cluster:
#####    
```
sudo microk8s.status --wait-ready
```
#####  output EXAMPLE:
#####  microk8s is running
##### addons:
##### cilium: disabled
##### dashboard: disabled
##### dns: disabled
##### fluentd: disabled
##### gpu: disabled
##### helm3: disabled
##### helm: disabled
##### ingress: disabled
##### istio: disabled
##### jaeger: disabled
##### juju: disabled
##### knative: disabled
##### kubeflow: disabled
##### linkerd: disabled
##### metallb: disabled
##### metrics-server: disabled
##### prometheus: disabled
##### rbac: disabled
##### registry: disabled
##### storage: disabled  
#####      
#####
#####  
#####    
```
sudo microk8s.enable dns dashboard registry
```
#####  Explore the Kubernetes cluster
#####      
#####  Run the snap alias command to alias microk8s.kubectl to kubectl    
#####
```
sudo snap alias microk8s.kubectl kubectl
```
#####    output EXAMPLE:
#####    Added:
#####  - microk8s.kubectl as kubectl

##### Display cluster node information
##### ---------------------------------
```
sudo kubectl get nodes
```
##### output EXAMPLE:
##### NAME          STATUS   ROLES    AGE   VERSION
##### microk8s-vm   Ready    <none>   35m   v1.17.3
##### 
```
sudo kubectl get nodes -o wide
```
##### output EXAMPLE:
##### NAME          STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
##### microk8s-vm   Ready    <none>   36m   v1.17.3   192.168.56.132   <none>        Ubuntu 18.04.4 LTS   4.15.0-88-generic   containerd://1.2.5
##### 
```
sudo kubectl get services -o wide
```
##### output EXAMPLE:
##### NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE   SELECTOR
##### kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   37m   <none>
```
sudo kubectl get services -o wide --all-namespaces
```
##### output EXAMPLE:
##### NAMESPACE            NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE   SELECTOR
##### container-registry   registry                    NodePort    10.152.183.36    <none>        5000:32000/TCP           28m   app=registry
##### default              kubernetes                  ClusterIP   10.152.183.1     <none>        443/TCP                  37m   <none>
##### kube-system          dashboard-metrics-scraper   ClusterIP   10.152.183.130   <none>        8000/TCP                 28m   k8s-app=dashboard-metrics-scraper
##### kube-system          heapster                    ClusterIP   10.152.183.115   <none>        80/TCP                   28m   k8s-app=heapster
##### kube-system          kube-dns                    ClusterIP   10.152.183.10    <none>        53/UDP,53/TCP,9153/TCP   28m   k8s-app=kube-dns
##### kube-system          kubernetes-dashboard        ClusterIP   10.152.183.132   <none>        443/TCP                  28m   k8s-app=kubernetes-dashboard
##### kube-system          monitoring-grafana          ClusterIP   10.152.183.88    <none>        80/TCP                   28m   k8s-app=influxGrafana
##### kube-system          monitoring-influxdb         ClusterIP   10.152.183.232   <none>        8083/TCP,8086/TCP        28m   k8s-app=influxGrafana
##### 
##### 
##### output EXAMPLE:
##### Installing a web server on a cluster
##### 
##### 
```
sudo kubectl create deployment nginx --image=nginx
```

##### output EXAMPLE:
##### deployment.apps/nginx created
##### 
##### 
```
sudo kubectl get deployments
```
##### output EXAMPLE:
##### NAME    READY   UP-TO-DATE   AVAILABLE   AGE
##### nginx   1/1     1            1           18s
##### 
#####
```
sudo kubectl get pods
```
##### output EXAMPLE:
##### NAME                     READY   STATUS    RESTARTS   AGE
##### nginx-86c57db685-dj6lz   1/1     Running   0          33s
##### 

##### Testing de website installation
##### -------------------------------
```
sudo kubectl get pods -o wide
```
#####  Accesing website with command WGET 
##### 
```
wget <POD_IP>
```
##### output EXAMPLE:
##### --2020-03-16 13:34:17--  http://10.1.83.10/
##### Connecting to 10.1.83.10:80... connected.
##### HTTP request sent, awaiting response... 200 OK
##### Length: 612 [text/html]
##### Saving to: 'index.html'
##### 
##### index.html                                    100%[===============================>]     612  --.-KB/s    in 0s
##### 
##### 2020-03-16 13:34:17 (150 MB/s) - 'index.html' saved [612/612]
##### 

#####  
#####  Scale a web server deployment on a cluster
#####  ------------------------------------------

```
sudo kubectl scale --replicas=3 deployments/nginx
```
##### output EXAMPLE:
##### deployment.apps/nginx scaled

##### output EXAMPLE:
##### To check the number of running pods, run the kubectl get command, and again pass the -o wide parameter

```
sudo kubectl get pods -o wide
```
##### output EXAMPLE:
##### NAME                     READY   STATUS    RESTARTS   AGE     IP           NODE          NOMINATED NODE   READINESS GATES
##### nginx-86c57db685-dj6lz   1/1     Running   0          7m57s   10.1.83.10   microk8s-vm   <none>           <none>
##### nginx-86c57db685-lzrwp   1/1     Running   0          9s      10.1.83.12   microk8s-vm   <none>           <none>
##### nginx-86c57db685-m7vdd   1/1     Running   0          9s      10.1.83.11   microk8s-vm   <none>           <none>
##### ubuntu@microk8s-vm:~$



```

```

##### output EXAMPLE:
##### 
##### 
##### 
##### 
##### 
```
```
