## <a name="deploy-uploaded-zip-file"></a>Nasazení nahraného souboru ZIP

V prostředí Cloud Shell nasaďte nahraný soubor ZIP do vaší webové aplikace příkazem [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip). Nezapomeňte nahradit *\<app_name>* názvem vaší webové aplikace.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

Tento příkaz nasadí soubory a adresáře ze souboru ZIP do vaší výchozí složky aplikací služby App Service (`\home\site\wwwroot`) a restartuje aplikaci. Pokud je nakonfigurovaný nějaký vlastní proces sestavení, spustí se také.
