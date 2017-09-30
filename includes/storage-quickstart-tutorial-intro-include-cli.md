## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Vytvořte pomocí příkazu [az storage account create](/cli/azure/storage/account#create) účet úložiště úrovně Standard pro obecné účely. Účet úložiště úrovně Standard pro obecné účely lze použít pro všechny čtyři služby: Blobs, Files, Tables a Queues. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Zadání přihlašovacích údajů účtu úložiště

Azure CLI pro většinu příkazů v tomto kurzu potřebuje přihlašovací údaje účtu úložiště. Existuje několik možností, jak to udělat. Jedním z nejjednodušších způsobů, jak je poskytnout, je nastavit proměnné prostředí `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`.

Nejprve zobrazte klíče účtu úložiště pomocí příkazu [az storage account keys list](/cli/azure/storage/account/keys#list):

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

Nyní nastavte proměnné prostředí `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`. To můžete provést v prostředí Bash pomocí příkazu `export`:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_ACCESS_KEY="myStorageAccountKey"
```