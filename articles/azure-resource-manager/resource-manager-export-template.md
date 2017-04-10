---
title: "Vyexportování šablony Azure Resource Manageru | Dokumentace Microsoftu"
description: "Pomocí Azure Resource Manageru si můžete vyexportovat šablonu z existující skupiny prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/30/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: f41fbee742daf2107b57caa528e53537018c88c6
ms.openlocfilehash: cee4748a0b24e11cd8a8ee46471418680fcf7b33
ms.lasthandoff: 03/31/2017


---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Vyexportování šablony Azure Resource Manageru z existujících prostředků
Resource Manager vám umožňuje exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném. Z vygenerované šablony pak zjistíte syntaxi šablony a podle potřeby pak můžete automatizovat opakované nasazení svého řešení.

Je důležité si uvědomit, že jsou dva různé způsoby exportu šablony:

* Je možné exportovat skutečnou šablonu, kterou jste použili k nasazení. Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně. Tento přístup je užitečný, pokud jste prostředky nasadili prostřednictvím portálu a teď se chcete podívat, jak sestavit šablonu pro vytváření těchto prostředků.
* Můžete exportovat šablonu, která představuje aktuální stav skupiny prostředků. Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení. Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků. Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali. Tento přístup je užitečný, pokud jste od nasazení změnili skupinu prostředků prostřednictvím portálu nebo skriptů a teď potřebujete zachytit skupinu prostředků jako šablonu.

Toto téma ukazuje oba přístupy.

V tomto kurzu se přihlásíte na web Azure Portal, vytvoříte účet úložiště a vyexportujete šablonu pro daný účet úložiště. Přidáte virtuální síť, čímž upravíte skupinu prostředků. Nakonec vyexportujete novou šablonu, která představuje aktuální stav skupiny. Přestože se tento článek zaměřuje na zjednodušenou infrastrukturu, můžete stejným postupem vyexportovat i šablonu pro složitější řešení.

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
1. Na webu [Azure Portal](https://portal.azure.com) vyberte **Nový** > **Úložiště** > **Účet úložiště**.
   
      ![vytvoření úložiště](./media/resource-manager-export-template/create-storage.png)
2. Vytvořte účet úložiště s názvem **storage**, vašimi iniciálami a datem. Název účtu úložiště musí být jedinečný v rámci Azure. Pokud se název už používá, zobrazí se chybová zpráva oznamující, že se název používá. Vyzkoušejte jinou variantu. Pro skupinu prostředků vyberte **Vytvořit novou** a nazvěte ji **ExportGroup**. Pro ostatní vlastnosti můžete použít výchozí hodnoty. Vyberte **Vytvořit**.
   
      ![zadání hodnot pro úložiště](./media/resource-manager-export-template/provide-storage-values.png)

Nasazení může trvat minutu. Po dokončení nasazení obsahuje vaše předplatné účet úložiště.

## <a name="view-a-template-from-deployment-history"></a>Zobrazení šablony z historie nasazení
1. Přejděte do okna nové skupiny prostředků. Všimněte si, že okno zobrazuje výsledek posledního nasazení. Vyberte tento odkaz.
   
      ![okno skupiny prostředků](./media/resource-manager-export-template/resource-group-blade.png)
2. Zobrazí se vám historie nasazení pro skupinu. Ve vašem případě bude okno pravděpodobně uvádět jenom jedno nasazení. Vyberte ho.
   
     ![poslední nasazení](./media/resource-manager-export-template/last-deployment.png)
3. Okno zobrazí souhrn vybraného nasazení. Souhrn obsahuje stav nasazení a jeho operací a také hodnoty, které jste zadali pro parametry. Pokud chcete zobrazit šablonu, kterou jste použili k nasazení, vyberte možnost **Zobrazit šablonu**.
   
     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/deployment-summary.png)
4. Resource Manager pro vás načte následujících sedm souborů:
   
   1. **Template** - Šablona, která definuje infrastrukturu pro vaše řešení. Když jste prostřednictvím portálu vytvářeli účet úložiště, Resource Manager k jeho nasazení použil šablonu a tuto šablonu uložil pro budoucí použití.
   2. **Parameters** - Soubor s parametry, který slouží k předávání hodnot během nasazení. Obsahuje hodnoty, které jste zadali při prvním nasazení. Kteroukoli z nich ale můžete při opětovném nasazování šablony změnit.
   3. **CLI** - Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony.
   3. **CLI 2.0** – Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony
   4. **PowerShell** - Soubor skriptu Azure PowerShellu, který můžete použít k nasazení šablony.
   5. **.NET** - Třída .NET, kterou můžete použít k nasazení šablony.
   6. **Ruby** - Třída Ruby, kterou můžete použít k nasazení šablony.
      
      Soubory jsou k dispozici prostřednictvím odkazů v rámci okna. Ve výchozím nastavení zobrazí okno šablonu.
      
       ![zobrazení šablony](./media/resource-manager-export-template/view-template.png)
      
      Pojďme se teď na šablonu podívat trochu podrobněji. Vaše šablona by měla vypadat nějak takto:
      
      ```json
      {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "name": {
            "type": "String"
          },
          "accountType": {
            "type": "String"
          },
          "location": {
            "type": "String"
          },
          "encryptionEnabled": {
            "defaultValue": false,
            "type": "Bool"
          }
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
              "name": "[parameters('accountType')]"
            },
            "kind": "Storage",
            "name": "[parameters('name')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "properties": {
              "encryption": {
                "services": {
                  "blob": {
                    "enabled": "[parameters('encryptionEnabled')]"
                  }
                },
                "keySource": "Microsoft.Storage"
              }
            }
          }
        ]
      }
      ```

Toto je skutečná šablona použitá k vytvoření vašeho účtu úložiště. Všimněte si, že obsahuje parametry, které vám umožňují nasadit různé typy účtů úložiště. Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md). Úplný seznam funkcí, které můžete použít v šabloně, je uveden v tématu [Funkce šablony Azure Resource Manageru](resource-group-template-functions.md).

## <a name="add-a-virtual-network"></a>Přidání virtuální sítě
Šablona, kterou jste si stáhli v předchozí části, představovala infrastrukturu pro dané původní nasazení. Nezohlední se v ní však žádné změny, které provedete po nasazení.
Pro ilustraci tohoto problému pojďme upravit skupinu prostředků přidáním virtuální sítě prostřednictvím portálu.

1. V okně skupiny prostředků vyberte **Přidat**.
   
      ![přidání prostředku](./media/resource-manager-export-template/add-resource.png)
2. Z dostupných prostředků vyberte **Virtuální síť**.
   
      ![výběr virtuální sítě](./media/resource-manager-export-template/select-vnet.png)
3. Virtuální síť pojmenujte **VNET** a pro ostatní vlastnosti použijte výchozí hodnoty. Vyberte **Vytvořit**.
   
      ![nastavení upozornění](./media/resource-manager-export-template/create-vnet.png)
4. Po úspěšném nasazení virtuální sítě do skupiny prostředků si znovu zobrazte historii nasazení. Uvidíte teď dvě nasazení. Pokud nevidíte druhé nasazení, bude možná potřeba zavřít okno skupiny prostředků a znovu je otevřít. Vyberte novější nasazení.
   
      ![historie nasazení](./media/resource-manager-export-template/deployment-history.png)
5. Zobrazte šablonu pro toto nasazení. Všimněte si, že definuje jenom virtuální síť. Nezahrnuje účet úložiště, který jste nasadili dříve. Už nemáte šablonu, která představuje všechny prostředky ve skupině prostředků.

## <a name="export-the-template-from-resource-group"></a>Export šablony ze skupiny prostředků
Pokud chcete získat informace o aktuálním stavu skupiny prostředků, vyexportujte šablonu, která zobrazuje snímek skupiny prostředků.  

> [!NOTE]
> Nejde exportovat šablonu pro skupinu prostředků, která má víc než 200 prostředků.
> 
> 

1. Pokud chcete zobrazit šablonu pro skupinu prostředků, vyberte **Skript automatizace**.
   
      ![export skupiny prostředků](./media/resource-manager-export-template/export-resource-group.png)
   
     Ne všechny typy prostředků podporují funkci exportu šablony. Pokud vaše skupina prostředků obsahuje jenom účet úložiště a virtuální síť uvedené v tomto článku, nezobrazí se vám žádná chyba. Pokud jste však vytvořili další typy prostředků, může se zobrazit chyba, že nastal problém s exportem. Postup řešení těchto problémů najdete v části [Oprava problémů s exportem](#fix-export-issues).
2. Opět uvidíte šest souborů, které můžete použít k opětovnému nasazení řešení, tentokrát se ale šablona mírně liší. Tato šablona obsahuje pouze dva parametry: jeden pro název účtu úložiště a jeden pro název virtuální sítě.

   ```json
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
   ```
   
   Resource Manager nenačetl šablony, které jste použili během nasazení. Místo toho vygeneroval novou šablonu, která je založená na aktuální konfiguraci prostředků. Šablona například nastaví umístění účtu úložiště a hodnotu replikace takto:

   ```json 
   "location": "northeurope",
   "tags": {},
   "properties": {
     "accountType": "Standard_RAGRS"
   },
   ```
3. Máte pár možností, jak s touto šablonou dále pracovat. Šablonu si můžete stáhnout a pracovat na ní místně v editoru JSON nebo si ji můžete uložit do knihovny a pracovat s ní prostřednictvím portálu.
   
     Pokud se vám s editorem JSON, jako je [VS Code](resource-manager-vs-code.md) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), dobře pracuje, můžete si ji stáhnout místně a použít tento editor. Pokud editor JSON nechcete používat, můžete preferovat úpravy šablony prostřednictvím portálu. Ve zbývající části tohoto tématu se předpokládá, že máte šablonu uloženou v knihovně na portálu. Stejné změny syntaxe ale můžete v šabloně provést, ať pracujete místně v editoru JSON nebo prostřednictvím portálu.
   
     Pokud chcete pracovat místně, vyberte **Stáhnout**.
   
      ![stažení šablony](./media/resource-manager-export-template/download-template.png)
   
     Pokud chcete pracovat přes portál, vyberte **Přidat do knihovny**.
   
      ![přidání do knihovny](./media/resource-manager-export-template/add-to-library.png)
   
     Když přidáváte šablonu do knihovny, uveďte její název a popis. Potom vyberte **Uložit**.
   
     ![nastavení hodnot šablony](./media/resource-manager-export-template/set-template-values.png)
4. Pokud chcete šablonu uloženou v knihovně zobrazit, vyberte **Další služby**, napište **Šablony**, abyste vyfiltrovali výsledky, a vyberte **Šablony**.
   
      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)
5. Vyberte šablonu s názvem, který jste uložili.
   
      ![výběr šablony](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Přizpůsobení šablony
Vyexportovaná šablona funguje správně, pokud chcete vytvořit stejný účet úložiště a virtuální síť pro všechna nasazení. Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon. Během nasazování můžete například chtít zadat typ účtu úložiště, který se má vytvořit, nebo hodnoty, které se mají používat pro předponu adresy virtuální sítě a předponu podsítě.

V této části do vyexportované šablony přidáte parametry, abyste ji mohli použít při nasazování těchto prostředků do dalších prostředí. Také do šablony přidáte některé funkce, aby se snížila pravděpodobnost, že při nasazování šablony dojde k nějaké chybě. Už nemusíte metodou pokus-omyl zkoušet vytvořit jedinečný název pro účet úložiště. Šablona vytvoří jedinečný název sama. Omezíte hodnoty, které je možné zadat pro typ účtu úložiště, jenom na platné možnosti.

1. Vyberte **Upravit** pro přizpůsobení šablony.
   
     ![zobrazení šablony](./media/resource-manager-export-template/show-template.png)
2. Vyberte šablonu.
   
     ![Úprava šablony](./media/resource-manager-export-template/edit-template.png)
3. Abyste mohli předat hodnoty, které byste mohli chtít zadat během nasazování, nahraďte oddíl **parameters** novými definicemi parametrů. Všimněte si hodnot **allowedValues** pro **storageAccount_accountType**. Pokud nechtěně zadáte neplatnou hodnotu, rozpozná se tato chyba ještě před zahájením nasazování. Ještě je třeba upozornit na to, že zadáváte pouze předponu názvu účtu úložiště a předpona je omezena na 11 znaků. Když předponu omezíte na 11 znaků, zajistíte tím, že úplný název nebude delší než maximální povolený počet znaků pro účet úložiště. Předpona umožňuje pro účty úložiště použít zásady vytváření názvů. To, jak vytvořit jedinečný název v dalším kroku, si ukážeme v dalším kroku.

   ```json
   "parameters": {
     "storageAccount_prefix": {
       "type": "string",
       "maxLength": 11
     },
     "storageAccount_accountType": {
       "defaultValue": "Standard_RAGRS",
       "type": "string",
       "allowedValues": [
         "Standard_LRS",
         "Standard_ZRS",
         "Standard_GRS",
         "Standard_RAGRS",
         "Premium_LRS"
       ]
     },
     "virtualNetwork_name": {
       "type": "string"
     },
     "addressPrefix": {
       "defaultValue": "10.0.0.0/16",
       "type": "string"
     },
     "subnetName": {
       "defaultValue": "subnet-1",
       "type": "string"
     },
     "subnetAddressPrefix": {
       "defaultValue": "10.0.0.0/24",
       "type": "string"
     }
   },
   ```

4. Oddíl **variables** šablony je teď prázdný. V oddílu **variables** vytvořte hodnoty, které zjednodušují syntaxi pro zbytek šablony. Tuto část nahraďte novou definicí proměnné. Proměnná **StorageAccount_name** zřetězí předponu z parametru do jedinečného řetězce, který se generuje na základě identifikátoru skupiny prostředků. Při zadávání hodnoty parametru už nemusíte odhadovat jedinečný název.

   ```json
   "variables": {
     "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
   },
   ```

5. Abyste mohli použít parametry a proměnnou v definicích prostředků, nahraďte oddíl **resources** novými definicemi prostředku. Všimněte si, že v definicích prostředků se toho moc nezměnilo – jenom hodnota, která je přiřazená k vlastnosti prostředků. Vlastnosti jsou stejné jako vlastnosti z vyexportované šablony. Jednoduše místo pevně definovaných hodnot přiřazujete hodnotám parametrů vlastnosti. Umístění prostředků je nastavené tak, aby se používalo stejné umístění jako pro skupinu prostředků, a to prostřednictvím výrazu **resourceGroup().location**. Na proměnnou, kterou jste vytvořili pro název účtu úložiště, se odkazuje prostřednictvím výrazu **variables**.

   ```json
   "resources": [
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "[parameters('virtualNetwork_name')]",
       "apiVersion": "2015-06-15",
       "location": "[resourceGroup().location]",
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "[parameters('addressPrefix')]"
           ]
         },
         "subnets": [
           {
             "name": "[parameters('subnetName')]",
             "properties": {
               "addressPrefix": "[parameters('subnetAddressPrefix')]"
             }
           }
         ]
       },
       "dependsOn": []
     },
     {
       "type": "Microsoft.Storage/storageAccounts",
       "name": "[variables('storageAccount_name')]",
       "apiVersion": "2015-06-15",
       "location": "[resourceGroup().location]",
       "tags": {},
       "properties": {
         "accountType": "[parameters('storageAccount_accountType')]"
       },
       "dependsOn": []
     }
   ]
   ```

6. Po dokončení úprav šablony vyberte **OK**.
7. Uložte změny šablony kliknutím na **Uložit**.
   
     ![uložení šablony](./media/resource-manager-export-template/save-template.png)
8. Pokud chcete aktualizovanou šablonu nasadit, vyberte **Nasadit**.
   
     ![nasazení šablony](./media/resource-manager-export-template/deploy-template.png)
9. Zadejte hodnoty parametrů a vyberte novou skupinu prostředků, do které prostředky nasadíte.

## <a name="update-the-downloaded-parameters-file"></a>Aktualizace staženého souboru parametrů
Pokud pracujete se staženými soubory (a ne s knihovnou portálu), bude potřeba aktualizovat stažený soubor parametrů. Stažený soubor už neodpovídá parametrům ve vaší šabloně. Soubor parametrů nemusíte používat, ale může vám zjednodušit proces při opětovném nasazování prostředí. Výchozí hodnoty, které jsou definované v šabloně, použijete pro spoustu parametrů, takže vám v souboru parametrů stačí jenom dvě hodnoty.

Nahraďte obsah souboru parameters.json tímto kódem:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Aktualizovaný soubor parametrů obsahuje hodnoty pouze pro parametry, které nemají výchozí hodnotu. Pokud chcete zadat hodnotu, která se liší od výchozí hodnoty, můžete zadat hodnoty pro další parametry.

## <a name="fix-export-issues"></a>Oprava problémů s exportem
Ne všechny typy prostředků podporují funkci exportu šablony. Resource Manager konkrétně neexportuje určité typy prostředků, aby se předešlo k úniku citlivých dat. Pokud máte například v konfiguraci webu připojovací řetězec, pravděpodobně jej nechcete v exportované šabloně explicitně ukazovat. Tento problém můžete vyřešit ručním přidáním chybějících prostředků zpět do šablony.

> [!NOTE]
> K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení. Pokud vaše poslední nasazení přesně reprezentuje aktuální stav skupiny prostředků, měli byste šablonu exportovat z historie nasazení, ne ze skupiny zdrojů. Ze skupiny zdrojů exportujte pouze tehdy, pokud jste ve skupině prostředků provedli změny, které nejsou definovány v jedné šabloně.
> 
> 

Pokud například exportujete šablonu pro skupinu prostředků, která v konfiguraci webu obsahuje webovou aplikaci, databázi SQL a připojovací řetězec, zobrazí se následující zpráva:

![zobrazení chyby](./media/resource-manager-export-template/show-error.png)

Výběrem zprávy zobrazíte, které typy prostředků nebyly exportovány. 

![zobrazení chyby](./media/resource-manager-export-template/show-error-details.png)

Toto téma popisuje běžné opravy.

### <a name="connection-string"></a>Připojovací řetězec
V prostředku webu přidejte definici pro připojovací řetězec do databáze:

```json
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

### <a name="web-site-extension"></a>Rozšíření webu
V prostředku webu přidejte definici pro kód k instalaci:

```json
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

### <a name="virtual-machine-extension"></a>Rozšíření virtuálního počítače
Příklady rozšíření virtuálních počítačů najdete v článku [Ukázky konfigurace rozšíření virtuálních počítačů Azure s Windows](../virtual-machines/windows/extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="virtual-network-gateway"></a>Brána virtuální sítě
Přidání prostředku typu brána virtuální sítě.

```json
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

### <a name="local-network-gateway"></a>Brána místní sítě
Přidání prostředku typu brána místní sítě.

```json
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

### <a name="connection"></a>Připojení
Přidání prostředku typu připojení.

```json
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


## <a name="next-steps"></a>Další kroky
Blahopřejeme! Naučili jste se, jak vyexportovat šablonu z prostředků, které jste vytvořili na portálu.

* Šablonu můžete nasadit pomocí těchto možností: [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) nebo [REST API](resource-group-template-deploy-rest.md).
* Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).
* Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).


