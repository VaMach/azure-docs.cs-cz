---
title: "Správa Azure DDoS ochrany standardní pomocí Azure PowerShell | Microsoft Docs"
description: "Naučte se spravovat Azure DDoS ochrany standardní pomocí Azure PowerShell."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: a1a3688d4ff215d05d2f78cdfa7d402e3fc20be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Správa Azure DDoS ochrany standardní pomocí Azure PowerShell

>[!IMPORTANT]
>Azure DDoS ochrany Standard (Ochrana proti útoku DDoS) se aktuálně ve verzi preview. Omezený počet prostředků Azure podpory ochrana proti útoku DDoS a vyberte počet oblastí. Budete muset [zaregistrovat pro službu](http://aka.ms/ddosprotection) během omezené preview získat ochrana proti útoku DDoS pro vaše předplatné povolený. Se vás kontaktovat tým Azure DDoS při registraci, který vás provede procesem povolování. Ochrana proti útoku DDoS je k dispozici v oblastech USA – Východ USA – západ a – Západ střední USA. Verzi Preview se vám neúčtují poplatky za používání služby.

Tento článek ukazuje, jak používat prostředí Azure PowerShell k povolení ochrana proti útoku DDoS, zakažte ochranu DDoS a zmírnit útok pomocí telemetrie.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete. Pokud je potřeba nainstalovat nebo upgradovat prostředí Azure PowerShell najdete v tématu [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection"></a>Povolit ochrana proti útoku DDoS

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Vytvoření nové virtuální sítě a povolení ochrana proti útoku DDoS

Chcete-li vytvořit virtuální síť s povolenou ochranou proti DDoS, spusťte v následujícím příkladu:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6 -EnableDDoSProtection
```

Tento příklad vytvoří virtuální síť se dvěma podsítěmi a dva servery DNS. Účinek zadání serverů DNS ve virtuální síti je, že síťové adaptéry nebo virtuální počítače, které jsou nasazeny do této virtuální síti dědit tyto servery DNS jako výchozí. Ochrana proti útoku DDoS je povoleno pro všechny chráněné prostředky ve virtuální síti.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Povolit ochrana proti útoku DDoS na existující virtuální síť

Chcete-li povolit ochrana proti útoku DDoS na existující virtuální síť, spusťte v následujícím příkladu:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks
```

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Zakažte ochranu DDoS ve virtuální síti

Chcete-li zakázat ochrana proti útoku DDoS ve virtuální síti, spusťte v následujícím příkladu:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks"
```

## <a name="review-the-ddos-protection-status-of-virtual-networks"></a>Zkontrolujte stav ochrany DDoS virtuálních sítí 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>Pomocí telemetrie ochrana proti útoku DDoS

Telemetrie na útok je zajišťováno prostřednictvím Azure monitorování v reálném čase. Telemetrie je k dispozici pouze po dobu, která veřejnou IP adresu je pod zmírnění dopadů. Neuvidíte telemetrie před nebo po zmírnit útok.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Nakonfigurujte upozornění na metriky ochrana proti útoku DDoS

Využití konfiguraci výstrah monitorování Azure, můžete vybrat všechny dostupné metriky ochrana proti útoku DDoS pro upozornění, když dojde active zmírnění při útoku.

#### <a name="configure-email-alert-rules-via-azure-powershell"></a>Konfigurace pravidla výstrah e-mailu pomocí prostředí Azure PowerShell

1. Získání seznamu odběry, které máte k dispozici. Ověřte, že pracujete s správné předplatné. Pokud ne, nastavte ji na ten správný pomocí výstup Get-AzureRmSubscription. 

    ```powershell
    Get-AzureRmSubscription 
    Get-AzureRmContext 
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

2. K zobrazení seznamu existujících pravidel ve skupině prostředků, použijte následující příkaz: 

    ```powershell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

3. K vytvoření pravidla, musíte nejprve mít několik důležitých informací. 

    - ID prostředku pro prostředek, kterou chcete nastavit výstrahy pro.
    - Metriky definice je k dispozici pro tento prostředek. Jeden způsob, jak získat ID prostředku je použití portálu Azure. Za předpokladu, že byl již vytvořený, vyberte ho v portálu Azure. Na další stránce vyberte *vlastnosti* pod *nastavení* části. **ID prostředku** je pole na další stránce. Dalším způsobem je použít [Průzkumníka prostředků Azure](https://resources.azure.com/). Je například ID prostředku pro veřejnou IP adresu:`/subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Následující příklad nastaví výstrahu na veřejné IP adresy přidružené k virtuální síti. Metrika na vytvoření výstrahy **útoku DDoS pod nebo není**. Toto je logická hodnota, 1 nebo 0. A **1** znamená, můžete v útoku. A **0** znamená nejsou napadené. Výstraha je vytvořen po napadené během posledních 5 minut.

    K vytvoření webhook, jehož nebo odeslat e-mail, když se vytvoří výstrahu, je nutné nejprve vytvořte e-mailu nebo webhooky. Potom hned vytvořte pravidlo později se značkou - akce a jak je znázorněno v následujícím příkladu. Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí prostředí PowerShell.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail-Description "Under DDoS Attack" 
    ```

4. Chcete-li ověřit, že upozornění nebyly vytvořeny správně pohledem na jednotlivá pravidla.

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

Můžete si také přečíst informace o [konfigurace webhooky](../monitoring-and-diagnostics/insights-webhooks-alerts.md) a [aplikace logiky](../logic-apps/logic-apps-what-are-logic-apps.md) pro vytvoření výstrahy.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfigurovat protokolování na metriky ochrana proti útoku DDoS

Odkazovat [prostředí PowerShell rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md) vám pomohou přístup a konfiguraci služby Azure pomocí prostředí PowerShell protokolování diagnostiky.

## <a name="next-steps"></a>Další kroky

- [Další informace o diagnostických protokolů Azure.](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analýza protokolů z úložiště Azure s analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
- [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)