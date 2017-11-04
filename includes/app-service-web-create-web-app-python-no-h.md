V prostředí cloudu, vytvoření webové aplikace v `myAppServicePlan` plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp#create) příkaz. Nezapomeňte nahradit `<app_name>` s jedinečným názvem aplikace.

Modul runtime v následujícím příkazu je nastaven na `python|3.4`. Pokud chcete zobrazit všechny podporované moduly runtime, spusťte [az webapp seznamu runtimes](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "python|3.4" --deployment-local-git
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

Prázdný nové webové aplikace, jste vytvořili s nasazením git povolena.

> [!NOTE]
> Adresa URL Git vzdáleného jsou uvedené v `deploymentLocalGitUrl` vlastnost ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tato adresa URL uložte, protože ho budete potřebovat později.
>
