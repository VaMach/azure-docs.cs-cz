Pomocí příkazu [az appservice web create](/cli/azure/appservice/web#create) vytvořte webovou aplikaci. V následujícím příkazu nahraďte zástupný symbol `<app_name>` jedinečným názvem aplikace. `<app_name>` se používá ve výchozí lokalitě DNS pro webovou aplikaci. Pokud název `<app_name>` není jedinečný, zobrazí se popisná chybová zpráva „Web se zadaným názvem <název_aplikace> již existuje“.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Po vytvoření webové aplikace se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:

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

Přejděte na web (`http://<app_name>.azurewebsites.net`)a zobrazte nově vytvořenou webovou aplikaci.

![app-service-web-service-created](../articles/app-service-web/media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)


## <a name="configure-local-git-deployment"></a>Konfigurace nasazení z místního Gitu

App Service podporuje řadu způsobů nasazení obsahu do webové aplikace, jako například FTP, místní Git, GitHub, Visual Studio Team Services nebo Bitbucket. 

Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu. To znamená, že nasazení provedete použitím příkazu Gitu, který přenese obsah z místního úložiště do úložiště v Azure. 

Pomocí příkazu [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) nastavte přístup místního Gitu k webové aplikaci.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Výstup má následující formát:

```
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Uložte si zobrazený identifikátor URI. Použijete ho v dalším kroku. `<username>` je [uživatel nasazení](#configure-a-deployment-user), kterého jste vytvořili v předchozím kroku.

## <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

Přidejte vzdálené úložiště Azure do místního úložiště Gitu.

```bash
git remote add azure <URI from previous step>
```

Nasaďte aplikaci do vzdáleného úložiště Azure. Zobrazí se výzva k zadání hesla, které jste zadali dříve při vytváření uživatele nasazení. Dejte pozor na to, abyste zadali heslo, které jste vytvořili v kroku [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```azurecli
git push azure master
```

Předchozí příkaz zobrazí podobné informace jako v následujícím příkladu:

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-app"></a>Přechod do aplikace


Přejděte do nasazené aplikace:

```
http://<app_name>.azurewebsites.net
```

