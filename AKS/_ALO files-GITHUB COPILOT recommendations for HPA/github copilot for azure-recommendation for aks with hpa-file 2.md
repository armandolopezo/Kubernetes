armandolopezo: @azure can you give an aks template or sample to test horizontal pod autoscaler?

Azure (Preview): To test the Horizontal Pod Autoscaler (HPA) in Azure Kubernetes Service (AKS), you can follow the [tutorial on scaling applications in AKS](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale?wt.mc_id=knwlserAPI_inproduct_sdkextention#autoscale-pods). Here's a brief overview of the steps involved:

1. **Define Resource Requests and Limits**: Ensure that all containers have defined CPU requests and limits. For example:
   ```yaml
   resources:
     requests:
       cpu: 1m
     limits:
       cpu: 1000m
   ```

2. **Create an Autoscaler Manifest File**: Define the autoscaler behavior and resource limits in a manifest file, such as `aks-store-quickstart-hpa.yaml`.

3. **Apply the Autoscaler Manifest**: Use the following command to apply the manifest:
   ```bash
   kubectl apply -f aks-store-quickstart-hpa.yaml
   ```

4. **Check Autoscaler Status**: Verify the status of the autoscaler using:
   ```bash
   kubectl get hpa
   ```

For more detailed guidance, you can refer to the [Azure documentation on autoscaling pods in AKS](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale?wt.mc_id=knwlserAPI_inproduct_sdkextention#autoscale-pods).

*Responses are currently scoped to the **Mediatech Venezuela** tenant. [Select a different tenant](command:@azure.invokeChangeTenantSlashCommand).*

armandolopezo: @azure where can i find the file aks-store-quickstart-hpa.yaml mentioned previously?

Azure (Preview): The `aks-store-quickstart-hpa.yaml` file is typically a custom YAML manifest file that you create to define the Horizontal Pod Autoscaler for your application. You can create this file in your local development environment with the necessary configuration for your application. Here's an example of what the content might look like:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

You can adjust the `name`, `minReplicas`, `maxReplicas`, and `metrics` according to your application's requirements. Once you have created this file, you can apply it to your AKS cluster using the `kubectl apply -f aks-store-quickstart-hpa.yaml` command.

armandolopezo: @azure Can you show me an example of the aks-store-quickstart-hpa.yaml file?

Azure (Preview): 

armandolopezo: @azure How do I define resource requests and limits for my containers?

Azure (Preview): To define resource requests and limits for your containers in a Kubernetes deployment, you need to specify them in the container specification within your Pod or Deployment YAML file. Here's an example of how you can define CPU and memory requests and limits:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app-image:latest
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```

### Explanation:
- **requests**: This is the amount of CPU and memory that Kubernetes guarantees to the container. For example, `cpu: "250m"` means 250 milliCPU, which is 0.25 of a CPU core.
- **limits**: This is the maximum amount of CPU and memory that the container is allowed to use. If the container tries to exceed these limits, it may be throttled or terminated.

By setting these values, you help Kubernetes make better scheduling decisions and ensure that your application has the resources it needs to run efficiently.