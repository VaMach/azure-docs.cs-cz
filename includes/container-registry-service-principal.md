## <a name="create-a-service-principal"></a>Vytvoření instančního objektu

Pokud chcete vytvořit objekt služby přístup k registru systému kontejneru, můžete použít následující skript. Aktualizace `ACR_NAME` proměnné s názvem vaší registru kontejneru a volitelně `--role` hodnotu [az ad sp vytvořit pro rbac] [ az-ad-sp-create-for-rbac] příkaz udělit oprávnění jiný. Musíte mít [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nainstalovat pomocí tohoto skriptu.

Po spuštění skriptu si všimněte instanční objekt **ID** a **heslo**. Jakmile máte svoje přihlašovací údaje, můžete nakonfigurovat aplikace a služby k ověření vaší registru kontejneru jako objekt služby.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_NAME can be any unique name within your
# subscription (you can use the default below).
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate some values required for subsequent command args
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for both docker push and pull access. Modify the
# '--role' argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>Použít stávající instanční objekt

Pokud chcete udělit přístup k registru do existující služby hlavní, je nutné přiřadit ke službě hlavní novou roli. Stejně jako u vytváření hlavní novou službu, můžete udělit pro vyžádání obsahu, zápis a čtení a přístup vlastníka.

Tento skript používá [vytvořit přiřazení role az] [ az-role-assignment-create] příkaz udělit *vyžádání* oprávnění k hlavní název služby zadáte v `SERVICE_PRINCIPAL_ID` proměnné. Upravit `--role` hodnotu, pokud chcete udělit různé úrovně přístupu.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create