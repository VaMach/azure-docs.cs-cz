# Přehled

## [Co je Resource Manager?](resource-group-overview.md)

## [Poskytovatelé a typy prostředků](resource-manager-supported-services.md)

## [Resource Manager a klasické nasazení](resource-manager-deployment-model.md)

## [Zásady správného řízení pro předplatná](resource-manager-subscription-governance.md)

## [Spravované aplikace](managed-application-overview.md)


# Začínáme

## [Export šablony](resource-manager-export-template.md)

## [Vytvoření první šablony](resource-manager-create-first-template.md)

## [Visual Studio s Resource Managerem](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)


# Ukázky

## PowerShell

### [Nasazení šablony](resource-manager-samples-powershell-deploy.md)


## Azure CLI

### [Nasazení šablony](resource-manager-samples-cli-deploy.md)


# Postup

## Vytváření šablon

### [Osvědčené postupy pro šablony](resource-manager-template-best-practices.md)

### [Části šablon](resource-group-authoring-templates.md)

### [Propojení s ostatními šablonami](resource-group-linked-templates.md)

### [Definování závislosti mezi prostředky](resource-group-define-dependencies.md)

### [Vytvoření víc instancí](resource-group-create-multiple.md)

### [Nastavení umístění](resource-manager-template-location.md)

### [Přiřazení značek](resource-manager-template-tags.md)

### [Nastavení názvu a typu podřízeného prostředku](resource-manager-template-child-resource.md)

### [Aktualizace prostředku](resource-manager-update.md)

### [Použití objektů pro parametry](resource-manager-objects-as-parameters.md)

### [Sdílení stavu mezi propojenými šablonami](best-practices-resource-manager-state.md)

### [Způsoby navrhování šablon](best-practices-resource-manager-design-templates.md)


## Nasazení

### PowerShell

#### [Nasazení šablony](resource-group-template-deploy.md)

#### [Nasazení privátní šablony s tokenem SAS](resource-manager-powershell-sas-token.md)

#### [Export šablony a opětovné nasazení](resource-manager-export-template-powershell.md)

### Azure CLI

#### [Nasazení šablony](resource-group-template-deploy-cli.md)

#### [Nasazení privátní šablony s tokenem SAS](resource-manager-cli-sas-token.md)

#### [Export šablony a opětovné nasazení](resource-manager-export-template-cli.md)

### [Azure Portal](resource-group-template-deploy-portal.md)

### [REST API](resource-group-template-deploy-rest.md)

### [Nasazení napříč skupinami prostředků](resource-manager-cross-resource-group-deployment.md)

### [Průběžná integrace se sadou Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

### [Předání zabezpečených hodnot během nasazení](resource-manager-keyvault-parameter.md)


## Spravovat

### [PowerShell](powershell-azure-resource-manager.md)

### [Azure CLI](xplat-cli-azure-resource-manager.md)

### [Azure Portal](resource-group-portal.md)

### [REST API](resource-manager-rest-api.md)

### [Použití značek k uspořádání prostředků](resource-group-using-tags.md)

### [Přesunutí prostředků do nové skupiny nebo předplatného](resource-group-move-resources.md)

### [Příklady zásad správného řízení](resource-manager-subscription-examples.md)


## Řízení přístupu

### Vytvoření instančního objektu

#### [PowerShell](resource-group-authenticate-service-principal.md)

#### [Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

#### [Azure CLI 1.0](resource-group-authenticate-service-principal-cli.md)

#### [Azure Portal](resource-group-create-service-principal-portal.md)

### [Ověřovací rozhraní API pro přístup k předplatným](resource-manager-api-authentication.md)

### [Uzamčení prostředků](resource-group-lock-resources.md)


## Nastavení zásad prostředků

### [Co jsou zásady prostředků?](resource-manager-policy.md)

### [Přiřazení zásad pomocí portálu](resource-manager-policy-portal.md)

### [Přiřazení zásad pomocí skriptů](resource-manager-policy-create-assign.md)

### Příklady

#### [Značky](resource-manager-policy-tags.md)

#### [Zásady vytváření názvů](resource-manager-policy-naming-convention.md)

#### [Síť](resource-manager-policy-network.md)

#### [Storage](resource-manager-policy-storage.md)

#### [Virtuální počítač s Linuxem](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

#### [Virtuální počítač s Windows](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)


## Použití spravovaných aplikací

### [Publikování aplikace katalogu služeb](managed-application-publishing.md)

### [Využití aplikace katalogu služeb](managed-application-consumption.md)

### [Publikování aplikace Marketplace](managed-application-author-marketplace.md)

### [Využití aplikace Marketplace](managed-application-consume-marketplace.md)

### [Vytvoření definic uživatelského rozhraní](managed-application-createuidefinition-overview.md)


## Auditování

### [Zobrazení protokolů aktivit](resource-group-audit.md)

### [Zobrazení operací nasazení](resource-manager-deployment-operations.md)


## Řešení potíží

### [Běžné chyby nasazení](resource-manager-common-deployment-errors.md)

### [Chyba RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)


# Referenční informace

## [Formát šablon](/azure/templates/)

## [Funkce šablon](resource-group-template-functions.md)

### [Funkce pole a objektu](resource-group-template-functions-array.md)

### [Funkce porovnání](resource-group-template-functions-comparison.md)

### [Funkce nasazení](resource-group-template-functions-deployment.md)

### [Numerické funkce](resource-group-template-functions-numeric.md)

### [Funkce prostředků](resource-group-template-functions-resource.md)

### [Funkce řetězců](resource-group-template-functions-string.md)

## [Funkce definic uživatelského rozhraní](managed-application-createuidefinition-functions.md)

## [Definiční prvky uživatelského rozhraní](managed-application-createuidefinition-elements.md)

### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)

### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)

### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)

### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)

### [Microsoft.Common.Section](managed-application-microsoft-common-section.md)

### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)

### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)

### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)

### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)

### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)

### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)

### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)

### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)

## [PowerShell](/powershell/module/azurerm.resources)

## [Azure CLI](/cli/azure/resource)

## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)

## [Java](/java/api/com.microsoft.azure.management.resources)

## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)

## [REST](/rest/api/resources/)


# Zdroje a prostředky

## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/)

## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=azure-resource-manager)

## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)

## [Omezování požadavků](resource-manager-request-limits.md)

## [Sledování asynchronních operací](resource-manager-async-operations.md)

## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)

