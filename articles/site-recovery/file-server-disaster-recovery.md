---
title: "Chránit souborový Server pomocí Azure Site Recovery"
description: "Tento článek popisuje, jak chránit souborový Server pomocí Azure Site Recovery"
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 8c9d8dadcd6181d9894ab6ee7110841afdec5708
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="protect-a-file-server-using-azure-site-recovery"></a>Chránit souborový Server pomocí Azure Site Recovery 

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení ponechat obchodních aplikací stále a spuštěním dostupné během plánovaných a neplánovaných výpadků. Site Recovery spravuje a orchestruje zotavení po havárii místní počítače a virtuální počítače Azure (VM), včetně replikace, převzetí služeb při selhání a obnovení úlohy.

Tento článek popisuje, jak chránit souborovém serveru pomocí Azure Site Recovery a dalších doporučeních podle různých prostředích.     

- [Replikovat počítače Azure IaaS souborového serveru](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Replikovat místní souborovém serveru pomocí Azure Site Recovery](#replicate-an-onpremises-file-server-using-azure-site-recovery)

## <a name="file-server-architecture"></a>Architektura serveru souboru
Cílem soubor otevřený distribuované sdílení systému je a poskytuje prostředí, kde mohou spolupracovat na skupinu uživatelů geograficky rozptýlené efektivní práci se soubory a zaručit, aby se vynucují jejich integritu požadavků. Typické místní ekosystém souborový Server, podporující velký počet souběžných uživatelů a velké množství obsahu položek pomocí distribuovaných replikace systému souborů (DFSR) replikace plánování a omezování šířky pásma. DFSR používá algoritmus komprese označovaný jako Remote Differential Compression (RDC), který slouží k efektivní aktualizaci souborů přes síť omezenou šířkou pásma. Zjistí vkládání, odstraňování a změnu uspořádání dat v souborech, povolení DFSR k replikuje pouze změněný soubor bloky, pokud jsou aktualizovány soubory. Existují také souborový Server, prostředí, kde denní zálohy se provádějí v časování mimo špičku, které slouží různým potřebovat po havárii a neexistuje žádná implementace DFSR.

Následující topologie znázorňuje prostředí souborový Server s DFSR implementována.
                
![DFSR architexture](media/site-recovery-file-server/dfsr-architecture.JPG)

Několik souborových serverů v výše uvedený odkaz, označuje jako členové, aktivně podílet na replikaci souborů mezi replikační skupiny. Obsah v replikované složce bude k dispozici pro všechny klienty odesílání požadavků na jednu z členů, i v případě jednoho z členů přechod do stavu offline.

## <a name="disaster-recovery-recommendation-for-file-servers"></a>Doporučení pro zotavení po havárii pro souborové servery:

1.  Replikovat souborovém serveru pomocí Azure Site Recovery: souborových serverů je možné replikovat do Azure pomocí Azure Site Recovery. Pokud jeden nebo více souborů servery místní nedostupné, může být zapínají obnovení virtuálních počítačů v Azure, což může pak používat požadavky od klientů, místní, existuje-li je připojení Site-to-Site VPN a služby Active directory, které jsou nakonfigurované v Azure. To lze provést v případě jednoduchého souboru prostředí serveru s žádné DFSR nebo prostředí DFSR nakonfigurované. 

2.  Rozšíření virtuálního počítače Azure Iaas DFSR:-v prostředí clusterového souborového serveru s DFSR implementovat jednu navrhované přístup je rozšířit místní DFSR do Azure. Virtuální počítač Azure je pak povoleno provádět roli souborového serveru. 

    Jakmile jsou zpracovávány závislostí připojení Site-to-Site VPN a služby Active directory a DFSR je na místě, když jeden nebo více souborů servery místní je nedostupný, můžete stále se klienti připojují, virtuální počítač Azure, který bude sloužit žádosti.

    Tento přístup je navržený v případě virtuálních počítačů konfiguracemi, které nejsou podporovány službou Azure Site Recovery, jako například: clusteru sdíleného disku, které se někdy běžně používá v prostředích souborového serveru.  DFSR také dobře funguje v prostředí s malou šířkou pásma s střední klidové vytížení. Další náklady na virtuální počítač Azure s a neustále spuštěné taky umístěnými s tím musí.  

3.  Replikovat vaše soubory pomocí služby Azure souboru Sync: Pokud se připravuje pro vaši cestu do cloudu, nebo se už používá virtuální počítač Azure, pak doporučujeme používání Azure File synchronizační služby, který nabízí synchronizuje plně spravované sdílené složky v cloudu, které jsou dostupné v IA oborový standard [Server Message Block ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protokol (SMB). Azure soubor, který pak sdílené složky současně pomocí připojené cloudové nebo místní nasazení systému Windows, Linux a systému macOS. 

Následující diagram poskytuje vizuální reprezentaci zaměřené na usnadnění na rozhodnutí, jaké strategie pro vaše serverové prostředí a souboru.

![decisiontree](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-while-making-decision-of-disaster-recovery-to-azure"></a>Faktory, které je potřeba zvážit při rozhodování o zotavení po havárii do Azure

|Prostředí  |Doporučení  |Body, které je třeba zvážit |
|---------|---------|---------|
|Soubor prostředí serveru/bez DFSR|   [Pomocí Azure Site Recovery pro replikaci](#replicate-an-onpremises-file-servers-using-azure-site-recovery)   |    Site Recovery nepodporuje sdílený disk clusteru, NAS. Pokud vaše prostředí používá některý z těchto konfigurací, používejte kterýkoli z jiných přístupů podle potřeby. <br> Azure Site Recovery nepodporuje protokol SMB 3.0, což znamená, že pouze v případě změny provedené v souborech se aktualizuje v původním umístění souboru bude replikovaný virtuální počítač projevily změny.
|Prostředí serveru soubor s DFSR     |  [Rozšíření DFSR do virtuálního počítače Azure IaaS:](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |     DFSR skvěle funguje ve velmi šířky pásma crunched prostředích, tento přístup, ale vyžaduje tak, aby měl virtuální počítač Azure nahoru a všechny doby spuštění. Náklady na virtuální počítač musí být zahrnuté v plánování.         |
|Virtuálních počítačů Azure Iaas     |     [Synchronizace Azure File](#use-azure-file-sync-service-to-replicate-your-files)   |     Ve scénáři zotavení po Havárii Pokud používáte Azure synchronizace souborů, během převzetí služeb při selhání ručně prováděné akce muset třeba zajistit, že soubor sdílí jako přístupné transparentním způsobem klientský počítač. AFS vyžaduje port 445 otevřen v klientském počítači.     |


### <a name="site-recovery-support"></a>Podpora pro obnovení lokality
Site Recovery replikace je nezávislá na aplikace, budou doporučení uvedená tady platí pro následující scénáře:
| Zdroj    |Sekundární lokality    |Do Azure
|---------|---------|---------|
|Azure| -|Ano|
|Hyper-V|   Ano |Ano
|VMware |Ano|   Ano
|Fyzický server|   Ano |Ano
 

> [!IMPORTANT]
> Než budete pokračovat s žádným z následující tři přístupy, ujistěte se, že následující závislé položky bylo postaráno:

**Připojení Site-to-Site**: přímé připojení mezi místními servery a síť Azure, musí vytvořit a povolit komunikaci mezi servery.  Tím lze zajistit bezpečné připojení VPN typu Site-to-Site k virtuální síti Microsoft Azure, který se použije jako lokality pro zotavení po Havárii.  
Odkazovat: [navázání Site-to-Site VPN připojení mezi místními servery a síť Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Služby Active Directory**: DFSR závisí na službě Active Directory.  To znamená, že doménová struktura služby Active Directory s řadiče místní domény je rozšířeno na web zotavení po Havárii v Azure. I když nepoužíváte DFSR příslušným uživatelům muset být udělena / ověřit přístup, jako je ve většině organizaci, je nutné provést tyto kroky.
Odkazovat: [místní službu Active Directory rozšířit do Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Doporučení pro zotavení po havárii pro virtuální počítače Azure IaaS

Pokud nakonfigurujete a spravovat zotavení po havárii souborových serverů, které jsou hostované na virtuální počítače Azure IaaS, můžete si vybrat mezi dvě možnosti, v závislosti na tom, jestli chcete přesunout do [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

1. [Používat synchronizaci Azure File](#use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine)
2. [Použití Azure Site Recovery](#replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery)

## <a name="use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine"></a>Pomocí služby Azure souboru Sync k replikaci souborů, které jsou hostované na virtuálním počítači IaaS

**Soubory Azure** slouží k zcela nahradí nebo doplní tradičních místních souborových serverech nebo zařízeních NAS. Sdílené složky Azure je také možné pomocí služby Azure File Sync replikovat na místní nebo cloudové servery Windows pro zajištění výkonného a distribuovaného ukládání dat do mezipaměti v místě, kde se používají. Následující kroky jsou upřesněny zotavení po Havárii doporučení pro virtuální počítače Azure, které provádějí stejné funkce jako tradiční souborových serverů:
1.  Ochrana počítačů pomocí Azure Site Recovery pomocí uvedených pokynů [zde](azure-to-azure-quickstart.md).
2.  Použijte synchronizaci souboru Azure replikovat soubory z virtuálního počítače, který funguje jako souborový server do cloudu.
3.  Pomocí Azure Site Recovery [plán obnovení](site-recovery-create-recovery-plans.md) funkci přidat skripty, které [připojit sdílenou složku Azure File](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) a přístup do sdílené složky ve virtuálním počítači.

Níže uvedených pokynů popisují stručně používání služby Azure souboru Sync:

1. [Vytvořit účet úložiště v Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Pokud jste zvolili geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) pro účty úložiště, získáte přístup pro čtení k datům ze sekundární oblasti v případě havárie. Odkazovat [strategie obnovení po havárii sdílenou složku Azure File](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) Další informace.
2. [Vytvoření sdílené složky](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Nasazení Azure souboru Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) na Azure souborovém serveru.
4. Vytvořte skupinu synchronizace: koncové body v rámci skupiny synchronizace se budou uchovávat vzájemně synchronizována. Synchronizace skupiny musí obsahovat alespoň jeden koncového bodu cloudu, který představuje sdílenou složku Azure, a jeden koncový bod serveru, který představuje cestu na serveru systému Windows.
5.  Vaše soubory se teď sesynchronizovávat napříč Azure sdílené složky a místní server.
6.  V případě havárie v místním prostředí, proveďte převzetí služeb při selhání pomocí [plán obnovení](site-recovery-create-recovery-plans.md) a přidejte skript, který chcete [připojit sdílenou složku Azure File](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) a přístup do sdílené složky ve virtuálním počítači.

### <a name="replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery"></a>Replikovat IaaS souborového serveru virtuálního počítače pomocí Azure Site Recovery

Pokud máte místní klientům přístup k IaaS virtuálního počítače souborového serveru proveďte první dva kroky taky else pokračujte krokem 3.

1. Navázání připojení Site-to-Site VPN mezi místními servery a sítě Azure.
1. Místní službu Active Directory rozšiřte.
1. [Nastavit zotavení po havárii](azure-to-azure-tutorial-enable-replication.md) pro k počítači souborového serveru IaaS v sekundární oblasti.


Další informace o zotavení po havárii v sekundární oblasti, najdete v části [zde](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-using-azure-site-recovery"></a>Replikovat pomocí Azure Site Recovery pro místní souborový server

Níže kroky podrobnosti replikace pro virtuální počítače VMware, kroky pro replikaci virtuálního počítače technologie Hyper-V, najdete v [zde](tutorial-hyper-v-to-azure.md).

1.  [Příprava prostředků Azure](tutorial-prepare-azure.md) pro replikaci počítačů na místě.
2.  Navázání připojení Site-to-Site VPN mezi místními servery a sítě Azure.  
3. Místní službu Active Directory rozšiřte.
4.  [Příprava na místní servery VMware](tutorial-prepare-on-premises-vmware.md).
5.  [Nastavit zotavení po havárii](tutorial-vmware-to-azure.md) do Azure pro virtuální počítače na místě.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Rozšíření DFSR do virtuálního počítače Azure IaaS:

1.  Navázání připojení Site-to-Site VPN mezi místními servery a sítě Azure. 
2.  Místní službu Active Directory rozšiřte.
3.  [Vytvořit a zřídit souborový server virtuálního počítače](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) ve virtuální síti systému Windows Azure.
Ujistěte se, že virtuální počítač je přidat do stejné Windows Azure virtuální síti, která má křížové spojení s místním prostředí. 
4.  Instalace a [konfigurace replikace DFS](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) v systému Windows Server.
5.  [Implementace systému souborů DFS Namespace](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6.  Pomocí systému souborů DFS Namespace, implementována lze provést převzetí služeb při selhání sdílených složek z výroby na zotavení po Havárii lokality aktualizací cíle složky systému souborů DFS Namespace.  Jakmile se tyto změny Namespace DFS replikovat prostřednictvím služby Active Directory, jsou uživatelé připojeni k cílům příslušné složky transparentně.

## <a name="use-azure-file-sync-service-to-replicate-your-on-premises-files"></a>Pomocí služby Azure souboru Sync replikovat vaše místní soubory:
Pomocí služby synchronizace souboru Azure, můžete replikovat požadované soubory do cloudu, tak, aby v případě havárie a nedostupnosti váš místní souborový server, můžete připojit z cloudu na umístění požadovaných souborů a pokračovat se žádostí o službu z klientské počítače.
Doporučuje integrace synchronizace souboru Azure s Azure Site Recovery se
1.  Ochrana počítače serveru soubor pomocí Azure Site Recovery pomocí uvedených pokynů [zde](tutorial-vmware-to-azure.md).
2.  Použijte synchronizaci souboru Azure replikovat soubory z počítače, který slouží jako souborový Server, do cloudu.
3.  Pomocí Azure Site Recovery funkce plánu obnovení přidat skripty připojit sdílenou složku Azure File na neúspěšný přes souborovém serveru virtuálního počítače v Azure.

Pod pomocí služby Azure souboru Sync podrobné kroky:

1. [Vytvořit účet úložiště v Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Pokud jste zvolili přístup pro čtení geograficky redundantní úložiště (RA-GRS) (doporučeno) pro účty úložiště, máte přístup pro čtení k datům ze sekundární oblasti v případě havárie. Odkazovat [strategie obnovení po havárii sdílenou složku Azure File](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) Další informace.
2. [Vytvoření sdílené složky](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Nasazení Azure souboru Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) ve vaší místní souborový server.
4. Vytvořte skupinu synchronizace: koncové body v rámci skupiny synchronizace se budou uchovávat vzájemně synchronizována. Synchronizace skupiny musí obsahovat alespoň jeden koncového bodu cloudu, který představuje sdílenou složku Azure, a jeden koncový bod serveru, který představuje cestu v místním systému Windows Server.
1. Vaše soubory se teď sesynchronizovávat napříč Azure sdílené složky a místní server.
6.  V případě havárie v místním prostředí, proveďte převzetí služeb při selhání pomocí [plán obnovení](site-recovery-create-recovery-plans.md) a přidejte skript, který chcete připojit sdílenou složku Azure File a přístup do sdílené složky ve virtuálním počítači.

> [!NOTE]
> Ujistěte se, je otevřený port 445: Azure souborů používá protokol SMB. Protokol SMB komunikuje přes protokol TCP 445 – zkontrolujte, že brána firewall neblokuje port TCP 445 z klientského počítače.


## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb

1.  Přejděte na portál Azure a vyberte trezoru služby zotavení.
2.  Klikněte na plán obnovení, které jsou vytvořené pro prostředí souborovém serveru.
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte bod obnovení a virtuální síť Azure ke spuštění procesu testovací převzetí služeb při selhání.
5.  Jakmile sekundární prostředí zapnutý, můžete provést vaše ověření.
6.  Po dokončení k ověření se 'vyčistit testovací převzetí služeb při selhání, můžete kliknutím na plánu obnovení a vyčištění prostředí testovacího převzetí služeb při selhání.

Další informace o provedení testu převzetí služeb při selhání, najdete v části [zde](site-recovery-test-failover-to-azure.md).

Pokyny k provádění převzetí služeb při selhání pro AD a DNS, odkazovat na [testovací převzetí služeb při selhání aspekty AD a DNS](site-recovery-active-directory.md).

## <a name="doing-a-failover"></a>Převzetím služeb

1.  Přejděte na portál Azure a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení, které jsou vytvořené pro prostředí souborovém serveru.
3.  Klikněte na 'Převzetí služeb při selhání'.
4.  Vyberte bod obnovení se spustit proces převzetí služeb při selhání.

Další informace o provádění převzetí služeb při selhání, najdete v části [zde](site-recovery-failover.md).
