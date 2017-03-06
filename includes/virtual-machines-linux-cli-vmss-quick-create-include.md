## <a name="prerequisites"></a>Požadavky

Pokud jste to ještě neudělali, získejte [bezplatnou zkušební verzi předplatného Azure](https://azure.microsoft.com/pricing/free-trial/) a nainstalujte [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="create-the-scale-set"></a>Vytvoření škálovací sady

Nejdřív vytvořte skupinu prostředků pro nasazení škálovací sady:

```azurecli
az group create --location westus --name myResourceGroup
```

Nyní pomocí příkazu `az vmss create` vytvořte škálovací sadu. Následující příklad vytvoří škálovací sadu pro Linux s názvem `myvmss` a skupinu prostředků `myrg`:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image UbuntuLTS --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Následující příklad vytvoří škálovací sadu pro Windows se stejnou konfigurací:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image Win2016Datacenter --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Pokud chcete vybrat jinou image operačního systému, můžete si dostupné image zobrazit pomocí příkazu `az vm image list` nebo `az vm image list --all`. K zobrazení informací o připojení pro virtuální počítače ve škálovací sadě použijte příkaz `az vmss list_instance_connection_info`:

```azurecli
az vmss list_instance_connection_info --resource-group myResourceGroup --name myVmss
```