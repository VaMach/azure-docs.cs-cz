---
title: "Konfigurace portů pro vysokou dostupnost pro vyrovnávání zatížení Azure | Microsoft Docs"
description: "Naučte se používat porty vysokou dostupnost pro interní přenosů na všech portech služby Vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2017
ms.author: kumud
ms.openlocfilehash: 646ade828e96810bdc3b07d4dc5c0276a1621969
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>Postup konfigurace portů pro vysokou dostupnost pro interní nástroj pro vyrovnávání zatížení

Tento článek poskytuje příklad nasazení vysoké dostupnosti (HA) porty na interní pro vyrovnávání zatížení. Konkrétní konfigurací virtuálních síťových zařízení (NVAs) najdete v části odpovídající weby zprostředkovatele.

>[!NOTE]
> Vysoká dostupnost porty funkce je aktuálně ve verzi Preview. Během období Preview tato funkce nemusí dosahovat stejné úrovně dostupnosti a spolehlivosti jako funkce, které jsou ve verzi všeobecné dostupnosti. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Obrázek 1 ukazuje příklad nasazení, které jsou popsané v tomto článku následující konfiguraci:
- NVAs nasazených ve fondu back-end interní služby Vyrovnávání zatížení za konfiguraci portů HA. 
- UDR použít v podsíti DMZ trasy veškerý provoz NVAs tím, že další směrování jako interní virtuální IP nástroje pro vyrovnávání zatížení. 
- Interní nástroj pro vyrovnávání zatížení distribuuje provoz do jednoho z active NVAs podle algoritmus LB.
- Hodnocení chyb zabezpečení zpracovává provoz a předává do původního cíle v podsíti back-end.
- Návratový cestu můžete taky využít stejné trasy, pokud odpovídající UDR je nakonfigurovaný v podsíti back-end. 

![ha porty příklad nasazení](./media/load-balancer-configure-ha-ports/haports.png)

Obrázek 1 – síťových virtuálních zařízení nasazený za interní Vyrovnávání zatížení s vysokou dostupností porty 

## <a name="preview-sign-up"></a>Náhled registrace

K účasti ve verzi Preview funkci HA porty ve standardní nástroje pro vyrovnávání zatížení, zaregistrujte předplatné pro získání přístupu pomocí Azure CLI 2.0 nebo prostředí PowerShell. Registrace předplatného pro [standardní nástroje pro vyrovnávání zatížení preview](https://aka.ms/lbpreview#preview-sign-up).

>[!NOTE]
>Registrace standardní nástroje pro vyrovnávání zatížení verze Preview může trvat až jednu hodinu.

## <a name="configuring-ha-ports"></a>Konfigurace portů HA

Konfigurace portů HA zahrnuje nastavení k interní nástroj pro vyrovnávání zatížení, s NVAs ve fondu back-end odpovídající konfiguraci služby Vyrovnávání zatížení stavu test ke zjištění stavu hodnocení chyb zabezpečení a pravidlo Vyrovnávání zatížení s vysokou DOSTUPNOSTÍ porty. Obecná konfigurace související nástroje pro vyrovnávání zatížení je popsaná v [Začínáme](load-balancer-get-started-ilb-arm-portal.md). V tomto článku klade důraz na konfiguraci portů HA.

Konfigurace v podstatě zahrnuje nastavení hodnoty front-end port a back-end port **0**a pro protokol hodnotu **všechny**. Tento článek popisuje postup konfigurace vysoké dostupnosti portů pomocí portálu Azure, PowerShell a Azure CLI 2.0.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Konfigurovat porty HA se pravidlo Vyrovnávání zatížení pomocí portálu Azure

Portál Azure obsahuje **HA porty** možnost prostřednictvím zaškrtávací políčko pro tuto konfiguraci. Pokud vybraná, se automaticky vyplní odpovídající konfigurační protokol a port. 

![ha porty konfigurace prostřednictvím portálu Azure](./media/load-balancer-configure-ha-ports/haports-portal.png)

Obrázek 2 – HA porty konfigurace prostřednictvím portálu

### <a name="configure-ha-ports-lb-rule-via-resource-manager-template"></a>Konfigurace pravidla LB porty HA pomocí šablony Resource Manageru

Můžete nakonfigurovat porty HA pomocí 2017-08-01 verze rozhraní API pro Microsoft.Network/loadBalancers v prostředku nástroj pro vyrovnávání zatížení. Následující fragment kódu JSON znázorňuje změny v konfiguraci Vyrovnávání zatížení pro HA porty přes REST API.

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>Konfigurovat porty HA se pravidlo Vyrovnávání zatížení v prostředí PowerShell

Vytvoření pravidla HA porty pro vyrovnávání zatížení při vytváření interní nástroj pro vyrovnávání zatížení v prostředí PowerShell, použijte následující příkaz:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Pravidlo Vyrovnávání zatížení HA porty nakonfigurovat 2.0 rozhraní příkazového řádku Azure

V kroku č. 4 z [vytvoření interní zatížení vyrovnávání skupiny](load-balancer-get-started-ilb-arm-cli.md), použijte následující příkaz pro vytvoření porty HA pravidla pro vyrovnávání zatížení.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Další kroky

- Další informace o [porty vysokou dostupnost](load-balancer-ha-ports-overview.md)
