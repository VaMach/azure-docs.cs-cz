---
title: "Sadách škálování virtuálních počítačů Linux škálování | Microsoft Docs"
description: "Nastavení automatického škálování pro Linux sadu škálování virtuálního počítače pomocí rozhraní příkazového řádku Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: eff4add1cb16fe25022787668dc1d2277845dd95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a>Automatické škálování počítače se systémem Linux v škálovací sadu virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další informace najdete v tématu [Přehled sady škálování virtuálních počítačů](virtual-machine-scale-sets-overview.md).

V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů Linux pomocí nejnovější verze Ubuntu Linux. Tento kurz také ukazuje, jak automaticky škálovat na počítače v sadě. Můžete vytvořit měřítko nastavení a nastavení škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí rozhraní příkazového řádku Azure. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md). Další informace o automatické škálování sad škálování najdete v tématu [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).

V tomto kurzu nasadíte následující prostředků a rozšíření:

* Microsoft.Storage/storageAccounts.
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).

Než začnete s kroky v tomto kurzu [nainstalovat Azure CLI](../cli-install-nodejs.md).

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a>Krok 1: Vytvoření skupiny prostředků a účet úložiště

1. **Přihlaste se k Microsoft Azure**  
Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku), přepněte do režimu Resource Manager a potom [přihlásit pomocí id váš pracovní nebo školní](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Postupujte podle pokynů pro prostředí interaktivní přihlášení k účtu Azure.

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > Pokud máte pracovní nebo školní ID a není povoleno dvoufaktorové ověřování, použijte `azure login -u` s ID přihlásit bez interaktivní relace. Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../active-directory/active-directory-users-create-azure-portal.md).
    
2. **Vytvořte skupinu prostředků**  
Všechny prostředky musí být nasazeny do skupiny prostředků. V tomto kurzu, název skupiny prostředků **vmsstest1**.
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. **Nasazení účtu úložiště do nové skupiny prostředků**  
Tento účet úložiště je, kde je šablona uložena. Vytvořit účet úložiště s názvem **vmsstestsa**.
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-the-template"></a>Krok 2: Vytvoření šablony
Šablonu Azure Resource Manager umožňuje nasadit a spravovat prostředky Azure společně s použitím popis JSON prostředků a parametry přidružené nasazení.

1. Ve svém oblíbeném editoru vytvořte soubor VMSSTemplate.json a přidejte počáteční struktuře JSON pro podporu šablony.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak zadat hodnoty při nasazení šablony. Přidejte tyto parametry v rámci nadřazeného elementu parametry, které jste přidali do šablony.

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * Nastavte název pro samostatné virtuální počítač, který se používá k přístupu k počítačům v měřítka.
   * Název pro účet úložiště, které šablona uložena.
   * Nastavit počet instancí virtuálních počítačů v měřítka nejprve vytvořit.
   * Název a heslo účtu správce na virtuálních počítačích.
   * Předpona názvu pro prostředky, které jsou vytvořené pro podporu nastavit.

3. Proměnné můžete použít v šabloně a zadejte hodnoty, které může často mění nebo hodnoty, které je potřeba vytvořit z kombinace hodnot parametrů. Přidejte tyto proměnné v rámci nadřazeného elementu proměnné, které jste přidali do šablony.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * Názvy DNS, které používají rozhraní sítě.
   * Názvy adres IP a předpony pro virtuální sítě a podsítě.
   * Názvy a identifikátory ve virtuální síti, zatížení vyrovnávání a síťových rozhraní.
   * Nastavit názvy účtů úložiště pro účty spojené s počítači v měřítka.
   * Nastavení pro rozšíření diagnostiky, který je nainstalován na virtuálních počítačích. Další informace o rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Přidáte prostředek účet úložiště v rámci nadřazeného elementu prostředky, které jste přidali do šablony. Tato šablona používá smyčku vytvoření doporučené pět účtů úložiště uložení disků operačního systému a diagnostická data. Tuto sadu účtů může podporovat až 100 virtuální počítače ve škálovací sadě, což je aktuální maximální. Každý účet úložiště je s názvem se písmeno označení, která byla definována v proměnné v kombinaci s příponou, které poskytujete parametry šablony.
   
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Přidáte prostředek virtuální sítě. Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Přidáte prostředky adresy veřejné IP adresy, které se používají k vyrovnávání zatížení a síťové rozhraní.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Přidáte prostředek pro vyrovnávání zatížení, který se používá škálovací sada. Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. Přidáte prostředek rozhraní sítě, který používá samostatný virtuální počítač. Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří ve stejné virtuální síti vzdáleně přístupu na počítače.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Přidáte samostatné virtuální počítače ve stejné síti jako sada škálování.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Přidejte prostředků sady škálování virtuálních počítačů a nastavte rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače v sadě škálování. Řadu nastavení pro tento prostředek je podobný jako prostředek virtuálního počítače. Hlavní rozdíly jsou kapacity elementu, který určuje počet virtuálních počítačů v sadě škálování a upgradePolicy, která určuje, jak jsou aktualizace provedené na virtuální počítače. Sada škálování vytvořen až všechny účty úložiště jsou vytvořeny jako zadaný dependsOn element.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Přidejte autoscaleSettings prostředek, který definuje, jak byly sadou škálování upraví podle využití procesoru na počítačích v sadě.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor\\PercentProcessorTime",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```
    
    V tomto kurzu jsou důležité tyto hodnoty:
    
    * **metricName**  
    Tato hodnota je stejná jako čítače výkonu, který jsme definovali v wadperfcounter proměnné. Pomocí této proměnné, rozšíření diagnostiky shromažďuje **Processor\PercentProcessorTime** čítače.
    
    * **metricResourceUri**  
    Tato hodnota je identifikátor prostředku sady škálování virtuálního počítače.
    
    * **časovými úseky**  
    Tato hodnota je členitost metriky, které se shromažďují. V této šabloně je nastavena na jednu minutu.
    
    * **statistiky**  
    Tato hodnota určuje, jak se zkombinují metriky pro přizpůsobení automatické škálování akci. Možné hodnoty jsou: průměr, minimum, maximum. V této šabloně se shromažďují průměrné využití procesoru celkový počet virtuálních počítačů.

    * **Hodnota timeWindow**  
    Tato hodnota je doba, ve kterém se shromažďují instance data. Musí být mezi 5 minutami a 12 hodin.
    
    * **Agregace času**  
    jeho hodnota určuje, jak by měla být kombinovány data, která se shromažďují v čase. Výchozí hodnota je průměr. Možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.
    
    * **operátor**  
    Tato hodnota je operátor, který se používá k porovnání data metriky a prahovou hodnotu. Možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **Prahová hodnota**  
    Tato hodnota se aktivuje akci škálování. V této šabloně počítače se přidají do sad po více než 50 % průměrné využití procesoru mezi počítači v sadě škálování.
    
    * **směr**  
    Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty. Možné hodnoty jsou zvýšení nebo snížení. V této šabloně se zvyšuje počet virtuálních počítačů v sadě škálování, když je prahová hodnota je více než 50 % v okně definovaného časového.

    * **Typ**  
    Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount.
    
    * **Hodnota**  
    Tato hodnota je počet virtuálních počítačů, které jsou přidány nebo odebrány ze sady škálování. Tato hodnota musí být 1 nebo vyšší. Výchozí hodnota je 1. V této šabloně počet počítačů v měřítka nastavit zvýší o 1 při splnění prahovou hodnotu.

    * **cooldown**  
    Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce. Tato hodnota musí být mezi minutu a jeden týden.

12. Uložte soubor šablony.    

## <a name="step-3-upload-the-template-to-storage"></a>Krok 3: Nahrát šablonu do úložiště
Šablonu je možné uložit tak dlouho, dokud znáte název a primární klíč účtu úložiště, který jste vytvořili v kroku 1.

1. Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) spusťte tyto příkazy a nastavení proměnných prostředí, které jsou potřebné pro přístup k účtu úložiště:

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    Klíč můžete získat kliknutím na ikonu klíče při zobrazení prostředků účtu úložiště na portálu Azure. Při použití příkazového řádku Windows, zadejte **nastavit** místo exportu.

2. Vytvořte kontejner pro uložení šablony.
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. Nahrajte soubor šablony do nového kontejneru.
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-the-template"></a>Krok 4: Nasazení šablony
Teď, když jste vytvořili šablonu, můžete začít nasazovat prostředky. Použijte tento příkaz ke spuštění procesu:

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

Po stisknutí klávesy zadejte, zobrazí se výzva k zadání hodnot pro proměnné, které jste přiřadili. Zadejte tyto hodnoty:

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

Má trvat přibližně 15 minut pro všechny prostředky. Chcete-li úspěšně nasadit.

> [!NOTE]
> Možnost na portálu můžete taky nasadit tyto prostředky. Použít tento odkaz: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>


## <a name="step-5-monitor-resources"></a>Krok 5: Sledování prostředků
Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:

* Portál Azure – můžete aktuálně získat omezené množství informací pomocí portálu.

* [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu. Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* Azure CLI - použijte tento příkaz určité informace:

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* Připojte k virtuálnímu počítači jumpbox stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.

> [!NOTE]
> Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx).

## <a name="step-6-remove-the-resources"></a>Krok 6: Odebrat prostředky
Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba. Nemusíte samostatně odstranit jednotlivé prostředky ze skupiny prostředků. Můžete odstranit skupinu prostředků a všechny její prostředky se automaticky odstraní.

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a>Další kroky
* Najít příklady Azure monitorování monitorování funkcí v [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md)
* Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Zjistěte, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
* Podívejte se [škálování ukázkovou aplikaci na Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablony, která nastaví aplikaci Python nebo bottle vykonávat automatické škálování funkce sadách škálování virtuálního počítače.

