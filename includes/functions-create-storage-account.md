## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

Funkce používá pro obecné účely účet ve službě Azure Storage zachování stavu a další informace o funkcí. Vytvořit účet úložiště pro obecné účely ve skupině prostředků, který jste vytvořili pomocí [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.

V následujícím příkazu nahraďte název účtu globálně jedinečný úložiště, kde uvidíte `<storage_name>` zástupný symbol. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Po vytvoření účtu úložiště se v rozhraní Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```