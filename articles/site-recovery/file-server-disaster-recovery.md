---
title: "Chránit souborový server s využitím Azure Site Recovery"
description: "Tento článek popisuje, jak pomoci chránit souborový server s využitím Azure Site Recovery"
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
ms.openlocfilehash: cc585d6add9b9c5ce7f3379aeaf5ec79f5c61d51
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Chránit souborový server s využitím Azure Site Recovery 

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategii obchodní kontinuitu a po havárii obnovení (BCDR) tím, že vaše obchodní aplikace, které jsou dostupné během plánovaných a neplánovaných výpadků. Site Recovery spravuje a orchestruje zotavení po havárii místní počítače a virtuální počítače Azure (VM), včetně replikace, převzetí služeb při selhání a obnovení úlohy.

Tento článek popisuje, jak pomoct chránit souborový server s využitím Azure Site Recovery a dalších doporučeních podle různých prostředích.

## <a name="file-server-architecture"></a>Architektura serveru souboru
Otevřený, distribuované sdílení souborů systém poskytuje prostředí, kde geograficky rozptýlené uživatelé mohou spolupracovat na soubory bez kompromisů integrity požadavky. 

Typické místní server zahrnující soubor podporující velký počet souběžných uživatelů a obsahu položky distribuované replikace systému souborů (DFSR) používá replikace plánování a omezování šířky pásma. DFSR používá algoritmus komprese Remote Differential Compression (RDC), který můžete použít k efektivní aktualizaci souborů přes síť omezenou šířkou pásma. RDC rozpoznává vkládání, odstraňování a změnu uspořádání dat v souborech. Umožňuje DFSR k replikuje pouze změněný soubor bloky, pokud jsou aktualizovány soubory. 

V některých prostředích s souborového serveru jsou provedeny denní zálohy na dobu mimo špičku pro zotavení po havárii. Těchto prostředích nepoužívejte DFSR.

Následující topologie znázorňuje prostředí souborového serveru s DFSR implementována:
                
![Architektura DFSR](media/site-recovery-file-server/dfsr-architecture.JPG)

Na obrázku několik souborových serverů (označované jako členy) aktivně účastnit replikaci souborů mezi replikační skupiny. Obsah v replikované složce jsou k dispozici pro všechny klienty, kteří jsou odesílání požadavků na jednu z členů, i v případě jednoho z členů přejde do režimu offline.

## <a name="disaster-recovery-strategies-for-file-servers"></a>Strategie zotavení po havárii pro souborové servery

- **Replikace na souborovém serveru do Azure pomocí Azure Site Recovery**: když jeden nebo více místními souborových serverů je nedostupné, obnovení virtuálních počítačů můžete zapínají v Azure. Obnovení virtuálních počítačů můžete pak obsluhovat požadavky od klientů, místní, pokud je k dispozici připojení site-to-site VPN a služby Active Directory je nakonfigurován v Azure. Můžete provést v prostředí nakonfigurovat DFSR nebo v prostředí serveru jednoduchého souboru s žádné DFSR. 

- **Rozšíření virtuálního počítače Azure IaaS DFSR**: V prostředí clusterového souborového serveru s DFSR implementovat jednu navrhované přístup je rozšířit místní DFSR do Azure. Virtuální počítač Azure může provést roli souborového serveru. 

    Po zpracování závislostí připojení site-to-site VPN a služby Active Directory a DFSR je na místě, když jeden nebo více místními souborových serverů je nedostupný, klienti se pořád připojit ke virtuálního počítače Azure. Virtuální počítač Azure slouží žádosti.

    Doporučujeme tuto metodu, pokud vaše virtuální počítače konfiguracemi, které nepodporuje Azure Site Recovery. Příkladem takové konfiguraci je disk sdíleného clusteru, který se často používá v prostředích souborového serveru. DFSR také dobře funguje v prostředí s malou šířkou pásma s střední klidové vytížení. S tímto přístupem budete muset zohlednit další náklady na virtuální počítač Azure s a všechny doby spuštění.  

- **Používat synchronizaci souboru Azure replikovat vaše soubory**: Pokud se připravuje pro vaši cestu do cloudu, nebo se už používá virtuální počítač Azure, doporučujeme použít synchronizace souborů Azure. Tato služba nabízí synchronizuje plně spravované sdílené složky v cloudu, které jsou přístupné přes oborových standardů [Server Message Block ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protokol (SMB). Pak se můžete připojit Azure sdílené složky souběžně cloudové nebo místní nasazení systému Windows, Linux a systému macOS. 

Následující diagram vám může pomoct rozhodnout, jaká strategie pro vaše serverové prostředí souboru:

![rozhodovací strom.](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-for-making-a-disaster-recovery-decision"></a>Faktory k rozhodnutí pro zotavení po havárii

|Prostředí  |Doporučení  |Body, které je třeba zvážit |
|---------|---------|---------|
|Prostředí serveru soubor s nebo bez DFSR|   [Pomocí Azure Site Recovery pro replikaci](#replicate-an-on-premises-file-server-by-using-azure-site-recovery)   |    Site Recovery nepodporuje sdílené diskové clustery nebo NAS. Pokud vaše prostředí používá jedna z těchto konfigurací, použijte některý z dalších přístupů podle potřeby. <br> Azure Site Recovery nepodporuje protokol SMB 3.0. Replikovaný virtuální počítač bude obsahovat změny soubory pouze v případě změny se aktualizuje na původním umístění souboru. 
|Prostředí serveru soubor s DFSR     |  [Rozšíření DFSR do virtuálního počítače Azure IaaS](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR dobře funguje v prostředí crunched šířky pásma. Tento přístup, ale vyžaduje, abyste tak, aby měl virtuální počítač Azure, nastavení a spuštění vždy. Plánování je potřeba počítat s náklady na virtuální počítač.         |
|Virtuálních počítačů Azure IaaS     |     [Používat synchronizaci Azure File](#use-azure-file-sync-to-replicate-your-on-premises-files)   |     Ve scénáři zotavení po havárii používáte synchronizace souboru Azure při převzetí služeb při selhání, musí provést ruční akce zajistit, že sdílené složky jsou k dispozici v transparentní způsob, jak klientský počítač. Synchronizace služby Azure soubor vyžaduje port 445 otevřen v klientském počítači.     |


### <a name="site-recovery-support"></a>Podpora pro obnovení lokality
Protože replikace Site Recovery je nezávislá na aplikace, doporučení uvedená tady očekává se, že platí pro následující scénáře:
| Zdroj    |Sekundární lokality    |Do Azure
|---------|---------|---------|
|Azure| -|Ano|
|Hyper-V|   Ano |Ano
|VMware |Ano|   Ano
|Fyzický server|   Ano |Ano
 

> [!IMPORTANT]
> Než budete pokračovat s žádným z následujících dvou přístupů, nezapomeňte závislostí adres.

**Připojení Site-to-Site**: přímé připojení mezi místními servery a síť Azure, musí vytvořit a povolit komunikaci mezi servery. Můžete vytvořit zabezpečené připojení site-to-site VPN Azure virtuální sítě, který se použije jako lokality pro obnovení po havárii. Další informace najdete v tématu [na portálu Azure vytvořit připojení Site-to-Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Služby Active Directory**: DFSR závisí na službě Active Directory. To znamená, že doménová struktura služby Active Directory s řadiče místní domény je rozšířeno na web pro zotavení po havárii v Azure. I když nepoužíváte DFSR, pokud příslušné uživatele musí být ověřen pro přístup k jejich udělena, musíte provést tyto kroky.
Další informace najdete v tématu [rozšíření místní služby Active Directory do Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendations-for-azure-iaas-virtual-machines"></a>Doporučení pro zotavení po havárii pro virtuální počítače Azure IaaS

Pokud jsou konfigurace a správa zotavení po havárii souborových serverů, které jsou hostované na virtuální počítače Azure IaaS, můžete vybrat mezi dvě možnosti: synchronizace souboru Azure a Azure Site Recovery. Rozhodnutí, závisí na tom, jestli chcete přesunout do [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

### <a name="use-azure-file-sync-to-replicate-files-hosted-on-iaas-virtual-machines"></a>Používat synchronizaci souboru Azure k replikaci souborů, které jsou hostované na virtuální počítače IaaS

Soubory Azure můžete zcela nahradí nebo doplní tradičních místních souborových serverech nebo zařízeních NAS. Sdílené složky Azure můžete také replikují se synchronizací souboru Azure na Windows Server, buď místní nebo v cloudu pro původce a distribuované ukládání do mezipaměti dat, kde se používá. 

Následující kroky jsou upřesněny doporučení pro zotavení po havárii pro virtuální počítače Azure, který provádí stejnou funkci jako tradiční souborových serverů:
1. Ochrana počítačů pomocí Azure Site Recovery prostřednictvím zde uvedených pokynů.

2. Použijte synchronizaci souboru Azure replikovat soubory z virtuálního počítače, který funguje jako souborový server do cloudu.

3. Použití [plán obnovení](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-create-recovery-plans) funkce v Azure Site Recovery přidat skripty, které [připojit sdílenou složkou Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) a přístup do sdílené složky ve virtuálním počítači.

Následující kroky stručně popisují, jak používat službu Azure souboru Sync:

1. [Vytvořit účet úložiště v Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Pokud jste zvolili geograficky redundantní úložiště s přístupem pro čtení pro účty úložiště, zobrazí se přístup pro čtení k datům ze sekundární oblasti v případě havárie. Další informace najdete v tématu [strategie obnovení po havárii sdílenou složku Azure File](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Vytvoření sdílené složky](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Nasazení Azure souboru Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) na Azure souborovém serveru.

4. Vytvořte skupinu synchronizace. Koncové body v rámci skupiny synchronizace zůstanou vzájemně synchronizována. Synchronizace skupiny musí obsahovat alespoň jeden cloud koncový bod, který představuje sdílenou složku Azure, a jeden koncový bod serveru, který představuje cestu v systému Windows server.  
    Vaše soubory se teď zůstanou synchronizovány mezi Azure sdílené složky a místní server.

5. Pokud dojde k havárii v místním prostředí, proveďte pomocí plánu obnovení převzetí služeb při selhání. Přidejte skript, který chcete [připojit sdílenou složkou Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) a přístup do sdílené složky ve virtuálním počítači.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-azure-site-recovery"></a>Pomocí Azure Site Recovery replikovat virtuální počítač serveru soubor IaaS

Pokud máte místní klienty, kteří přistupují virtuální počítač IaaS souborového serveru, proveďte následující kroky. Jinak proveďte pouze krok 3.

1. Vytvořit připojení site-to-site VPN mezi místními servery a síť Azure.  

2. Místní službu Active Directory rozšiřte.

3. [Nastavit zotavení po havárii](azure-to-azure-tutorial-enable-replication.md) pro k počítači souborového serveru IaaS v sekundární oblasti.

Další informace o zotavení po havárii v sekundární oblasti najdete v tématu [Azure do Azure replikace architektura](concepts-azure-to-azure-architecture.md).

## <a name="disaster-recovery-recommendations-for-on-premises-virtual-machines"></a>Doporučení pro zotavení po havárii pro místní virtuální počítače 

### <a name="replicate-an-on-premises-file-server-by-using-azure-site-recovery"></a>Pomocí Azure Site Recovery replikovat pro místní souborový server
Následující kroky podrobnosti replikace pro virtuální počítače VMware. Pokyny k replikaci virtuálního počítače technologie Hyper-V, najdete v části [nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-hyper-v-to-azure).

1. [Příprava prostředků Azure](tutorial-prepare-azure.md) pro replikaci počítačů na místě.

2. Vytvořit připojení site-to-site VPN mezi místními servery a síť Azure.  

3. Místní službu Active Directory rozšiřte.

4. [Příprava na místní servery VMware](tutorial-prepare-on-premises-vmware.md).

5. [Nastavit zotavení po havárii](tutorial-vmware-to-azure.md) do Azure pro virtuální počítače na místě.

### <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Rozšíření DFSR do virtuálního počítače Azure IaaS

1. Vytvořit připojení site-to-site VPN mezi místními servery a síť Azure. 

2. Místní službu Active Directory rozšiřte.

3. [Vytvořit a zřídit souborový server virtuálního počítače](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) na virtuální síť Azure.  
    Ujistěte se, že virtuální počítač je přidat do stejné virtuální síť Azure, který má připojení mezi místní prostředí. 

4. Instalace a [konfigurace replikace DFS](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) v systému Windows Server.

5. [Implementace oboru názvů DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).

6. Obor názvů DFS implementována můžete převzít sdílených složek z výroby do lokalit pro zotavení po havárii aktualizací cíle složky oboru názvů DFS. Po tyto DFS replikovat změny oboru názvů prostřednictvím služby Active Directory, jsou uživatelé připojeni k cílům příslušné složky transparentně.

### <a name="use-azure-file-sync-to-replicate-your-on-premises-files"></a>Používat synchronizaci souboru Azure replikovat vaše soubory místně
Pomocí služby synchronizace souboru Azure může replikovat požadované soubory do cloudu. V případě havárie a nedostupnosti váš místní souborový server můžete pak připojte umístění požadované souborů z cloudu a pokračovat se žádostí o službu z klientských počítačů.

Doporučuje integrace synchronizace souboru Azure s Azure Site Recovery se:
1. Chraňte počítače souborového serveru pomocí Azure Site Recovery. Postupujte podle kroků v [nastavit zotavení po havárii do Azure pro virtuální počítače VMware místní](tutorial-vmware-to-azure.md).

2. Použijte synchronizaci souboru Azure replikovat soubory z počítače, který slouží jako souborový server, do cloudu.

3. Použijte funkci plán obnovení v Azure Site Recovery přidat skripty připojit sdílenou složkou Azure na při selhání souborového serveru virtuálního počítače v Azure.

Následující kroky podrobností pomocí služby Azure souboru Sync:

1. [Vytvořit účet úložiště v Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Pokud jste zvolili přístup pro čtení geograficky redundantní úložiště (doporučeno) pro účty úložiště, budete mít přístup pro čtení k datům ze sekundární oblasti v případě havárie. Další informace najdete v tématu [strategie obnovení po havárii sdílenou složku Azure File](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Vytvoření sdílené složky](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Nasazení Azure souboru Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) ve vaší místní souborový server.

4. Vytvořte skupinu synchronizace. Koncové body v rámci skupiny synchronizace zůstanou vzájemně synchronizována. Synchronizace skupiny musí obsahovat alespoň jeden cloud koncový bod, který představuje sdílenou složku Azure, a koncový bod, jeden server, který představuje cestu v místním systému Windows server.  
    Vaše soubory se teď zůstanou synchronizovány mezi Azure sdílené složky a místní server.

5. Pokud dojde k havárii v místním prostředí, proveďte pomocí plánu obnovení převzetí služeb při selhání. Přidejte skript, který chcete připojit sdílenou složkou Azure a přístup do sdílené složky ve virtuálním počítači.

> [!NOTE]
> Ujistěte se, že je otevřený port 445. Soubory Azure používá protokol SMB. SMB komunikuje přes TCP port 445. Zkontrolujte, zda brána firewall neblokuje port 445 v klientském počítači.


## <a name="perform-a-test-failover"></a>Provedení testu převzetí služeb

1. Přejděte na portál Azure a vyberte svůj trezor služeb zotavení.

2. Vyberte plán obnovení, které jsou vytvořené pro prostředí souboru serveru.

3. Vyberte **testovací převzetí služeb při selhání**.

4. Vyberte bod obnovení a virtuální síť Azure ke spuštění procesu testovací převzetí služeb při selhání.

5. Po sekundární prostředí, proveďte vaše ověření.

6. Když k ověření se dokončí, můžete zvolit **vyčistit testovací převzetí služeb při selhání** vyčištění v plánu obnovení a převzetí služeb při selhání testovací prostředí.

Další informace o provádění převzetí služeb při selhání najdete v tématu [testovací převzetí služeb při selhání do Azure ve službě Site Recovery](site-recovery-test-failover-to-azure.md).

Pokyny k provedení testu převzetí služeb při selhání pro Active Directory a DNS, najdete v části [testovací převzetí služeb při selhání důležité informace týkající se služby Active Directory a DNS](site-recovery-active-directory.md).

## <a name="perform-a-failover"></a>Provedení převzetí služeb při selhání

1. Přejděte na portál Azure a vyberte svůj trezor služeb zotavení.

2. Vyberte plán obnovení, které jsou vytvořené pro prostředí souboru serveru.

3. Vyberte **převzetí služeb při selhání**.

4. Vyberte bod obnovení se spustit proces převzetí služeb při selhání.

Další informace o provádění převzetí služeb při selhání najdete v tématu [převzetí služeb při selhání ve službě Site Recovery](site-recovery-failover.md).
