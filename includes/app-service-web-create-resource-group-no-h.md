Ve službě Cloud Shell vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *Západní Evropa*. Pokud chcete zobrazit všechna podporovaná umístění pro službu App Service, spusťte `az appservice list-locations` příkaz.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Obvykle budete svoji skupinu prostředků a prostředky vytvářet v oblasti, kterou máte blízko. 
