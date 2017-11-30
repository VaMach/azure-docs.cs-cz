---
title: "Azure Site Recovery sítě pokyny pro replikaci virtuálních počítačů z Azure do Azure | Microsoft Docs"
description: "Sítě pokyny pro replikaci virtuálních počítačů Azure"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: a2ea66f2ed3815d0375001571e64dad338f16e3e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Sítě pokyny pro replikaci virtuálních počítačů Azure

>[!NOTE]
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.

Tento článek podrobnosti sítě pokyny pro Azure Site Recovery, kdy jste replikaci a obnovení virtuálních počítačů Azure z jedné oblasti do jiné oblasti. Další informace o požadavcích na Azure Site Recovery najdete v tématu [požadavky](site-recovery-support-matrix-azure-to-azure.md) článku.

## <a name="site-recovery-architecture"></a>Architektura obnovení lokality

Site Recovery poskytuje jednoduché a snadný způsob, jak replikovat aplikací běžících na virtuálních počítačích Azure jiné oblasti Azure, aby se můžete obnovit, pokud dojde přerušení v primární oblasti. Další informace o [tento scénář a architektuře Site Recovery](concepts-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Infrastruktura vaší sítě

Následující diagram znázorňuje typické prostředí Azure pro aplikace běžící na virtuálních počítačích Azure:

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Pokud používáte Azure ExpressRoute nebo VPN připojení z místní sítě do Azure, prostředí vypadá takto:

![prostředí zákazníka](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Obvykle se zákazníci chránit jejich sítím pomocí brány firewall nebo skupiny zabezpečení sítě (Nsg). Bránu firewall můžete použít buď na základě adresy URL nebo na základě IP povolených pro řízení připojení k síti. Skupiny Nsg povolí pravidla pro používání rozsahy IP adres k řízení připojení k síti.

>[!IMPORTANT]
> Pokud používáte ověřený server proxy pro řízení připojení k síti, se nepodporuje a Site Recovery replikaci nejde povolit.

Následující části popisují změny odchozí připojení k síti, které jsou požadovány z Azure Site Recovery replikace pro práci u virtuálních počítačů.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Odchozí připojení pro adresy URL Azure Site Recovery

Pokud používáte žádné brány firewall založená na adresu URL proxy serveru k řízení odchozí připojení, nezapomeňte seznamu povolených IP adres, že následující požadované adresy URL služby Azure Site Recovery:


**ADRESA URL** | **Účel**  
--- | ---
*.blob.core.windows.net | Vyžaduje, aby data je možné zapsat do mezipaměti účet úložiště v oblasti zdroje z virtuálního počítače.
Login.microsoftonline.com | Vyžaduje se pro autorizaci a ověřování na adresy URL služby Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Vyžaduje, aby komunikace služby Site Recovery může dojít z virtuálního počítače.
*. servicebus.windows.net | Vyžaduje, aby data monitorování a Diagnostika Site Recovery je možné zapsat z virtuálního počítače.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Odchozí připojení pro rozsahy Azure Site Recovery IP

>[!NOTE]
> Chcete-li automaticky vytvářet požadovaná pravidla NSG na skupinu zabezpečení sítě, můžete [stáhnout a použít tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Doporučujeme vytvořit požadované pravidla NSG na testovací skupiny zabezpečení sítě a ověřte, že před vytvořením pravidel na produkční skupina zabezpečení sítě nejsou žádné problémy.
> * Vytvořit požadovaný počet pravidel NSG, ujistěte se, že vaše předplatné je seznam povolených adres. Požádejte podporu o zvýšení limitu pravidla NSG v rámci vašeho předplatného.

Pokud používáte všechny proxy serveru na základě IP brány firewall nebo pravidla NSG k řízení odchozí připojení, musí být seznam povolených adres, v závislosti na zdrojové a cílové umístění virtuálních počítačů následující rozsahy IP:

- Všechny rozsahy IP, které odpovídají umístění zdroje. (Můžete stáhnout [rozsahy IP adres](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vytvoření seznamu povolených je potřeba, je možné zapsat data do účtu úložiště mezipaměti z virtuálního počítače.

- Všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Pokud nové IP adres se přidají do rozsahů Office 365 IP v budoucnu, musíte vytvořit nová pravidla NSG.

- Lokality koncový bod obnovení služby IP adresy ([k dispozici v souboru XML](https://aka.ms/site-recovery-public-ips)), které závisí na cílové umístění:

   **Cílové umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
   --- | --- | ---
   Východní Asie | 52.175.17.132 | 13.94.47.61
   Jihovýchodní Asie | 52.187.58.193 | 13.76.179.223
   Střed Indie | 52.172.187.37 | 104.211.98.185
   Indie – jih | 52.172.46.220 | 104.211.224.190
   Střed USA – sever | 23.96.195.247 | 168.62.249.226
   Severní Evropa | 40.69.212.238 | 52.169.18.8
   Západní Evropa | 52.166.13.64 | 40.68.93.145
   Východ USA | 13.82.88.226 | 104.45.147.24
   Západní USA | 40.83.179.48 | 104.40.26.199
   Střed USA – jih | 13.84.148.14 | 104.210.146.250
   Střed USA | 40.69.144.231 | 52.165.34.144
   Východní USA 2 | 52.184.158.163 | 40.79.44.59
   Japonsko – východ | 52.185.150.140 | 138.91.1.105
   Japonsko – západ | 52.175.146.69 | 138.91.17.38
   Brazílie – jih | 191.234.185.172 | 23.97.97.36
   Austrálie – východ | 104.210.113.114 | 191.239.64.144
   Austrálie – jihovýchod | 13.70.159.158 | 191.239.160.45
   Střední Kanada | 52.228.36.192 | 40.85.226.62
   Východní Kanada | 52.229.125.98 | 40.86.225.142
   Západní střed USA | 52.161.20.168 | 13.78.149.209
   Západní USA 2 | 52.183.45.166 | 13.66.228.204
   Spojené království – západ | 51.141.3.203 | 51.141.14.113
   Spojené království – jih | 51.140.43.158 | 51.140.189.52
   Spojené království – jih 2 | 13.87.37.4| 13.87.34.139
   Spojené království – sever | 51.142.209.167 | 13.87.102.68
   Korea – střed | 52.231.28.253 | 52.231.32.85
   Korea – jih | 52.231.298.185 | 52.231.200.144

## <a name="sample-nsg-configuration"></a>Ukázková konfigurace NSG
Tato část vysvětluje postup konfigurace pravidla NSG, aby mohl pracovat replikace Site Recovery na virtuálním počítači. Pokud používáte pravidla NSG k řízení odchozí připojení, použijte "Povolit HTTPS odchozí" pravidla pro všechny požadované rozsahy IP.

>[!Note]
> Chcete-li automaticky vytvářet požadovaná pravidla NSG na skupinu zabezpečení sítě, můžete [stáhnout a použít tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Například pokud zdrojové umístění Virtuálního počítače je "Východ USA" a cílové umístění vaší replikace je "Střední USA", postupujte podle kroků v následujících dvou částech.

>[!IMPORTANT]
> * Doporučujeme vytvořit požadované pravidla NSG na testovací skupiny zabezpečení sítě a ověřte, že před vytvořením pravidel na produkční skupina zabezpečení sítě nejsou žádné problémy.
> * Vytvořit požadovaný počet pravidel NSG, ujistěte se, že vaše předplatné je seznam povolených adres. Požádejte podporu o zvýšení limitu pravidla NSG v rámci vašeho předplatného.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>Pravidla NSG na skupinu zabezpečení sítě východní USA

* Vytvoření pravidla, které odpovídají [rozsahy IP USA – východ](https://www.microsoft.com/download/confirmation.aspx?id=41653). To je potřeba, aby data je možné zapsat do mezipaměti účet úložiště z virtuálního počítače.

* Vytvoření pravidel pro všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Vytvoření pravidla, která odpovídají do cílového umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Střed USA | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>Pravidla NSG na skupinu zabezpečení sítě střed USA

Tato pravidla jsou vyžadována, takže je možné povolit replikaci z oblasti target zdrojovém oblast post-převzetí služeb při selhání:

* Pravidla, které odpovídají [rozsahy IP centrální USA](https://www.microsoft.com/download/confirmation.aspx?id=41653). Toto jsou požadované, tak, aby data je možné zapsat do mezipaměti účet úložiště z virtuálního počítače.

* Pravidla pro všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Pravidla, které odpovídají umístění zdroje:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Východ USA | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Pokyny pro existující konfiguraci Azure na místní ExpressRoute nebo VPN

Pokud máte ExpressRoute nebo VPN připojení mezi místními a zdrojovým umístěním v Azure, postupujte podle pokynů v této části.

### <a name="forced-tunneling-configuration"></a>Vynutit tunelové konfigurace

Obvyklé konfigurace zákazníka je definovat výchozí trasa (0.0.0.0/0), který vynutí odchozí přenosy z Internetu do procházet skrz místní umístění. To nedoporučujeme. Provoz replikace a komunikace služby Site Recovery neměli ponechat Azure hranic. Řešení je přidat trasy definované uživatelem (udr) pro [tyto rozsahy IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges) tak, aby provoz replikace nelze přejít na místě.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Připojení mezi umístění cíle a místní

Postupujte podle následujících pokynů pro připojení mezi cílové umístění a místní umístění:
- Pokud aplikace potřebuje k připojení k místní počítače nebo pokud jsou klienti, kteří připojují k aplikaci z místního přes VPN nebo ExpressRoute, zajistěte, abyste měli alespoň [připojení site-to-site](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mezi vaší cílové oblasti Azure a místního datového centra.

- Pokud očekáváte velké množství přenosu mezi cílových oblast Azure a místního datového centra, měli byste vytvořit další [připojení ExpressRoute](../../expressroute/expressroute-introduction.md) mezi cílovou oblast Azure a místního datového centra.

- Pokud chcete zachovat IP adresy pro virtuální počítače po jejich převzetí služeb při selhání, zachovat připojení site na lokality nebo ExpressRoute cílová oblast v odpojeném stavu. Toto je zajistit, neexistuje žádný rozsah kolidovat mezi oblasti zdroj rozsahy IP adres a rozsahů IP cílová oblast.

### <a name="best-practices-for-expressroute-configuration"></a>Osvědčené postupy pro konfiguraci ExpressRoute
Postupujte podle těchto osvědčené postupy pro konfiguraci ExpressRoute:

- Budete muset vytvořit okruh ExpressRoute v oblasti pro zdroj i cíl. Pak musíte vytvořit spojení mezi:
  - Virtuální síť zdroje a okruh ExpressRoute.
  - Cílová virtuální síť a okruh ExpressRoute.

- Jako součást standardní ExpressRoute můžete vytvořit okruhů ve stejné geopolitické oblasti. K vytvoření okruhy ExpressRoute v různých geopolitických oblastí, Azure ExpressRoute Premium je nutné, což zahrnuje přírůstkové náklady. (Pokud už používáte ExpressRoute Premium, že nejsou zpoplatněné.) Další podrobnosti najdete v tématu [dokumentu umístění ExpressRoute](../../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) a [ExpressRoute ceny](https://azure.microsoft.com/pricing/details/expressroute/).

- Doporučujeme použít jiný rozsahy IP adres v zdrojové a cílové oblasti. Okruh ExpressRoute, nebudou moct připojit k dvou virtuálních sítí Azure stejné rozsahů IP adres ve stejnou dobu.

- Můžete vytvářet virtuální sítě pomocí stejné rozsahů IP v obou oblastech a pak vytvořte okruhy ExpressRoute v obou oblastí. V případě převzetí služeb při selhání události odpojte okruhu z virtuální sítě zdroje a připojte je okruh ve virtuální síti cíl.

 >[!IMPORTANT]
 > Pokud primární oblasti se úplně dolů, může selhat operace odpojení. Získání připojení ExpressRoute, nebude možné cílové virtuální síti.

## <a name="next-steps"></a>Další kroky
Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](azure-to-azure-quickstart.md).
