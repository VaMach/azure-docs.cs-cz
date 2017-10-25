Ve službě Cloud Shell pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.

[!INCLUDE [app-service-plan](app-service-plan-linux.md)]

Následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` v cenové úrovni **Standard** a v kontejneru Linuxu:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Po vytvoření plánu služby App Service se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```