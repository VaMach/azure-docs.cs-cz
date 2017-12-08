---
title: "Monitorovací a diagnostické Windows kontejnerů v Azure Service Fabric | Microsoft Docs"
description: "Nastavení monitorování a Diagnostika pro kontejner Windows orchestrovat na Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 69a59ea9fb93f6e9f3f3eea66b1a9e973b1b4eea
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="monitor-windows-containers-on-service-fabric-using-oms"></a>Monitorování Windows kontejnerů v Service Fabric pomocí OMS

Toto je část tři kurzu a vás provede procesem nastavení OMS monitorování Windows kontejnerů orchestrovat v Service Fabric.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace OMS pro váš cluster Service Fabric
> * K zobrazení a dotaz protokoly z kontejnerů a uzly použít k pracovnímu prostoru OMS
> * Konfigurace agenta OMS na vyzvednutí kontejneru a metriky uzlu

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, proveďte následující kroky:
- Máte cluster v Azure, nebo [vytvořit v tomto kurzu](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Nasazení do něj kontejnerizované aplikaci](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Nastavení OMS k vašemu clusteru v šabloně Resource Manager

V případě, že jste použili [šablony poskytuje](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) v první části tohoto kurzu, měl by obsahovat těmito přídavky na Obecné šablony služby infrastruktury Azure Resource Manager. V případě tento případ, že máte cluster vlastní, který hledáte pro nastavení pro monitorování kontejnery s OMS:
* Proveďte následující změny do šablony Resource Manageru.
* Nasazení pomocí prostředí PowerShell pro upgrade tohoto clusteru [nasazení šablony](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm). Azure Resource Manager rozpoznává, aby existoval, takže bude zavádět ho jako upgrade.

### <a name="adding-oms-to-your-cluster-template"></a>Přidávání do šablony clusteru OMS

Proveďte následující změny v vaše *template.json*:

1. Přidat pracovní prostor OMS umístění a název pro vaše *parametry* části:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your OMS Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your OMS workspace"
      }
    }
    ```

    Chcete-li změnit hodnotu použitou pro buď přidejte stejné parametry, které vaše *template.parameters.json* a změňte hodnoty používané existuje.

2. Přidejte název řešení a řešení, aby vaše *proměnné*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Přidáte agenta Microsoft Monitoring Agent OMS jako rozšíření virtuálního počítače. Vyhledání prostředků sadách škálování virtuálních počítačů: *prostředky* > *"apiVersion": "[variables('vmssApiVersion')]"*. V části *vlastnosti* > *virtualMachineProfile* > *extensionProfile* > *rozšíření*, přidejte následující popis rozšíření pod *ServiceFabricNode* rozšíření: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Přidáte pracovní prostor OMS jako jednotlivé zdroje. V *prostředky*po sadách škálování virtuálních počítačů prostředků, přidejte následující:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Zde](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) je ukázka šablonu (použité v část, jeden v tomto kurzu), která obsahuje všechny tyto změny, které můžete použít podle potřeby. Tyto změny přidá k pracovnímu prostoru analýzy protokolů OMS do vaší skupiny prostředků. V pracovním prostoru bude nakonfigurovat tak, aby odráželo události platformy Service Fabric z úložiště tabulek nakonfigurované [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) agenta. Agent OMS (Microsoft Monitoring Agent) také byla přidána do každého uzlu v clusteru jako rozšíření virtuálního počítače – to znamená, že při změně velikosti vašeho clusteru, agent je automaticky nakonfigurované na každém počítači a připojených k ve stejném pracovním prostoru.

Nasazení šablony s nové změny k upgradu aktuální cluster. Po dokončení byste měli vidět OMS prostředky ve vaší skupině prostředků. Až bude cluster připravený, nasaďte kontejnerizované aplikace k němu. V dalším kroku nastavíme monitorování kontejnerů.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>Přidat do pracovního prostoru OMS řešením pro monitorování kontejneru

Chcete-li nastavit řešení kontejneru v pracovním prostoru, vyhledejte *řešení monitorování kontejneru* a vytvořte prostředek kontejnery (v oddíle monitorování a správu kategorie).

![Přidání kontejnery řešení](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Po zobrazení výzvy k *pracovním prostorem OMS*, vyberte pracovní prostor, který byl vytvořen ve vaší skupině prostředků a klikněte na **vytvořit**. Tím se přidá *řešení monitorování kontejneru* do pracovního prostoru, automaticky způsobí, že agent OMS nasazené šablonou spustit shromažďování protokolů docker a statistiky. 

Vraťte se na vaše *skupiny prostředků*, kde byste teď měli vidět nově přidané řešení monitorování. Pokud kliknete na tlačítko do ní, cílová stránka by měla ukazují počet bitových kopií kontejneru máte systémem. 

*Všimněte si, že byl spuštěn 5 instance Moje společnost fabrikam kontejneru z [součástí dva](service-fabric-host-app-in-a-container.md) kurzu*

![Kontejner řešení cílová stránka](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Kliknutím na tlačítko do **řešení monitorování kontejneru** přejdete na podrobnější řídicí panel, který umožňuje procházet více panelů a také spouštět dotazy v analýzy protokolů. 

*Všimněte si, že se od září 2017 řešení prochází přes některé aktualizace - ignorovat všechny chyby, které se můžou zobrazit o událostech Kubernetes jako pracujeme na integraci několika orchestrators do stejného řešení.*

Vzhledem k tomu, že agent je výběr docker protokoly, výchozí hodnota pro zobrazení *stdout* a *stderr*. Pokud jste se posuňte doprava, zobrazí se kontejner image inventáře, stavu, metriky a ukázkové dotazy, které můžete spustit získat další užitečné data. 

![Řídicí panel řešení kontejneru](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Kliknutím na do některé z těchto panelů se dostanete k analýze protokolů dotaz, který generuje zobrazená hodnota. Změňte dotaz pro  *\**  zobrazíte všechny různé druhy protokoly, které jsou právě zachyceny. Tady můžete dotazovat nebo filtru pro kontejner výkonu, protokoly, nebo podívejte se na události platformy Service Fabric. Agenty nástroje jsou také neustále emitování prezenční signál z každého uzlu, který můžete prohlížet a ujistěte se, že stále probíhá shromáždění dat z vašich počítačů Pokud se změní konfiguraci clusteru.   

![Kontejner dotazu](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>Konfigurace agenta OMS na vyzvednutí čítače výkonu

Další výhodou používání agenta OMS je možné změnit čítače výkonu, které chcete vyzvednutí prostřednictvím OMS uživatelské rozhraní, namísto nutnosti konfigurovat Azure diagnostics agenta a provádět Resource Manager upgrade pokaždé, když na základě šablony. Chcete-li to provést, klikněte na **portálu OMS** na cílovou stránku vašeho řešení monitorování kontejneru (nebo Service Fabric).

![Portál OMS](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

To bude trvat můžete do pracovního prostoru na portálu OMS, kde můžete zobrazit vaše řešení, vytvořit vlastní řídicí panely, jakož i Konfigurace agenta OMS. 
* Klikněte na **ozubené kolo wheel** v pravém horním rohu obrazovky a otevře *nastavení* nabídky.
* Klikněte na **připojené zdroje** > **servery Windows** a ověřte, že máte *připojené 5 počítače se systémem Windows*.
* Klikněte na **Data** > **čítačů výkonu systému Windows** vyhledat a přidat nové čítače výkonu. Zde se zobrazí seznam doporučení od OMS pro čítače výkonu, že můžete shromažďovat a také možnost vyhledat další čítače. Klikněte na tlačítko **přidat vybrané čítače výkonu** spustit shromažďování navrhované metriky.

    ![Čítače výkonu](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Zpět na portálu Azure **aktualizovat** řešení monitorování kontejneru v pár minut a by se měl spustit zobrazíte *výkon počítače* příchozí data. To vám pomůže pochopit, jak jsou používány vašich prostředků. Tyto metriky můžete použít také k příslušné rozhodnutí o škálování clusteru nebo k potvrzení, pokud je cluster vyrovnávání zatížení, podle očekávání.

*Poznámka: Ujistěte se, že čas filtry jsou správně nastavené pro vás využívat tyto metriky.* 

![Čítače výkonu 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace OMS pro váš cluster Service Fabric
> * K zobrazení a dotaz protokoly z kontejnerů a uzly použít k pracovnímu prostoru OMS
> * Konfigurace agenta OMS na vyzvednutí kontejneru a metriky uzlu

Teď, když jste nastavili monitorování pro aplikaci kontejnerizované, zkuste následující postup:

* Nastavte OMS pro cluster s podporou Linux následující podobným způsobem, jak je uvedeno výše. Referenční dokumentace [této šablony](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) provést změny v šabloně Resource Manager.
* Konfigurace OMS nastavit [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) které pomáhají při zjišťování a diagnostiky.
* Prozkoumejte Service Fabric seznam [doporučená čítače výkonu](service-fabric-diagnostics-event-generation-perf.md) konfigurace pro clusterů.
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů.