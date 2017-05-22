## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

 [Skupina prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) je logický kontejner, ve kterém se spravují prostředky jako webové aplikace a databáze. Prostředky ve skupině prostředků můžete nasazovat, aktualizovat a odstraňovat.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).

```azurecli
az group create --name myResourceGroup --location westeurope
```

Pokud chcete zobrazit dostupná umístění, použijte příkaz `az appservice list-locations`. Obvykle budete prostředky vytvářet v oblasti, kterou máte blízko.

## <a name="create-an-azure-app-service-plan"></a>Vytvoření plánu služby Azure App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte [plán služby App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) na úrovni FREE.

[!INCLUDE [app-service-plan](app-service-plan.md)]

Následující příkaz vytvoří plán služby App Service s názvem `quickStartPlan` a s cenovou úrovní **Free**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Po vytvoření plánu služby App Service se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "quickStartPlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "quickStartPlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```

## <a name="create-a-web-app"></a>Vytvoření webové aplikace