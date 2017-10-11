---
title: "Automatické škálování a virtuálního počítače sady škálování | Microsoft Docs"
description: "Další informace o použití diagnostiky a škálování prostředky automaticky škálovat virtuální počítače ve škálovací sadě."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06ff9d9ae1dd8256f0d22c1a60ed6a85554f1f17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-automatic-scaling-and-virtual-machine-scale-sets"></a>Jak používat automatické škálování a sady škálování virtuálního počítače
Automatické škálování virtuálních počítačů v sadě škálování je vytvoření nebo odstranění počítačů v sadě podle potřeby tak, aby odpovídaly požadavkům na výkon. S růstem objem pracovní aplikace, může vyžadovat další zdroje, které umožní, aby efektivní provádění úloh.

Automatické škálování je automatizovaný proces, pomáhá usnadňují režie na správu. Protože se sníží režie, nemusíte průběžně monitorovat výkon systému nebo rozhodnout, jak spravovat prostředky. Škálování je elastické proces. Při rostoucí zátěži, můžete přidat další zdroje. A jak vyžádání snižuje, minimalizovat náklady a Udržovat úrovně výkonu lze odebrat prostředky.

Nastavte automatické škálování na škále nastavit pomocí šablonu Azure Resource Manager, Azure PowerShell, rozhraní příkazového řádku Azure nebo portálu Azure.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Nastavení škálování pomocí šablony Resource Manageru
Namísto nasazení a samostatně spravovat každý prostředek vaší aplikace, použijte šablonu, která nasadí všechny prostředky v rámci jediné koordinované operace. V šabloně jsou definovány prostředky aplikace a jsou zadány parametry nasazení pro různá prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení. Další informace, podívejte se na [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

V šabloně zadejte element kapacity:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

Kapacita identifikuje počet virtuálních počítačů v sadě. Můžete ručně změnit kapacitu nasazení šablony s jinou hodnotou. Pokud nasazujete šablonu, která má kapacitu, pouze změní, můžete zahrnout pouze element SKU s aktualizované kapacitu.

Kapacitu škálovací sadu pomocí kombinace automaticky upravována **autoscaleSettings** prostředků a rozšíření diagnostiky.

### <a name="configure-the-azure-diagnostics-extension"></a>Konfigurace rozšíření Azure Diagnostics
Automatické škálování provést pouze pokud se kolekce metriky úspěšné na každém virtuálním počítači v sadě škálování. Rozšíření diagnostiky Azure poskytuje možnosti monitorování a diagnostiky, které splňuje potřeby kolekce metriky automatického škálování prostředku. Rozšíření můžete nainstalovat v rámci šablony Resource Manageru.

Tento příklad ukazuje proměnné, které se používají v šabloně ke konfiguraci rozšíření diagnostiky:

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

Parametry jsou k dispozici při nasazení šablony. V tomto příkladu jsou zadat název účtu úložiště (které jsou uložená data) a název sady škálování (kdy se shromažďují data). Také v tomto příkladu systému Windows Server se shromažďují pouze počet vláken počítadla výkonu. Všechny čítače výkonu k dispozici v systému Windows nebo Linux umožňuje shromažďovat diagnostické informace a může být zahrnutý v konfiguraci rozšíření.

Tento příklad ukazuje definici rozšíření v šabloně:

```json
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
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

Při spuštění rozšíření diagnostiky, se data uložená a shromažďují v tabulce v účtu úložiště, který určíte. V **WADPerformanceCounters** tabulky, můžete najít na shromážděná data:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurace prostředků autoScaleSettings
Prostředek autoscaleSettings používá informace z rozšíření diagnostiky se rozhodnout, jestli se má zvýšit nebo snížit počet virtuálních počítačů v sadě škálování.

Tento příklad ukazuje konfiguraci prostředku v šabloně:

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
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

V příkladu nahoře vytvoří se dvě pravidla pro definování automatické škálování akce. První pravidlo definuje akce škálování a druhé pravidlo definuje akce škálování v. Tyto hodnoty jsou k dispozici v pravidlech:

| Pravidlo | Popis |
| ---- | ----------- |
| metricName        | Tato hodnota je stejná jako čítače výkonu, který jste definovali v proměnné wadperfcounter pro rozšíření diagnostiky. V předchozím příkladu se používá čítač počtu vláken.    |
| metricResourceUri | Tato hodnota je identifikátor prostředku sady škálování virtuálního počítače. Tento identifikátor obsahuje název skupiny prostředků, název zprostředkovatele prostředků a názvu sad škálování škálování. |
| Časovými úseky         | Tato hodnota je členitost metriky, které se shromažďují. V předchozím příkladu data jsou shromažďována v intervalu jedné minuty. Tato hodnota se používá s hodnota timeWindow. |
| statistiky         | Tato hodnota určuje, jak se zkombinují metriky pro přizpůsobení automatické škálování akci. Možné hodnoty jsou: průměr, minimum, maximum. |
| Hodnota timeWindow        | Tato hodnota je doba, ve kterém se shromažďují instance data. Musí být mezi 5 minutami a 12 hodin. |
| Agregace času   | Tato hodnota určuje, jak by měla být kombinovány data, která se shromažďují v čase. Výchozí hodnota je průměr. Možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet. |
| Operátor          | Tato hodnota je operátor, který se používá k porovnání data metriky a prahovou hodnotu. Možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual. |
| Prahová hodnota         | Tato hodnota je hodnota, která aktivuje akci škálování. Nezapomeňte poskytnout dostatečnou rozdíl mezi prahové hodnoty pro **Škálováním na více systémů** a **škálování v** akce. Pokud jste nastavili stejné hodnoty pro obě akce, připravená na systém konstantní změna, která zabraňuje v jeho implementaci akce škálování. Například nebude fungovat nastavení na 600 vláken v předchozím příkladu. |
| Směr         | Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty. Možné hodnoty jsou zvýšení nebo snížení. |
| type              | Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount. |
| hodnota             | Tato hodnota je počet virtuálních počítačů, které jsou přidat nebo odebrat ze sady škálování. Tato hodnota musí být 1 nebo vyšší. |
| cooldown          | Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce. Tato hodnota musí být mezi minutu a jeden týden. |

V závislosti na čítače výkonu který používáte, některé prvky v konfiguraci šablony fungují jinak. V předchozím příkladu je hodnota čítače výkonu počtu vláken, prahová hodnota je 650 pro akci škálování a prahová hodnota je 550 pro akci škálování in. Pokud používáte čítače, jako je % času procesoru, prahová hodnota nastavená na procento využití procesoru, který určuje škálování akce.

Když se aktivuje škálování akce, jako je například vysoké zatížení, je vyšší kapacita sady závislosti na hodnotě v šabloně. Například v škálování sady, kde je kapacitu nastaven na 3 a hodnotu akce škálování je nastaven na 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Pokud aktuální zatížení způsobí, že počet průměrná vláken přejdete nad prahovou hodnotou 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

A **Škálováním na více systémů** akce se aktivuje, který způsobuje, že kapacita sady do dá zvětšit o jeden:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

Výsledkem je, že virtuální počítač je přidat do sady škálování:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Po určité době cooldown pět minut je-li průměrný počet vláken na počítačích více než 600 je jiný počítač přidat do sady. Je-li počet průměrná vláken níže 550, kapacita sady škálování se sníží o jedna a počítač je odebrán ze sady.

## <a name="set-up-scaling-using-azure-powershell"></a>Nastavení škálování pomocí Azure PowerShell

Chcete-li zobrazit příklady použití prostředí PowerShell pro nastavení automatického škálování, podívejte se na [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Nastavení škálování pomocí rozhraní příkazového řádku Azure

Pokud chcete zobrazit příklady použití Azure CLI pro nastavení automatického škálování, podívejte se na [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Nastavení škálování pomocí portálu Azure

Pokud chcete zobrazit příklad nastavení automatického škálování pomocí portálu Azure, podívejte se na [vytvoření sady škálování virtuálního počítače pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Prozkoumat škálování akce

* **Azure Portal**  
Teď můžete získat omezené množství informací pomocí portálu.

* **Průzkumník prostředků Azure**  
Tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu. Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:  
**Odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > {škálovací sadu} > virtuálních počítačů**

* **Azure PowerShell**  
Určité informace, použijte tento příkaz:

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* Připojte k virtuálnímu počítači jumpbox stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.

## <a name="next-steps"></a>Další kroky
* Podívejte se na [automaticky škálovat počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-autoscale.md) příklad toho, jak vytvořit měřítko nastavit automatické škálování nakonfigurované.

* Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)

* Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).

* Další informace o tom, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

* Další informace o [pokročilé scénáře škálování](virtual-machine-scale-sets-advanced-autoscale.md).
