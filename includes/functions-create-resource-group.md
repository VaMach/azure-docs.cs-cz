## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#az_group_create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například aplikace Function App, databáze a účty úložiště.

Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.  
Pokud nepoužíváte cloudové prostředí, přihlaste se pomocí první `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Obvykle budete svoji skupinu prostředků a prostředky vytvářet v oblasti, kterou máte blízko. Pokud chcete zobrazit všechna podporovaná umístění pro plánů služby App Service, spusťte [míst seznamu služby App Service az](/cli/azure/appservice#az_appservice_list_locations) příkaz.