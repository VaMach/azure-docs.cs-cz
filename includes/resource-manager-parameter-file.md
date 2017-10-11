Pokud použijete soubor s parametry předat hodnoty parametrů během nasazování, budete muset vytvořte soubor JSON ve formátu podobně jako v následujícím příkladu:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Velikost souboru parametr nemůže být delší než 64 KB.

Pokud potřebujete zajistit citlivou hodnotu pro parametr (třeba heslo), přidejte tuto hodnotu do trezoru klíčů. Během nasazování načtěte trezoru klíčů, jak je znázorněno v předchozím příkladu. Další informace najdete v tématu [předat zabezpečené hodnoty během nasazení](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

