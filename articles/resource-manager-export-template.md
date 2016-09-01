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
    ms.date="08/03/2016"
    ms.author="tomfitz"/>

# Vyexportování šablony Azure Resource Manageru z existujících prostředků

Resource Manager vám umožňuje exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném. Z vygenerované šablony pak zjistíte syntaxi šablony a podle potřeby pak můžete automatizovat opakované nasazení svého řešení.

Je důležité si uvědomit, že jsou dva různé způsoby exportu šablony:

- Je možné exportovat skutečnou šablonu, kterou jste použili k nasazení. Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně. Tento přístup je užitečný, pokud jste prostředky nasadili prostřednictvím portálu a teď se chcete podívat, jak sestavit šablonu pro vytváření těchto prostředků.
- Můžete exportovat šablonu, která představuje aktuální stav skupiny prostředků. Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení. Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků. Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali. Tento přístup je užitečný, pokud jste od nasazení změnili skupinu prostředků prostřednictvím portálu nebo skriptů a teď potřebujete zachytit skupinu prostředků jako šablonu.

Toto téma ukazuje oba přístupy. V článku [Přizpůsobení exportované šablony Azure Resource Manageru](resource-manager-customize-template.md) se dozvíte, jak vzít šablonu vygenerovanou z aktuálního stavu skupiny prostředků a využít ji lépe k opětovnému nasazení vašeho řešení.

V tomto kurzu se přihlásíte na web Azure Portal, vytvoříte účet úložiště a vyexportujete šablonu pro daný účet úložiště. Přidáte virtuální síť, čímž upravíte skupinu prostředků. Nakonec vyexportujete novou šablonu, která představuje aktuální stav skupiny. Přestože se tento článek zaměřuje na zjednodušenou infrastrukturu, můžete stejným postupem vyexportovat i šablonu pro složitější řešení.

## vytvořit účet úložiště

1. Na [Portálu Azure](https://portal.azure.com) vyberte **Nový** > **Data a úložiště** > **Účet úložiště**.

      ![vytvoření úložiště](./media/resource-manager-export-template/create-storage.png)

2. Vytvořte účet úložiště s názvem **storage**, vašimi iniciálami a datem. Název účtu úložiště musí být jedinečný v rámci Azure. Pokud se první název, který zadáte, už používá, zkuste použít jinou variantu. Pro skupinu prostředků použijte název **ExportGroup**. Pro ostatní vlastnosti můžete použít výchozí hodnoty. Vyberte **Vytvořit**.

      ![zadání hodnot pro úložiště](./media/resource-manager-export-template/provide-storage-values.png)

Po dokončení nasazení obsahuje vaše předplatné účet úložiště.

## Export šablony z historie nasazení

1. Přejděte do okna nové skupiny prostředků. Všimněte si, že okno zobrazuje výsledek posledního nasazení. Vyberte tento odkaz.

      ![okno skupiny prostředků](./media/resource-manager-export-template/resource-group-blade.png)

2. Zobrazí se vám historie nasazení pro skupinu. Ve vašem případě bude okno pravděpodobně uvádět jenom jedno nasazení. Vyberte ho.

     ![poslední nasazení](./media/resource-manager-export-template/last-deployment.png)

3. Okno zobrazí souhrn vybraného nasazení. Souhrn obsahuje stav nasazení a jeho operací a také hodnoty, které jste zadali pro parametry. Pokud chcete zobrazit šablonu, kterou jste použili k nasazení, vyberte možnost **Zobrazit šablonu**.

     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/deployment-summary.png)

4. Resource Manager pro vás načte následujících šest souborů:

   1. **Template** - Šablona, která definuje infrastrukturu pro vaše řešení. Když jste prostřednictvím portálu vytvářeli účet úložiště, Resource Manager k jeho nasazení použil šablonu a tuto šablonu uložil pro budoucí použití.
   2. **Parameters** - Soubor s parametry, který slouží k předávání hodnot během nasazení. Obsahuje hodnoty, které jste zadali při prvním nasazení. Kteroukoli z nich ale můžete při opětovném nasazování šablony změnit.
   3. **CLI** - Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony.
   4. **PowerShell** - Soubor skriptu Azure PowerShellu, který můžete použít k nasazení šablony.
   5. **.NET** - Třída .NET, kterou můžete použít k nasazení šablony.
   6. **Ruby** - Třída Ruby, kterou můžete použít k nasazení šablony.

     Soubory jsou k dispozici prostřednictvím odkazů v rámci okna. Ve výchozím nastavení zobrazí okno šablonu.

       ![zobrazení šablony](./media/resource-manager-export-template/view-template.png)

     Pojďme se teď na šablonu podívat trochu podrobněji. Vaše šablona by měla vypadat nějak takto:

        {     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",     "contentVersion": "1.0.0.0",     "parameters": {       "name": {         "type": "String"       },       "accountType": {         "type": "String"       },       "location": {         "type": "String"       },       "encryptionEnabled": {         "defaultValue": false,         "type": "Bool"       }     },     "resources": [       {         "type": "Microsoft.Storage/storageAccounts",         "sku": {           "name": "[parameters('accountType')]"         },         "kind": "Storage",         "name": "[parameters('name')]",         "apiVersion": "2016-01-01",         "location": "[parameters('location')]",         "properties": {           "encryption": {             "services": {               "blob": {                 "enabled": "[parameters('encryptionEnabled')]"               }             },             "keySource": "Microsoft.Storage"           }         }       }     ]   }
 
Toto je skutečná šablona použitá k vytvoření vašeho účtu úložiště. Všimněte si, že obsahuje parametry, které vám umožňují nasadit různé typy účtů úložiště. Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md). Úplný seznam funkcí, které můžete použít v šabloně, je uveden v tématu [Funkce šablony Azure Resource Manageru](resource-group-template-functions.md).


## Přidání virtuální sítě

Šablona, kterou jste si stáhli v předchozí části, představovala infrastrukturu pro dané původní nasazení. Nezohlední se v ní však žádné změny, které provedete po nasazení.
Pro ilustraci tohoto problému pojďme upravit skupinu prostředků přidáním virtuální sítě prostřednictvím portálu.

1. V okně skupiny prostředků vyberte **Přidat**.

      ![přidání prostředku](./media/resource-manager-export-template/add-resource.png)

2. Z dostupných prostředků vyberte **Virtuální síť**.

      ![výběr virtuální sítě](./media/resource-manager-export-template/select-vnet.png)

2. Virtuální síť pojmenujte **VNET** a pro ostatní vlastnosti použijte výchozí hodnoty. Vyberte **Vytvořit**.

      ![nastavení upozornění](./media/resource-manager-export-template/create-vnet.png)

3. Po úspěšném nasazení virtuální sítě do skupiny prostředků si znovu zobrazte historii nasazení. Uvidíte teď dvě nasazení. Pokud nevidíte druhé nasazení, bude možná potřeba zavřít okno skupiny prostředků a znovu je otevřít. Vyberte novější nasazení.

      ![historie nasazení](./media/resource-manager-export-template/deployment-history.png)

4. Podívejte se na šablonu pro toto nasazení. Všimněte si, že definuje pouze změny, které jste provedli, když jste přidávali virtuální síť.

Obecně je vhodné pracovat se šablonou, která nasadí veškerou infrastrukturu pro vaše řešení v jediné operaci. Tento přístup je spolehlivější než pamatovat na mnoho různých šablon, které je třeba nasadit.


## Export šablony ze skupiny prostředků

I když každé nasazení zobrazuje pouze změny, které jste provedli ve vaší skupině prostředků, můžete šablonu kdykoli vyexportovat a zobrazit si atributy celé skupiny prostředků.  

1. Pokud chcete zobrazit šablonu pro skupinu prostředků, vyberte **Skript automatizace**.

      ![export skupiny prostředků](./media/resource-manager-export-template/export-resource-group.png)

     Ne všechny typy prostředků podporují funkci exportu šablony. Pokud vaše skupina prostředků obsahuje pouze účet úložiště a virtuální síť uvedené v tomto článku, nezobrazí se vám žádná chyba. Pokud jste však vytvořili další typy prostředků, může se zobrazit chyba, že nastal problém s exportem. Postup řešení těchto problémů najdete v části [Oprava problémů s exportem](#fix-export-issues).

      

2. Opět uvidíte šest souborů, které můžete použít k opětovnému nasazení řešení, tentokrát se ale šablona mírně liší. Tato šablona obsahuje pouze dva parametry: jeden pro název účtu úložiště a jeden pro název virtuální sítě.

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

     Resource Manager nenačetl šablony, které jste použili během nasazení. Místo toho vygeneroval novou šablonu, která je založená na aktuální konfiguraci prostředků. Šablona například nastaví umístění účtu úložiště a hodnotu replikace takto:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Stáhněte si šablonu, abyste na ní mohli pracovat místně.

      ![stažení šablony](./media/resource-manager-export-template/download-template.png)

4. Vyhledejte soubor .zip, který jste si stáhli, a vyextrahujte obsah. Tuto staženou šablonu můžete použít k opětovnému nasazení své infrastruktury.

## Oprava problémů s exportem

Ne všechny typy prostředků podporují funkci exportu šablony. Resource Manager konkrétně neexportuje určité typy prostředků, aby se předešlo k úniku citlivých dat. Pokud máte například v konfiguraci webu připojovací řetězec, pravděpodobně jej nechcete v exportované šabloně explicitně ukazovat. Tento problém můžete obejít ručním přidáním chybějících prostředků zpět do šablony.

> [AZURE.NOTE] K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení. Pokud vaše poslední nasazení přesně reprezentuje aktuální stav skupiny prostředků, měli byste šablonu exportovat z historie nasazení, ne ze skupiny zdrojů. Ze skupiny zdrojů exportujte pouze tehdy, pokud jste ve skupině prostředků provedli změny, které nejsou definovány v jedné šabloně.

Pokud například exportujete šablonu pro skupinu prostředků, která v konfiguraci webu obsahuje webovou aplikaci, databázi SQL a připojovací řetězec, zobrazí se následující zpráva.

![zobrazení chyby](./media/resource-manager-export-template/show-error.png)

Výběrem zprávy zobrazíte, které typy prostředků nebyly exportovány. 
     
![zobrazení chyby](./media/resource-manager-export-template/show-error-details.png)

Toto téma ukazuje následující běžné opravy. K implementaci těchto prostředků je třeba do šablony přidat parametry. Další informace najdete v článku o [přizpůsobení a opětovné nasazení exportované šablony](resource-manager-customize-template.md).

### Připojovací řetězec

V prostředku webu přidejte definici pro připojovací řetězec do databáze:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### Rozšíření webu

V prostředku webu přidejte definici pro kód k instalaci:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### Rozšíření virtuálního počítače

Příklady rozšíření virtuálních počítačů najdete v článku [Ukázky konfigurace rozšíření virtuálních počítačů Azure s Windows](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### Brána virtuální sítě

Přidání prostředku typu brána virtuální sítě.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### Brána místní sítě

Přidání prostředku typu brána místní sítě.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### Připojení

Přidání prostředku typu připojení.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## Další kroky

Blahopřejeme! Naučili jste se, jak vyexportovat šablonu z prostředků, které jste vytvořili na portálu.

- V druhé části tohoto kurzu si šablonu, kterou jste si stáhli, přizpůsobíte přidáním dalších parametrů a znovu ji nasadíte pomocí skriptu. Viz [Přizpůsobení a opětovné nasazení exportované šablony](resource-manager-customize-template.md).
- Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).
- Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).



<!----HONumber=Aug16_HO4-->


