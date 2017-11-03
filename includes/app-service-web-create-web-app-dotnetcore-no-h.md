Ve službě Cloud Shell pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../articles/app-service/containers/app-service-linux-intro.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app_name>` s jedinečným názvem aplikace.

Modul runtime v následujícím příkazu je nastaven na `dotnetcore|1.1`. Pokud chcete zobrazit všechny podporované moduly runtime, spusťte [az webapp seznamu runtimes](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "dotnetcore|1.1" --deployment-local-git
```

Po vytvoření webové aplikace Azure CLI ukazuje výstup podobně jako v následujícím příkladu:

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

Prázdný novou webovou aplikaci jste vytvořili v kontejneru Linux s nasazením git povolena.

> [!NOTE]
> Adresa URL Git vzdáleného jsou uvedené v `deploymentLocalGitUrl` vlastnost ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tato adresa URL uložte, protože ho budete potřebovat později.
>
