Ve službě Cloud Shell pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../articles/app-service/app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. 

Tato webová aplikace poskytuje prostor pro hostování vašeho kódu a adresu URL, na které si můžete nasazenou aplikaci zobrazit.

V následujícím příkazu nahraďte *\<app_name >* jedinečným názvem (platné znaky jsou `a-z`, `0-9` a `-`). Pokud název `<app_name>` není jedinečný, zobrazí se chybová zpráva „Web se zadaným názvem <název_aplikace> již existuje“. Výchozí adresa URL webové aplikace je `https://<app_name>.azurewebsites.net`. 

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

Nasazení git povoleno jste vytvořili prázdný webové aplikace.

> [!NOTE]
> Adresa URL Git vzdáleného jsou uvedené v `deploymentLocalGitUrl` vlastnost ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tato adresa URL uložte, protože ho budete potřebovat později.
>

Přejděte na web a zobrazte nově vytvořenou webovou aplikaci.

```bash
http://<app_name>.azurewebsites.net
```
