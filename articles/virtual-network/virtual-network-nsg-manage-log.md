---
title: "Sledování operací, události a čítače pro skupiny Nsg | Microsoft Docs"
description: "Informace o povolení čítače, události a provozní protokolování pro skupiny Nsg"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: 552f37dd704de25159bc0f0ad34fdae9ed8b73f5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Analýza protokolu pro skupiny zabezpečení sítě (NSG)

Pro skupiny Nsg můžete povolit v těchto kategoriích protokolů diagnostiky:

* **Události:** obsahuje položky, pro které skupina NSG pravidla, použije se pro virtuální počítače a instance rolí na základě adresy MAC. Stav pro tato pravidla se shromažďují každých 60 sekund.
* **Čítač pravidel:** obsahuje položky pro jednotlivé skupiny NSG počet použití pravidla se aplikuje zakázat nebo povolit provoz.

> [!NOTE]
> Diagnostické protokoly jsou dostupné pouze pro skupiny Nsg nasazené prostřednictvím modelu nasazení Azure Resource Manager. Nelze povolit protokolování diagnostiky pro skupiny Nsg nasazené prostřednictvím modelu nasazení classic. Lépe porozumět obou modelů, odkazovat [modelech nasazení Azure Principy](../resource-manager-deployment-model.md) článku.

Ve výchozím nastavení je u skupiny Nsg vytvořena prostřednictvím buď modelu nasazení Azure povoleno protokolování aktivity (dříve označované jako auditování nebo provozní protokoly). Pokud chcete zjistit, které operace byly dokončeny podle skupin Nsg v protokolu aktivit, vyhledejte položky, které obsahují následující typy prostředků: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Pro čtení [přehled protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) článku Další informace o protokoly aktivity. 

## <a name="enable-diagnostic-logging"></a>Povolení protokolování diagnostiky

Musí být povoleno protokolování diagnostiky pro *každý* NSG, které chcete shromažďovat data. [Přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článek vysvětluje, kde lze odesílat diagnostické protokoly. Pokud nemáte existující skupina NSG, proveďte kroky v [vytvořit skupinu zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md) článek k jeho vytvoření. Můžete povolit NSG diagnostické protokolování pomocí kteréhokoli z následujících metod:

### <a name="azure-portal"></a>portál Azure

Povolení protokolování, přihlášení, které budou používat portál [portál](https://portal.azure.com). Klikněte na tlačítko **další služby**, pak zadejte *skupin zabezpečení sítě*. Vyberte NSG, které chcete povolit protokolování. Postupujte podle pokynů pro jiný výpočetní prostředky v [povolení diagnostických protokolů na portálu](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyberte **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, nebo obě kategorie protokolů.

### <a name="powershell"></a>PowerShell

Chcete-li povolit protokolování pomocí prostředí PowerShell, postupujte podle pokynů v [povolení diagnostických protokolů pomocí prostředí PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyhodnoťte následující informace před zadáním příkazu z článku:

- Můžete určit hodnotu pro `-ResourceId` parametr nahrazením následující [text], podle potřeby, pak zadáte příkaz `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. ID výstup z tohoto příkazu bude vypadat podobně jako */subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Pokud chcete shromažďovat data z protokolu kategorie přidat `-Categories [category]` na konec příkazu v následujícím článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*. Pokud nepoužijete `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.

### <a name="azure-command-line-interface-cli"></a>Rozhraní příkazového řádku Azure (CLI)

Chcete-li povolit protokolování pomocí rozhraní příkazového řádku, postupujte podle pokynů [povolení diagnostických protokolů prostřednictvím rozhraní příkazového řádku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyhodnoťte následující informace před zadáním příkazu z článku:

- Můžete určit hodnotu pro `-ResourceId` parametr nahrazením následující [text], podle potřeby, pak zadáte příkaz `azure network nsg show [resource-group-name] [nsg-name]`. ID výstup z tohoto příkazu bude vypadat podobně jako */subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Pokud chcete shromažďovat data z protokolu kategorie přidat `-Categories [category]` na konec příkazu v následujícím článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*. Pokud nepoužijete `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.

## <a name="logged-data"></a>Data protokolu

Data ve formátu JSON je napsán pro oba protokoly. Konkrétní data, zapsaná pro každý typ protokolu je uvedené v následujících částech:

### <a name="event-log"></a>V protokolu událostí
Tento protokol obsahuje informace o které skupina NSG pravidla, použije se pro virtuální počítače a instance role služby, na základě adresy MAC v cloudu. Pro všechny události se protokolují tato data příklad:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Pravidlo čítače protokolu

Tento protokol obsahuje informace o každé pravidlo použito pro prostředky. Pokaždé, když se použije pravidlo se protokolují tato data příklad:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Zobrazení a analýza protokolů

Zjistěte, jak zobrazit data protokolu aktivit, přečtěte si téma [přehled protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku. Zjistěte, jak zobrazit data protokolů diagnostiky, přečtěte si téma [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku. Pokud odešlete diagnostická data k analýze protokolů, můžete použít [skupinu zabezpečení sítě Azure analytics](../log-analytics/log-analytics-azure-networking-analytics.md) řešení pro správu (preview) pro rozšířené statistiky. 
