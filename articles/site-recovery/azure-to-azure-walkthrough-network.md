---
title: "Plán sítí pro VMware do Azure s replikací pomocí Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě požadované při replikaci virtuálních počítačů Azure s Azure Site Recovery"
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
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: 7b37e853f6b97ba313111f9201f877846a28fae9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Krok 3: Plánování sítě pro replikaci virtuálního počítače Azure

Jakmile ověříte, [požadavky nasazení](azure-to-azure-walkthrough-prerequisites.md), přečtěte si tento článek vám pomůže porozumět aspekty sítě při replikaci a obnovení virtuálních počítačů Azure (VM) z jedné oblasti Azure do jiné, pomocí Azure Služba Site Recovery. 

- Po dokončení článku měli vědět, jak nastavit odchozí přístup pro virtuální počítače Azure, které chcete replikovat a jak se připojit z místního serveru pro tyto virtuální počítače.
- Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Azure replikace virtuálního počítače pomocí Site Recovery je aktuálně ve verzi preview.



## <a name="network-overview"></a>Přehled sítě

Virtuální počítače Azure se obvykle nacházejí v Azure virtuální síť nebo podsíť, a je připojení z místního serveru do Azure pomocí Azure ExpressRoute, nebo připojení k síti VPN.

Sítě jsou obvykle chráněné pomocí brány firewall nebo skupin zabezpečení (Nsg) sítě. Brány firewall můžete použít na základě adresy URL v seznamech založenou na protokolu IP, k připojení k síti ovládacího prvku. Pravidla na základě rozsahu IP použít skupiny Nsg. 


Site Recovery k fungovat očekávaným budete muset provést některé změny v odchozí síťové připojení z virtuálních počítačů, které chcete replikovat. Site Recovery nepodporuje použití proxy služby ověřování pro připojení k síti ovládacího prvku. Pokud máte proxy služby ověřování, nelze povolit replikaci. 


Následující diagram znázorňuje typické prostředí pro aplikace běžící na virtuálních počítačích Azure.

![prostředí zákazníka](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Prostředí virtuálních počítačů Azure**

Také může mít připojení k Azure nastavení z vaší místní lokality pomocí Azure ExpressRoute nebo připojení k síti VPN. 

![prostředí zákazníka](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Místní připojení k Azure**



## <a name="outbound-connectivity-for-urls"></a>Odchozí připojení pro adresy URL

Pokud používáte proxy server brány firewall založená na adresu URL k řízení odchozí připojení, ujistěte se, že povolíte, aby tyto adresy URL používá Site Recovery

**ADRESA URL** | **Podrobnosti**  
--- | ---
*.blob.core.windows.net | Umožňuje zápis z virtuálního počítače, k účtu úložiště mezipaměti v oblasti zdroj dat.
Login.microsoftonline.com | Poskytuje autorizaci a ověřování adresy URL služby Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Umožňuje komunikaci se službou Site Recovery z virtuálního počítače.
*. servicebus.windows.net | Vyžaduje, aby data monitorování a Diagnostika Site Recovery je možné zapsat z virtuálního počítače.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Odchozí připojení pro rozsahy IP adres

- Můžete automaticky vytvořit požadované pravidla v této skupině stažení a spuštění [tento skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Doporučujeme vytvořit požadované pravidla NSG na testovací skupina NSG a ověřte, že neexistují žádné problémy, před vytvořením pravidel u produkčních NSG.
- Vytvořit požadovaný počet pravidel NSG, ujistěte se, že vaše předplatné je seznam povolených adres. Požádejte podporu o zvýšení limitu pravidla NSG v rámci vašeho předplatného.
Pokud používáte všechny proxy serveru na základě IP brány firewall nebo pravidla NSG k řízení odchozí připojení, musí být seznam povolených adres, v závislosti na zdrojové a cílové umístění virtuálních počítačů následující rozsahy IP:

    - Všechny rozsahy IP adres, které odpovídají umístění zdroje. Stažení [rozsahy](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vytvoření seznamu povolených je nutné, aby data je možné zapsat do mezipaměti účet úložiště z virtuálního počítače.
    - Všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Pokud nové IP adres se přidají do rozsahů Office 365 IP, budete muset vytvořit nová pravidla NSG.
-     Lokality obnovení služby koncový bod IP adresy ([k dispozici v souboru XML](https://aka.ms/site-recovery-public-ips)), které závisí na cílové umístění: 

   **Cílové umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
   --- | --- | ---
   Východní Asie | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Jihovýchodní Asie | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Střed Indie | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Indie – jih | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Střed USA – sever | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Severní Evropa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Západní Evropa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Východ USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Západní USA | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Střed USA – jih | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Střed USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Východní USA 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Japonsko – východ | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Japonsko – západ | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Brazílie – jih | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Austrálie – východ | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Austrálie – jihovýchod | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Střední Kanada | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Východní Kanada | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Západní střed USA | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Západní USA 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Spojené království – západ | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Spojené království – jih | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Příklad konfigurace NSG

V této části ukazuje, jak nakonfigurovat pravidla NSG, tak, aby replikace funguje pro virtuální počítač. Pokud používáte pravidla NSG k řízení odchozí připojení, použijte "Povolit HTTPS odchozí" pravidla pro všechny požadované rozsahy IP.

V tomto příkladu se zdrojové umístění virtuálních počítačů "Východní USA". Cílové umístění replikace je střed USA.


### <a name="example"></a>Příklad

#### <a name="east-us"></a>Východ USA

1. Vytvoření pravidla, které odpovídají [rozsahy IP USA – východ](https://www.microsoft.com/download/confirmation.aspx?id=41653). To je potřeba, aby data je možné zapsat do mezipaměti účet úložiště z virtuálního počítače.
2. Vytvoření pravidel pro všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Vytvoření pravidla, která odpovídají do cílového umístění:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Střed USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Střed USA

Tato pravidla jsou vyžadována tak, aby je možné povolit replikaci z oblasti cíl pro danou oblast zdroj po převzetí služeb při selhání.

1. Vytvoření pravidla, které odpovídají [rozsahy IP centrální USA](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Vytvoření pravidel pro všechny rozsahy IP, které odpovídají Office 365 [ověřování a identita koncové body IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Vytvoření pravidla, které odpovídají umístění zdroje:

   **Umístění** | **Služba Site Recovery IP adresy** |  **Site Recovery monitorování IP**
    --- | --- | ---
   Východ USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Existující místní připojení

Pokud máte ExpressRoute nebo VPN připojení mezi místními servery a pak zdrojové umístění v Azure, postupujte podle těchto pokynů:

**Konfigurace** | **Podrobnosti**
--- | ---
**Vynuceného tunelování** | Výchozí trasa (0.0.0.0/0) obvykle vynutí odchozí přenosy z Internetu do procházet skrz místní umístění. Nedoporučujeme to. Provoz replikace a komunikaci Site Recovery může zůstat v rámci Azure.<br/><br/> Jako řešení, přidejte trasy definované uživatelem (udr) pro [tyto rozsahy IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges)tak, aby provoz nelze přejít na místě.
**Připojení** | Pokud aplikace třeba se připojit k místní počítače nebo místní klienti se připojují k aplikaci přes místní přes VPN nebo ExpressRoute, ujistěte se, máte alespoň [připojení site-to-site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), mezi cílové oblasti Azure a místní lokalita.<br/><br/> Pokud jsou svazky provoz vysoké mezi cílovou oblast Azure a místními servery, vytvořte další [připojení ExpressRoute](../expressroute/expressroute-introduction.md), mezi místními a cílová oblast.<br/><br/> Pokud chcete zachovat IP adresy pro virtuální počítače po převzetí služeb při selhání, zachovat připojení site na lokality nebo ExpressRoute cílová oblast v odpojeném stavu. To zajišťuje, neexistují žádné střetům rozsahu mezi zdrojem a cílem rozsahy IP adres.
**ExpressRoute** | Vytvoření okruhu ExpressRoute v oblasti zdroje a cíle.<br/><br/> Umožňuje vytvořte připojení mezi zdrojovou síť a okruh ExpressRoute a mezi Cílová síť a okruh.<br/><br/> Doporučujeme použít jiný rozsahy IP adres v zdrojové a cílové oblasti. Okruh nebudou moci připojit k více než jeden sítě Azure s stejné rozsahy IP ve stejnou dobu.<br/><br/> Můžete vytvořit virtuální sítě pomocí stejné rozsahů IP v obou oblastí a pak vytvořte okruhy ExpressRoute v obou oblastí. Pro převzetí služeb při selhání odpojte okruhu z virtuální sítě zdroje a připojte je okruh ve virtuální síti cíl.<br/><br/> Pokud primární oblasti se úplně dolů, může selhat operace odpojení. V takovém případě cílové virtuální síti nebude získat připojení ExpressRoute.
**ExpressRoute Premium** | Okruhy můžete vytvořit ve stejné geopolitické oblasti.<br/><br/> Chcete-li vytvořit okruhy v různých geopolitických oblastí, je třeba Azure ExpressRoute Premium.<br/><br/> Premium je přírůstkové náklady. Pokud jste ho už používáte, není bez dalších nákladů.<br/><br/> Další informace o [umístění](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) a [ceny](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)

