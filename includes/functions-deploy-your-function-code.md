## <a name="deploy-your-function-code"></a>Nasazení kódu funkce  

Existuje několik způsobů vytvoření kódu funkce v nové aplikaci Function App. V tomto tématu se připojíte k ukázkovému úložišti v GitHubu. Tak jako předtím nahraďte v následujícím kódu zástupný symbol `<app_name>` názvem aplikace Function App, který jste vytvořili. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Po nastavení zdroj nasazení, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu (hodnoty null Odebrat čitelnější):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```
