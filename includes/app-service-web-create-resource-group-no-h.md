Ve službě Cloud Shell vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).

[!INCLUDE [resource group intro text](resource-group.md)]

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *Západní Evropa*.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Obvykle budete svoji skupinu prostředků a prostředky vytvářet v oblasti, kterou máte blízko. Pokud chcete zobrazit všechna podporovaná umístění pro plány služby App Service, spusťte příkaz `az appservice list-locations`.