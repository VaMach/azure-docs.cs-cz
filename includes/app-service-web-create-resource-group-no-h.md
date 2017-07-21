Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).

[!INCLUDE [resource group intro text](resource-group.md)]

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westeurope*.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Obvykle budete svoji skupinu prostředků a prostředky vytvářet v oblasti, kterou máte blízko. Pokud chcete zobrazit všechna podporovaná umístění pro webové aplikace Azure, spusťte příkaz `az appservice list-locations`. 