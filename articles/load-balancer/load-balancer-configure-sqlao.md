---
title: "Konfigurace pro vyrovnávání zatížení pro SQL vždy na | Microsoft Docs"
description: "Konfigurace pro vyrovnávání zatížení tak pracovat s SQL vždy na a jak využít prostředí powershell vytvořit nástroj pro vyrovnávání zatížení pro provedení SQL"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3ebbf1c4009d89b1f18b2ff8ff5dd243c456dff8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurace pro vyrovnávání zatížení pro SQL vždy na

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Skupiny dostupnosti AlwaysOn serveru SQL je teď možné spustit s ILB. Skupina dostupnosti je řešení nejdůležitějšího SQL serveru pro vysokou dostupnost a zotavení po havárii. Naslouchací proces skupiny dostupnosti, umožňuje klientským aplikacím bezproblémově připojit k primární replice, bez ohledu na počet replik v konfiguraci.

Název naslouchacího procesu (DNS) je namapovaný na IP adresu s vyrovnáváním zatížení a nástroj pro vyrovnávání zatížení Azure směruje příchozí provoz jenom na primární server sady replik.

Podpora ILB můžete použít pro koncové body SQL Server AlwaysOn (naslouchací proces). Nyní máte kontrolu nad usnadnění naslouchacího procesu a vyberte IP adresu Vyrovnávání zatížení sítě z konkrétní podsítě ve virtuální síti (VNet).

Pomocí ILB na naslouchací proces, koncový bod SQL server (například Server = tcp:ListenerName, 1433; Database = DatabaseName) je přístupná jenom pro:

* Služby a virtuální počítače ve stejné virtuální síti
* Služeb a virtuálních počítačů z připojených do místní sítě
* Služeb a virtuálních počítačů z vzájemně propojena virtuální sítě

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Obrázek 1 – nakonfigurovaná pomocí nástroje pro vyrovnávání zatížení internetového technologie AlwaysOn serveru SQL

## <a name="add-internal-load-balancer-to-the-service"></a>Přidejte interní nástroj pro vyrovnávání zatížení do služby

1. V následujícím příkladu jsme nakonfigurovat virtuální síť, která obsahuje podsíť s názvem "Subnet-1.:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Přidat skupinu s vyrovnáváním zatížení koncové body pro ILB na jednotlivé virtuální počítače

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    V příkladu nahoře, máte názvem "sqlsvc1" a "sqlsvc2" spuštěna 2 Virtuálního počítače v cloudu služby "Sqlsvc". Po vytvoření ILB s `DirectServerReturn` přepínače, můžete přidat skupinu s vyrovnáváním zatížení koncové body k ILB umožňující nakonfigurovat naslouchací procesy pro skupiny dostupnosti SQL.

Další informace o SQL AlwaysOn najdete v tématu [konfigurace interní nástroj pro skupinu dostupnosti AlwaysOn v Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Viz také
[Začít konfigurovat internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Začněte konfiguraci Vyrovnávání zatížení interní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
