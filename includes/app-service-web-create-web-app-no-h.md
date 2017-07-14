Pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../articles/app-service-web/app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. 

Tato webová aplikace poskytuje prostor pro hostování vašeho kódu a adresu URL, na které si můžete nasazenou aplikaci zobrazit.

V následujícím příkazu nahraďte *\<app_name >* jedinečným názvem (platné znaky jsou `a-z`, `0-9` a `-`). Pokud název `<app_name>` není jedinečný, zobrazí se chybová zpráva „Web se zadaným názvem <název_aplikace> již existuje“. Výchozí adresa URL webové aplikace je `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Přejděte na web a zobrazte nově vytvořenou webovou aplikaci.

```bash
http://<app_name>.azurewebsites.net
```
