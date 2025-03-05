### Create an Azure DNS zone using the "az network dns zone create" command. The following example creates a DNS zone named contoso-website.com:

`az network dns zone create --resource-group $RESOURCE_GROUP --name contoso-website.com`

### Get the resource ID for your DNS zone using the "az network dns zone show" command and save the output to a variable named DNS_ZONE_ID.

`DNS_ZONE_ID=$(az network dns zone show --resource-group $RESOURCE_GROUP --name contoso-website.com --query id --output tsv)`

### Update the application routing cluster add-on to enable Azure DNS integration using the az aks approuting zone command.

`az aks approuting zone add --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --ids=${DNS_ZONE_ID} --attach-zones`

### Update the "INGRESS.YAML" with the DNS ZONE RECENTLY CREATED.

