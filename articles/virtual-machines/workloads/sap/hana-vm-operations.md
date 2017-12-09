---
title: SAP HANA operace v Azure | Microsoft Docs
description: "Operace na virtuálních počítačích Azure nativní SAP HANA"
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
ms.openlocfilehash: ab609fe9e7b01d7087dd00c22c19e69a471f6599
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA na Azure provozní příručky
Tato příručka obsahuje pokyny pro operační systémy SAP HANA, které jsou nasazené na virtuálních počítačích Azure. Tento dokument není určen k nahrazení všechny standardní dokumentace SAP. SAP příručky a poznámky naleznete v následujících umístěních:

- [Příručka pro správu SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Průvodce instalací SAP](https://service.sap.com/instguides)
- [Poznámka SAP](https://sservice.sap.com/notes)

Předpokladem je, že máte základní znalosti na různé komponenty Azure:

- [Virtuální počítače Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure sítě a virtuální sítě](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Další dokumentaci na SAP NetWeaver a další součásti SAP v Azure najdete v [SAP v Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) části [dokumentace k Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Důležité informace o základní nastavení
### <a name="connecting-into-azure"></a>Připojení do Azure
Jak je uvedeno v [virtuálních počítačích Azure plánování a implementace pro SAP NetWeaver] [-Příručka plánování], existují dvě základní metody pro připojení virtuálních počítačů do Azure. 

- Připojení prostřednictvím Internetu a veřejné koncové body na Přejít virtuálního počítače nebo virtuálního počítače s SAP HANA
- Připojení prostřednictvím [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) nebo Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

Pro produkční prostředí scénáře nebo scénářů, kde mimo produkční scénáře, které informačního kanálu do produkčních scénářích ve spojení s SAP softwaru, musíte mít připojení site-to-site prostřednictvím sítě VPN nebo ExpressRoute, jak je znázorněno na obrázku:

![Připojení webů](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choice-of-azure-vm-types"></a>Výběr typů virtuálních počítačů Azure
Azure typy virtuálních počítačů, které lze použít pro produkčních scénářích lze vyhledávat [zde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Pro scénáře nevýrobní prostředí můžete širší rozsah nativní virtuálních počítačích Azure. Měli byste ale omezit sami na typy virtuálních počítačů, které jsou [&#1928533; Poznámka SAP](https://launchpad.support.sap.com/#/notes/1928533). Nasazení těchto virtuálních počítačů v Azure se dá udělat pomocí:

- portál Azure
- Rutiny Azure Powershell
- Azure CLI

Také můžete nasadit kompletní nainstalované platformy SAP HANA do služby Azure virtuální počítače prostřednictvím [SAP Cloudová platforma](https://cal.sap.com/) podle postupu uvedeného [zde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choice-of-azure-storage"></a>Výběr úložiště Azure
Azure nabízí dva typy hlavní úložiště vhodný pro virtuální počítače Azure s SAP HANA

- [Úložiště Azure úrovně Standard](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure nabízí dvě metody nasazení pro virtuální pevné disky na Azure Standard a Premium Storage. Pokud celkový scénář umožňuje, doporučujeme využít [disku spravované Azure](https://azure.microsoft.com/services/managed-disks/) nasazení.

Pro přesné úložiště typy a SLA kolem tyto typy úložišť, zkontrolujte [této dokumentace](https://azure.microsoft.com/pricing/details/managed-disks/)

Doporučuje se používat Azure prémiové disky pro /hana/data a /hana/log svazky. Podporuje se sestavení LVM RAID přes víc disků úložiště Premium a použijte tyto svazky RAID jako /hana/data a /hana/log svazky.

Možné konfigurace pro různé běžné typy virtuálních počítačů, které zákazníci používají, pokud pro hostování SAP HANA na virtuálních počítačích Azure může vypadat podobně jako:

| VIRTUÁLNÍ POČÍTAČ SKU | Paměť RAM | / hana/protokolu a/hana/data<br /> rozdělená s LVM nebo MDADM | / hana/sdílené | / root svazku | / usr/sap | Hana/zálohování |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3.8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="azure-networking"></a>Síť Azure
Za předpokladu, že máte připojení site-to-site VPN nebo ExpressRoute do Azure, je minimálně byste mít [virtuální síť Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) který je připojený prostřednictvím brány virtuální sítě VPN nebo ExpressRoute s okruhem. Brána virtuální žije v podsíť ve virtuální síti Azure. Abyste mohli nainstalovat HANA, byste měli vytvořit další dvě podsítě v rámci virtuální sítě. Jednu podsíť hostitelů virtuální počítače, který spustí instance SAP HANA a jinou podsítí, který spouští případné Jumpbox nebo správu virtuální počítače, který může být hostitelem SAP HANA Studio nebo jiného softwaru pro správu.
Když instalujete virtuální počítače, které se má spustit HANA, musí mít virtuální počítače:

- Dva virtuální síťové karty nainstalované kterých jeden připojí k podsíti správy a jeden síťový adaptér slouží k připojení buď na místní nebo jiné sítě z instance SAP HANA ve virtuálním počítači Azure.
- Statickou privátní IP adresy nasazení pro obě vNICs

Přehled o různých možnostech, které přiřazení IP adresy najdete [zde](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Provoz směrování do přímo na instance SAP HANA nebo na jumpbox přesměrován podle [skupin zabezpečení sítě Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) , a jsou přidruženy ke HANA podsíť a podsíť správy.

Celkové schéma hrubý nasazení by vypadat podobně jako:

![Hrubý nasazení schématu pro SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Pokud nasadíte právě SAP HANA v Azure bez nutnosti site-to-site (VPN nebo ExpressRoute do Azure), by získat přístup k instanci SAP HANA, i když veřejnou IP adresu, kterému je přiřazen Azure virtuální počítač, který spouští Jumpbox virtuálního počítače. V případě jednoduchého také spoléháte na Azure vestavěné služby DNS za účelem vyřešení názvy hostitelů. Zejména při použití veřejných IP adres, kterou chcete použít skupiny zabezpečení sítě Azure k omezení otevřené porty nebo rozsahy IP adres, který se mohou připojit do Azure podsítí s prostředky veřejné IP adresy. Schéma takovémto nasazení může vypadat podobně jako:
  
![Hrubý nasazení schématu pro SAP HANA bez připojení site-to-site](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations"></a>Operace
### <a name="backup-and-restore-operations-on-azure-vms"></a>Operace zálohování a obnovení na virtuálních počítačích Azure
Možnosti SAP HANA zálohování a obnovení jsou popsány v těchto dokumentů:

- [Přehled funkce zálohování SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Zálohování SAP HANA na úrovni souborů](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Srovnávací test SAP HANA úložiště snímků](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)



### <a name="start-and-restart-of-vms-containing-sap-hana"></a>Počáteční a restartování virtuálních počítačů obsahující SAP HANA
Jedním z síly veřejného cloudu Azure je skutečnost, že se účtují poplatky za výpočetní minut, které jsou výdaje. To znamená, že pokud vypnout virtuální počítač s SAP HANA spuštěna v něm, během této doby se účtují pouze náklady na úložiště. Spusťte virtuální počítač s SAP HANA v ní znovu, virtuální počítač bude znovu spustit a bude mít stejné IP adresy (Pokud jste nasadili pomocí statické IP adresy). 


### <a name="saprouter-enabling-sap-remote-support"></a>SAPRouter povolení SAP vzdáleného podpory
Pokud máte připojení site-to-site mezi vaší místní umístění a Azure a spustíte SAP součásti již, je vysoce pravděpodobné, že už spouštíte SAProuter již. V takovém případě není nic, co je potřeba instancemi SAP HANA, které můžete nasadit v Azure. Kromě udržovat privátní a statickou IP adresu virtuálního počítače, který je hostitelem HANA v konfiguraci SAPRouter a mít NSG hostování podsíť virtuálních počítačů HANA přizpůsobit (provoz přes port TCP/IP port 3299 povoleny).

Pokud provádíte nasazení SAP HANA a připojíte se k Azure přes Internet a nemáte směrovač SAP nainstalován ve virtuální síti, na kterém běží virtuální počítač s SAP HANA, byste měli instalovat SAPRouter v samostatných virtuálních počítačů v podsíti správu, jak je vidět tady :


![Hrubý nasazení schématu pro SAP HANA bez připojení site-to-site a SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

Musíte nainstalovat SAPRouter v samostatných virtuálních počítačů a není Jumpbox virtuálního počítače. Samostatný virtuální počítač potřebuje statickou IP adresu. Aby bylo možné připojit SAPRouter k SAPRouter, který je hostitelem SAP (protějšku instance SAPRouter instalaci virtuálního počítače), budete muset kontaktovat SAP k získání IP adresy ze SAP, který je nutné nakonfigurovat instanci SAPRouter. Nezbytné pouze port je TCP port 3299.
Další informace o tom, jak nastavit a spravovat podporu vzdáleného připojení prostřednictvím SAPRouter, zaškrtněte toto políčko [SAP zdroj](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Vysoká dostupnost s SAP HANA na virtuálních počítačích Azure nativní
Systém SUSE Linux 12 SP1 a novější můžete vytvořit cluster kardiostimulátor s STONITH zařízení k nastavení konfigurace SAP HANA, který používá synchronní replikaci s HANA systému replikace a automatické převzetí služeb při selhání. Postup instalace je popsána v článku [vysokou dostupnost z SAP HANA ve virtuálních počítačích Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

 










