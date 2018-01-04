Ve službě Cloud Shell pomocí příkazu [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) vytvořte [webovou aplikaci](../articles/app-service/app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. 

V následujícím příkladu nahraďte *\<app_name>* globálně jedinečným názvem aplikace (platné znaky jsou `a-z`, `0-9` a `-`). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Vytvořili jste prázdnou webovou aplikaci s povoleným nasazením Gitu.

> [!NOTE]
> Adresa URL vzdáleného úložiště Git se zobrazuje ve vlastnosti `deploymentLocalGitUrl` ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tuto adresu URL si uložte, protože ji budete potřebovat později.
>

Přejděte do nově vytvořené webové aplikace.

```bash
http://<app_name>.azurewebsites.net
```
