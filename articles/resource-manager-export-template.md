<properties
    pageTitle="Vyexportování šablony Azure Resource Manageru | Microsoft Azure"
    description="Pomocí Azure Resource Manageru si můžete vyexportovat šablonu z existující skupiny prostředků."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/10/2016"
    ms.author="tomfitz"/>

# Vyexportování šablony Azure Resource Manageru z existujících prostředků

Porozumět principům vytváření šablon Azure Resource Manageru nemusí být jednoduché. Naštěstí vám s tím Resource Manager pomůže. Můžete si totiž vyexportovat šablonu z existujících prostředků v rámci svého předplatného. Z vygenerované šablony pak zjistíte syntaxi šablony a podle potřeby pak můžete automatizovat opakované nasazení svého řešení.

V tomto kurzu se přihlásíte na Portál Azure, vytvoříte účet úložiště a vyexportujete šablonu pro daný účet úložiště. Přidáte virtuální síť a tím upravíte skupinu prostředků. Nakonec vyexportujete novou šablonu, která představuje její aktuální stav. Přestože se tento článek zaměřuje na zjednodušenou infrastrukturu, můžete stejným postupem vyexportovat i šablonu pro složitější řešení.

## Vytvoření účtu úložiště

1. Na [Portálu Azure](https://portal.azure.com) vyberte **Nový** > **Data a úložiště** > **Účet úložiště**.

      ![vytvoření úložiště](./media/resource-manager-export-template/create-storage.png)

2. Vytvořte účet úložiště s názvem **storage**, vašimi iniciálami a datem. Název účtu úložiště musí být jedinečný v rámci Azure. Pokud se první název, který zadáte, už používá, zkuste použít jinou variantu. Pro skupinu prostředků použijte název **ExportGroup**. Pro ostatní vlastnosti můžete použít výchozí hodnoty. Vyberte **Vytvořit**.

      ![zadání hodnot pro úložiště](./media/resource-manager-export-template/provide-storage-values.png)

Po dokončení nasazení obsahuje vaše předplatné účet úložiště.

## Vyexportování šablony pro nasazení

1. Přejděte do okna nové skupiny prostředků. Uvidíte, že je zobrazený výsledek posledního nasazení. Vyberte tento odkaz.

      ![okno skupiny prostředků](./media/resource-manager-export-template/resource-group-blade.png)

2. Zobrazí se vám historii nasazení pro skupinu. Ve vašem případě bude s velkou pravděpodobností uvedené jenom jedno nasazení. Vyberte ho.

     ![poslední nasazení](./media/resource-manager-export-template/last-deployment.png)

3. Zobrazí se souhrn nasazení. Souhrn obsahuje stav nasazení a jeho operací a také hodnoty, které jste zadali pro parametry. Pokud chcete zobrazit šablonu, která byla použita pro nasazení, vyberte možnost **Zobrazit šablonu**.

     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/deployment-summary.png)

4. Resource Manager vám načte následujících pět souborů:

   - Šablonu, která definuje infrastrukturu pro vaše řešení. Když jste prostřednictvím portálu vytvářeli účet úložiště, Resource Manager k jeho nasazení použil šablonu a tuto šablonu uložil pro budoucí použití.

   - Soubor s parametry, který slouží k předávání hodnot během nasazování. Obsahuje hodnoty, které jste zadali při prvním nasazení. Kteroukoli z nich ale můžete při opětovném nasazování šablony změnit.

   - Soubor skriptu Azure PowerShellu, který můžete použít k nasazení šablony

   - Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony

   - Třídu rozhraní .NET, kterou můžete použít k nasazení šablony

     Soubory jsou k dispozici prostřednictvím odkazů v rámci okna. Ve výchozím nastavení je vybraná šablona.

       ![zobrazení šablony](./media/resource-manager-export-template/view-template.png)

     Pojďme se teď na šablonu podívat trochu podrobněji. Vaše šablona by měla vypadat nějak takto:

        {      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",      "contentVersion": "1.0.0.0",      "parameters": {        "name": {          "type": "String"        },        "accountType": {          "type": "String"        },        "location": {          "type": "String"        },        "encryptionEnabled": {          "defaultValue": false,          "type": "Bool"        }      },      "resources": [        {          "type": "Microsoft.Storage/storageAccounts",          "sku": {            "name": "[parameters('accountType')]"          },          "kind": "Storage",          "name": "[parameters('name')]",          "apiVersion": "2016-01-01",          "location": "[parameters('location')]",          "properties": {            "encryption": {              "services": {                "blob": {                  "enabled": "[parameters('encryptionEnabled')]"                }              },              "keySource": "Microsoft.Storage"            }          }        }      ]    }

     Všimněte si, že šablona definuje parametry pro název, typ a umístění účtu úložiště. Parametr také určuje, jestli je povolené šifrování, a výchozí hodnota je **false**. V oddílu **resources** uvidíte definici účtu úložiště, který se má nasadit.

Hranaté závorky obsahují výraz, který se vyhodnotí během nasazování. Výrazy v závorkách v šabloně se používají k získání hodnot parametrů během nasazování. Můžete použít celou řadu dalších výrazů a dále v tomto článku si ukážeme jejich příklady. Úplný seznam najdete v tématu věnovaném [funkcím šablony Azure Resource Manageru](resource-group-template-functions.md).

Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).

## Přidání virtuální sítě

Šablona, kterou jste si stáhli v předchozí části, představovala infrastrukturu pro tuto původní nasazení, nezohlední se v ní ale žádné změny, které provedete po nasazení.
Pro ilustraci tohoto problému pojďme upravit skupinu prostředků přidáním virtuální sítě prostřednictvím portálu.

1. V okně skupiny prostředků vyberte **Přidat** a z dostupných prostředků pak vyberte **Virtuální síť**.

2. Virtuální síť pojmenujte **VNET** a pro ostatní vlastnosti použijte výchozí hodnoty. Vyberte **Vytvořit**.

      ![nastavení upozornění](./media/resource-manager-export-template/create-vnet.png)

3. Po úspěšném nasazení virtuální sítě do skupiny prostředků si znovu zobrazte historii nasazení. Uvidíte teď dvě nasazení. Vyberte novější nasazení.

      ![historie nasazení](./media/resource-manager-export-template/deployment-history.png)

4. Podívejte se na šablonu pro toto nasazení. Všimněte si, že definuje pouze změny, které jste provedli, když jste přidávali virtuální síť.

Obecně je vhodné pracovat se šablonou, která nasadí veškerou infrastrukturu pro vaše řešení v jediné operaci, abyste nemuseli pamatovat na to, že máte nasadit několik různých šablon.


## Vyexportování šablony pro skupinu prostředků

I když každé nasazení zobrazuje pouze změny, které jste provedli ve vaší skupině prostředků, můžete šablonu kdykoli vyexportovat a zobrazit si atributy celé skupiny prostředků.  

1. Když si budete chtít zobrazit šablonu pro skupinu prostředků, vyberte **Exportovat šablonu**.

      ![export skupiny prostředků](./media/resource-manager-export-template/export-resource-group.png)

2. Opět uvidíte pět souborů, které můžete použít k opětovnému nasazení řešení, tentokrát se ale šablona mírně liší. Tato šablona obsahuje pouze dva parametry: jeden pro název účtu úložiště a jeden pro název virtuální sítě.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Resource Manager nenačetl šablony, které byly použity během nasazování. Místo toho vygeneroval novou šablonu, která je založená na aktuální konfiguraci prostředků. Resource Manager neví, jaké hodnoty chcete předat jako parametry, takže většinu hodnot pevně definuje na základě hodnot ve skupině prostředků. Například hodnoty umístění a replikace účtu úložiště jsou nastaveny takto:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Stáhněte si šablonu, abyste na ní mohli pracovat místně.

      ![stažení šablony](./media/resource-manager-export-template/download-template.png)

4. Vyhledejte soubor .zip, který jste si stáhli, a vyextrahujte obsah. Tuto staženou šablonu můžete použít k opětovnému nasazení své infrastruktury.

## Další kroky

Blahopřejeme! Naučili jste se, jak vyexportovat šablonu z prostředků, které jste vytvořili na portálu.

- V druhé části tohoto kurzu si šablonu, kterou jste si právě stáhli, přizpůsobíte přidáním dalších parametrů a znovu ji nasadíte pomocí skriptu. Další informace najdete v tématu [Přizpůsobení a opětovné nasazení vyexportované šablony](resource-manager-customize-template.md).
- Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).
- Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).



<!--HONumber=Jun16_HO2-->


