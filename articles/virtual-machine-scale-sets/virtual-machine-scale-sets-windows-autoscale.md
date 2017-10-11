---
title: "Sadách škálování virtuálních počítačů Windows škálování | Microsoft Docs"
description: "Nastavení automatického škálování pro Windows sadu škálování virtuálního počítače pomocí Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 91f731bec46c005221f4e66e95822994070d4c26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatické škálování počítače ve škálovací sadě virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).

V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů s Windows a automaticky škálovat na počítače v sadě. Můžete vytvořit měřítko nastavení a nastavení škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí prostředí Azure PowerShell. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md). Další informace o automatické škálování sad škálování najdete v tématu [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).

V tomto článku můžete nasadit následující prostředků a rozšíření:

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

## <a name="step-1-install-azure-powershell"></a>Krok1: Nainstalování prostředí Azure PowerShell
V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení do Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Krok 2: Vytvoření skupiny prostředků a účet úložiště
1. **Vytvořte skupinu prostředků** – všechny prostředky se musí nasadit do skupiny prostředků. Použití [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) vytvořit skupinu prostředků s názvem **vmsstestrg1**.
2. **Vytvoření účtu úložiště** – tento účet úložiště je, kde je šablona uložena. Použití [nový AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) chcete vytvořit účet úložiště s názvem **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Krok 3: Vytvoření šablony
Šablonu Azure Resource Manager umožňuje nasadit a spravovat prostředky Azure společně s použitím popis JSON prostředků a parametry přidružené nasazení.

1. Ve svém oblíbeném editoru vytvořte soubor C:\VMSSTemplate.json a přidejte počáteční struktuře JSON pro podporu šablony.

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
    * Název účtu úložiště, které šablona uložena.
    * Počet virtuálních počítačů na začátku vytvořit v sadě škálování.
    * Jméno a heslo účtu správce na virtuálních počítačích.
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
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * Názvy DNS, které používají rozhraní sítě.

        * Názvy adres IP a předpony pro virtuální sítě a podsítě.
        * Názvy a identifikátory ve virtuální síti, zatížení vyrovnávání a síťových rozhraní.
        * Nastavit názvy účtů úložiště pro účty spojené s počítači v měřítka.
        * Nastavení pro rozšíření diagnostiky, který je nainstalován na virtuálních počítačích. Další informace o rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Přidáte prostředek účet úložiště v rámci nadřazeného elementu prostředky, které jste přidali do šablony. Tato šablona používá smyčku vytvoření doporučené pět účtů úložiště uložení disků operačního systému a diagnostická data. Tuto sadu účtů může podporovat až 100 virtuální počítače ve škálovací sadě, což je aktuální maximální. Každý účet úložiště je s názvem se písmeno označení, která byla definována v proměnné v kombinaci s předponu, která zadáte v parametrech šablony.

    ```json
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
    ```

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
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
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
              "backendPort": 3389
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
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
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
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
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
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Přidáte autoscaleSettings prostředek, který definuje, jak byly sadou škálování upraví podle využití procesoru na počítačích v sadě škálování.

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
                  "metricName": "\\Processor(_Total)\\% Processor Time",
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
    Tato hodnota je stejná jako čítače výkonu, který jsme definovali v wadperfcounter proměnné. Pomocí této proměnné, rozšíření diagnostiky shromažďuje **procesor(_celkem)\% času procesoru** čítače.
    
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
    Tato hodnota je hodnota, která aktivuje akci škálování. V této šabloně počítače se přidají do sad po více než 50 % průměrné využití procesoru mezi počítači v sadě škálování.
    
    * **směr**  
    Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty. Možné hodnoty jsou zvýšení nebo snížení. V této šabloně se zvyšuje počet virtuálních počítačů v sadě škálování, když je prahová hodnota je více než 50 % v okně definovaného časového.
    
    * **Typ**  
    Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount.
    
    * **Hodnota**  
    Tato hodnota je počet virtuálních počítačů, které jsou přidány nebo odebrány ze sady škálování. Tato hodnota musí být 1 nebo vyšší. Výchozí hodnota je 1. V této šabloně počet počítačů v měřítka nastavit zvýší o 1 při splnění prahovou hodnotu.
    
    * **cooldown**  
    Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce. Tato hodnota musí být mezi minutu a jeden týden.

12. Uložte soubor šablony.    

## <a name="step-4-upload-the-template-to-storage"></a>Krok 4: Nahrání šablony do úložiště
Šablonu je možné uložit tak dlouho, dokud znáte název a primární klíč účtu úložiště, který jste vytvořili v kroku 1.

1. V okně prostředí Microsoft Azure PowerShell nastavte proměnné, která určuje název účtu úložiště, který jste vytvořili v kroku 1.
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. Nastavte proměnnou, která určuje primární klíč účtu úložiště.
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   Tento klíč můžete získat kliknutím na ikonu klíče při zobrazení prostředků účtu úložiště na portálu Azure.
3. Vytvořte objekt kontext účtu úložiště, který se používá k ověření operací s účtem úložiště.
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. Vytvořte kontejner pro uložení šablony.

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. Nahrajte soubor šablony do nového kontejneru.

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a>Krok 5: Nasazení šablony
Teď, když jste vytvořili šablonu, můžete začít nasazovat prostředky. Použijte tento příkaz ke spuštění procesu:

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

Po stisknutí klávesy zadejte, zobrazí se výzva k zadání hodnot pro proměnné, které jste přiřadili. Zadejte tyto hodnoty:

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

Má trvat přibližně 15 minut pro všechny prostředky. Chcete-li úspěšně nasadit.

> [!NOTE]
> Možnost na portálu můžete taky nasadit tyto prostředky. Použít tento odkaz: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"
> 
> 

## <a name="step-6-monitor-resources"></a>Krok 6: Sledování prostředků
Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:

* Portál Azure – můžete aktuálně získat omezené množství informací pomocí portálu.
* [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu. Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:
  
    odběry > {vaše předplatné} > Skupinyprostředků > vmsstestrg1 > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuálních počítačů

* Azure PowerShell – použijte tento příkaz určité informace:

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  Nebo
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* Připojte k samostatný virtuální počítač stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.

> [!NOTE]
> Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Krok 7: Odebrání prostředky
Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba. Nemusíte samostatně odstranit jednotlivé prostředky ze skupiny prostředků. Můžete odstranit skupinu prostředků a všechny její prostředky se automaticky odstraní.

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

Pokud chcete zachovat vaší skupiny prostředků, můžete odstranit pouze sad škálování.

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>Další kroky
* Spravovat byly sadou škálování, kterou jste právě vytvořili, pomocí informací v [spravovat virtuální počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-manage.md).
* Další informace o vertikálním škálování najdete v tématu [Vertikální automatické škálování se škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-vertical-scale-reprovision.md).
* Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Zjistěte, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

