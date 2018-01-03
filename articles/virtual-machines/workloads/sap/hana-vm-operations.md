---
title: SAP HANA operace v Azure | Microsoft Docs
description: "Provozní příručka pro SAP HANA systémy, které jsou nasazeny na virtuálních počítačích Azure."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d6991d40b9bb8543898bbbdc9d7c905dfe11536
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA na Azure provozní příručky
Tento dokument obsahuje pokyny pro operační systémy SAP HANA, které jsou nasazeny na Azure nativní virtuální počítače (VM). Tento dokument není určen k nahrazení standardní SAP dokumentace, která zahrnuje následující obsah:

- [Příručka pro správu SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Průvodce instalací SAP](https://service.sap.com/instguides)
- [Poznámky k SAP](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Požadavky
Chcete-li tohoto průvodce použijte, je třeba základní znalosti o Azure následující součásti:

- [Virtuální počítače Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure sítě a virtuální sítě](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Další informace o SAP NetWeaver a další součásti SAP v Azure, najdete v článku [SAP v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) části [dokumentace k Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Důležité informace o základní nastavení
Následující části popisují základní nastavení aspekty pro nasazení SAP HANA systémy na virtuálních počítačích Azure.

### <a name="connect-into-azure-virtual-machines"></a>Připojení do virtuálních počítačů Azure
Jak je uvedeno v [virtuální počítače Azure Průvodce plánováním](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), existují dvě základní metody pro připojení do virtuálních počítačích Azure:

- Připojení přes internet a veřejné koncové body na virtuálním počítači přejít, nebo na virtuální počítač se systémem SAP HANA.
- Připojení přes [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) nebo Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Připojení Site-to-site prostřednictvím sítě VPN nebo ExpressRoute je potřebný u produkčních scénářích. Tento typ připojení je také potřeba pro nevýrobní prostředí scénáře, které informačního kanálu do produkčních scénářích, kde se používá SAP softwaru. Následující obrázek ukazuje příklad připojení webů:

![Připojení webů](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Vyberte typy virtuálních počítačů Azure
Virtuální počítač Azure typy, které lze použít pro produkčních scénářích jsou uvedeny v [SAP dokumentaci IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Pro scénáře nevýrobní prostředí je k dispozici širší rozsah nativní typy virtuálního počítače Azure.

>[!NOTE]
>Mimo produkční scénářů použití typy virtuálních počítačů, které jsou uvedeny v [Poznámka SAP #1928533](https://launchpad.support.sap.com/#/notes/1928533).

Nasazení virtuálních počítačů v Azure pomocí:

- Portál Azure.
- Rutiny Azure PowerShell.
- Azure CLI.

Také můžete nasadit kompletní nainstalované SAP HANA platformy v rámci služeb virtuálního počítače Azure pomocí [SAP Cloudová platforma](https://cal.sap.com/). Proces instalace je popsán v [nasazení SAP S nebo 4HANA nebo BW/4HANA v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choose-azure-storage-type"></a>Zvolte typ úložiště Azure
Azure nabízí dva typy úložiště, které jsou vhodné pro virtuální počítače Azure, který běží SAP HANA:

- [Úložiště Azure úrovně Standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure nabízí dvě metody nasazení pro virtuální pevné disky na Azure Standard a Premium Storage. Pokud to umožňují celkové scénář, využít výhod [Azure spravované disku](https://azure.microsoft.com/services/managed-disks/) nasazení.

Seznam typů úložišť a jejich SLA, zkontrolujte [dokumentace k Azure pro spravované disky](https://azure.microsoft.com/pricing/details/managed-disks/).

Azure prémiové disky se doporučuje pro /hana/data a /hana/log svazky. Můžete sestavit LVM RAID přes víc disků úložiště Premium a použijte svazky RAID jako /hana/data a /hana/log svazky.

Následující tabulka uvádí konfigurací typy virtuálních počítačů, které zákazníci běžně používají k hostiteli SAP HANA na virtuálních počítačích Azure:

| VIRTUÁLNÍ POČÍTAČ SKU | Paměť RAM | Max. VIRTUÁLNÍ POČÍTAČ VSTUPNĚ-VÝSTUPNÍCH OPERACÍ<br /> Propustnost | / hana/protokolu a/hana/data<br /> rozdělená s LVM nebo MDADM | / hana/sdílené | / root svazku | / usr/sap | Hana/zálohování |
| --- | --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GiB | 384 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 giB | 768 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 giB | 1 200 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 giB | 2 000 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1000 giB | 1000 GB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1 750 giB | 1000 GB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2 000 GB |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 giB | 2 000 GB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


> [!NOTE]
> Disky doporučeno pro typy menší virtuální počítač s 3 x P20 oversize svazky týkající se podle doporučení místa [SAP TDI úložiště dokument White Paper](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Ale volba, zobrazovat v tabulce byl proveden zajistit dostatek propustnost disku pro SAP HANA. Pokud budete potřebovat méně propustnost vstupu/výstupu, můžete upravit volba disky úložiště Premium pro /hana/data a /hana/log. Totéž platí pro nastavení velikosti /hana/backup svazku, který byl dimenzované pro zálohování, které představují dvakrát svazku paměti. Pokud budete potřebovat méně místa, potom můžete upravit. Mějte celkovou propustnost vstupně-výstupních operací na virtuální počítač při změně velikosti nebo rozhodování pro virtuální počítač. Celkové propustnost virtuálního počítače je popsána v článku [paměťově optimalizované velikosti virtuálních počítačů](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)  

> [!NOTE]
> Pokud chcete využívat [jeden virtuální počítač Azure SLA k Virtuálním počítačům](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) budete muset změnit všechny virtuální pevné disky, které jsou uvedeny jako standardní úložiště (Sxx) do úložiště úrovně Premium (Pxx). 


### <a name="set-up-azure-virtual-networks"></a>Nastavení virtuální sítě Azure
Pokud máte připojení site-to-site do Azure přes VPN nebo ExpressRoute, musíte mít alespoň jeden [virtuální síť Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) který je připojený prostřednictvím brány virtuální sítě VPN nebo ExpressRoute s okruhem. Brána virtuální žije v podsíť ve virtuální síti Azure. Chcete-li nainstalovat SAP HANA, vytvořte dva další podsítě v rámci virtuální sítě. Jednu podsíť hostuje virtuální počítače ke spuštění instance SAP HANA. Další podsítě spustí Jumpbox nebo správu virtuálních počítačů pro hostování SAP HANA Studio nebo jiného softwaru pro správu.

Když instalujete virtuální počítače spustit SAP HANA, třeba virtuální počítače:

- Dva virtuální síťové karty nainstalované: jednu síťovou kartu pro připojení k podsíti správy a jeden síťový adaptér a z místní sítě nebo jiné sítě, se připojit k instanci SAP HANA ve virtuálním počítači Azure.
- Statickou privátní IP adresy, které jsou nasazeny pro obě virtuálních síťových karet.

Přehled různých metod pro přiřazení IP adres najdete v tématu [IP adres, typy a metody přidělování v Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

[Azure skupiny zabezpečení sítě (Nsg)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) jsou používána ke směrování provozu, který se směruje na instanci SAP HANA nebo Jumpbox. Jsou přidružené k SAP HANA podsíť a podsíť správy skupin Nsg.

Následující obrázek ukazuje přehled hrubý nasazení schématu pro SAP HANA:

![Hrubý nasazení schématu pro SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


K nasazení SAP HANA v Azure bez připojení site-to-site, přístup k SAP HANA instance, když veřejnou IP adresu. Virtuální počítač Azure, který běží Jumpbox virtuálního počítače musí být přidělit IP adresu. V tomto scénáři základní nasazení spoléhá na Azure vestavěné služby DNS přeložit názvy hostitelů. V složitější nasazení, kde se používají veřejné IP adresy jsou zvlášť důležité, Azure vestavěné služby DNS. Pomocí skupin Nsg Azure můžete omezit otevřené porty nebo rozsahy IP adres, které se můžou připojit do Azure podsítí s prostředky, které mají veřejné IP adresy. Následující obrázek znázorňuje hrubé schématu pro nasazení SAP HANA bez připojení site-to-site:
  
![Hrubý nasazení schématu pro SAP HANA bez připojení site-to-site](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Operace pro nasazení SAP HANA na virtuálních počítačích Azure
Následující části popisují některé operace související s nasazováním systémů SAP HANA na virtuálních počítačích Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Zálohování a obnovení operací na virtuálních počítačích Azure
Tyto dokumenty popisují, jak zálohovat a obnovovat nasazení SAP HANA:

- [Přehled zálohování SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Zálohování na úrovni souborů SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Srovnávací test snímku úložiště SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Spuštění a restartování virtuálních počítačů, které obsahují SAP HANA
Hlavní funkce veřejného cloudu Azure je, že se vám účtovat pouze pro vaše výpočetní minut. Například při vypnutí virtuálního počítače se systémem SAP HANA se fakturuje pouze náklady na úložiště během této doby. Další funkce je dostupná, když zadáte statické IP adresy pro virtuální počítače v původním nasazení. Po restartování virtuálního počítače, který má SAP HANA restartuje virtuální počítač s jeho předchozí IP adresy. 


### <a name="use-saprouter-for-sap-remote-support"></a>Použijte SAProuter podporu vzdálené SAP
Pokud máte připojení site-to-site mezi vaší místní umístění a Azure, a pak pravděpodobně již používáte SAProuter používáte SAP součásti. V takovém případě dokončete následující položky pro vzdálené podpory:

- Udržujte privátní a statickou IP adresu virtuálního počítače, který je hostitelem SAP HANA v konfiguraci SAProuter.
- Nakonfigurujte podsítě, který je hostitelem virtuálního počítače HANA umožňující přenos prostřednictvím portu TCP/IP 3299 NSG.

Pokud se připojujete k Azure přes internet a nemáte SAP směrovače pro virtuální počítač s SAP HANA, budete muset nainstalovat součást. Nainstalujte SAProuter v samostatných virtuálních počítačů v podsíti správy. Následující obrázek znázorňuje hrubé schématu pro nasazení SAP HANA bez připojení site-to-site a s SAProuter:

![Bez připojení site-to-site a SAProuter extenzivních nasazení schématu pro SAP HANA](media/hana-vm-operations/hana-simple-networking3.PNG)

Ujistěte se, že nainstalujete SAProuter v samostatných virtuálních počítačů a není v Jumpbox virtuálního počítače. Samostatný virtuální počítač musí mít statickou IP adresu. Pokud chcete připojit vaše SAProuter k SAProuter, který je hostitelem SAP, obraťte se na SAP pro IP adresu. (SAProuter, který je hostitelem SAP je protějškem SAProuter instance, kterou si nainstalujete do virtuálního počítače.) Slouží ke konfiguraci vaší instance SAProuter IP adresu z SAP. V nastavení konfigurace a jenom nezbytné port je TCP port 3299.

Další informace o tom, jak nastavit a spravovat podporu vzdáleného připojení prostřednictvím SAProuter najdete v tématu [SAP dokumentaci](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Vysoká dostupnost s SAP HANA na virtuálních počítačích Azure nativní
Pokud používáte systém SUSE Linux 12 SP1 nebo novější, můžete vytvořit cluster kardiostimulátor s STONITH zařízení. Zařízení můžete použít k nastavení konfigurace SAP HANA, který používá synchronní replikaci s HANA systému replikace a automatické převzetí služeb při selhání. Další informace o postupu instalace najdete v tématu [vysokou dostupnost SAP HANA na virtuálních počítačích Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).
