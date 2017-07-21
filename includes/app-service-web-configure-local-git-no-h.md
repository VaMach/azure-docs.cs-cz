Nakonfigurujte nasazení do webové aplikace prostřednictvím místního Gitu pomocí příkazu [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git).

App Service podporuje řadu způsobů nasazení obsahu do webové aplikace, jako například FTP, místní Git, GitHub, Visual Studio Team Services nebo Bitbucket. Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu. To znamená, že nasazení provedete použitím příkazu Gitu, který přenese obsah z místního úložiště do úložiště v Azure. 

V následujícím příkazu nahraďte *\<app_name>* názvem webové aplikace.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Výstup má následující formát:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

`<username>` je [uživatel nasazení](#configure-a-deployment-user), kterého jste vytvořili v předchozím kroku.

Uložte si zobrazený identifikátor URI. Použijete ho v dalším kroku. 
