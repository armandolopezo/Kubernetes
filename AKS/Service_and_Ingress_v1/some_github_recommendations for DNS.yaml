armandolopezo: I want to add azure DNS zone to ingress.yaml for aks

GitHub Copilot: To add an Azure DNS zone to your ingress.yaml for AKS, you need to add annotations under the `metadata` section. Here is an example of how you can modify your ingress.yaml file to include the Azure DNS zone:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: contoso-website
  annotations:
    external-dns.alpha.kubernetes.io/hostname: contoso-website.example.com
spec:
  ingressClassName: webapprouting.kubernetes.azure.com
  rules:
  - http:
      paths:
      - backend:
          service:
            name: contoso-website
            port:
              number: 80
        path: /
        pathType: Prefix
```

Replace `contoso-website.example.com` with your actual DNS zone name. This annotation tells the external DNS controller to create a DNS record for your Ingress resource.

armandolopezo: how to create de dns zone mencionated in annotations of this file using azure cloud shell with bash commands

GitHub Copilot: To create the DNS zone mentioned in the annotations of your `ingress.yaml` file using Azure Cloud Shell with Bash commands, follow these steps:

1. **Login to Azure:**
   If you are not already logged in, use the following command to log in to your Azure account:
   ```bash
   az login
   ```

2. **Create a Resource Group:**
   If you don't already have a resource group, create one using the following command:
   ```bash
   az group create --name MyResourceGroup --location eastus
   ```

3. **Create the DNS Zone:**
   Use the following command to create the DNS zone. Replace `example.com` with your actual domain name and `MyResourceGroup` with your resource group name:
   ```bash
   az network dns zone create --resource-group MyResourceGroup --name example.com
   ```

4. **Verify the DNS Zone:**
   You can verify that the DNS zone has been created by listing the DNS zones in your resource group:
   ```bash
   az network dns zone list --resource-group MyResourceGroup
   ```

Here is a summary of the commands:

```bash
az login
az group create --name MyResourceGroup --location eastus
az network dns zone create --resource-group MyResourceGroup --name example.com
az network dns zone list --resource-group MyResourceGroup
```

Replace `MyResourceGroup` and `example.com` with your actual resource group name and domain name respectively.