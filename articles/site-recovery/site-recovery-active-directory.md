---
title: "Ochrana služby Active Directory a DNS s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak implementovat řešení zotavení po havárii pro službu Active Directory přes Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 7/20/2017
ms.author: pratshar
ms.openlocfilehash: 197441fc24c178695d4eada6db59f503b21672ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Ochrana služby Active Directory a DNS s Azure Site Recovery
Podnikové aplikace, například SharePoint, Dynamics AX a SAP závisí na služby Active Directory a infrastruktury služby DNS, aby správně fungoval. Když vytvoříte řešení zotavení po havárii pro aplikace, je důležité si pamatovat, že budete muset ochrana a obnovení Active Directory a DNS než ostatní aplikace součásti, aby věcí fungovat správně při dojde k havárii.

Site Recovery je služba Azure, která poskytuje zotavení po havárii tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů. Site Recovery podporuje různé scénáře replikace konzistentně chránit a bezproblémově převzetí služeb při selhání virtuálního počítače a aplikace do privátního, public nebo hostitel cloudů.

Pomocí Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro službu Active Directory. Pokud dojde k přerušení, můžete zahájit převzetí služeb při selhání během několika sekund z libovolného místa a zprovoznění služby Active Directory za pár minut. Pokud jste nasadili služby Active Directory pro více aplikací, jako je například Sharepointu a SAP v primární lokalitě a chcete převzít dokončení lokality, můžete převzít služby Active Directory nejprve pomocí Site Recovery a pak převzít i další aplikace, pomocí plánů obnovení specifické pro aplikaci.

Tento článek vysvětluje, jak vytvořit řešení zotavení po havárii pro Active Directory, jak provést plánované, neplánované a testovací převzetí služeb při selhání pomocí plán obnovení jedním kliknutím, podporované konfigurace a požadavky.  Byste měli být obeznámeni s Active Directory a Azure Site Recovery, před zahájením.

## <a name="replicating-domain-controller"></a>Replikace řadiče domény

Potřebujete instalaci [Site Recovery replikaci](#enable-protection-using-site-recovery) na alespoň jeden virtuální počítač hostující řadič domény a DNS. Pokud máte [několika řadičů domény](#environment-with-multiple-domain-controllers) ve vašem prostředí, kromě replikace virtuálního počítače řadiče domény pomocí Site Recovery také nutné nastavit [další řadič domény](#protect-active-directory-with-active-directory-replication) v cílové lokalitě (Azure nebo do sekundárního místního datacentra). 

### <a name="single-domain-controller-environment"></a>Prostředí řadičů jedné domény.
Pokud máte několik aplikací a jenom jeden řadič domény a chcete převzetí služeb při selhání celá lokalita společně, pak doporučujeme používat Site Recovery k replikaci řadič domény do sekundární lokality (jestli je jste přebírání služeb při selhání do Azure nebo do sekundární lokality). Stejné domény, řadiče nebo DNS virtuálního počítače lze použít pro replikované [testovací převzetí služeb při selhání](#test-failover-considerations) také.

### <a name="environment-with-multiple-domain-controllers"></a>Prostředí s více řadiči domény
Pokud máte mnoho aplikací a existuje více než jeden řadič domény v prostředí, nebo pokud plánujete převzetí služeb při selhání několik aplikací najednou, doporučujeme, aby kromě replikace virtuálního počítače řadiče domény pomocí Site Recovery můžete také nastavit [další řadič domény](#protect-active-directory-with-active-directory-replication) v cílové lokalitě (Azure nebo do sekundárního místního datacentra). Pro [testovací převzetí služeb při selhání](#test-failover-considerations), použít řadič domény replikovaný pomocí Site Recovery a převzetí služeb při selhání, další řadiče domény v cílové lokalitě. 


Následující části popisují, jak povolit ochranu pro řadič domény ve službě Site Recovery a jak nastavit řadiče domény v Azure.

## <a name="prerequisites"></a>Požadavky
* Místní nasazení služby Active Directory a DNS serveru.
* Trezoru služby služeb Azure Site Recovery v rámci předplatného Microsoft Azure.
* Pokud replikujete do Azure, spusťte nástroj hodnocení připravenosti virtuální počítač Azure na virtuálních počítačích pro zajištění, že jsou kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery.

## <a name="enable-protection-using-site-recovery"></a>Povolit ochranu pomocí Site Recovery
### <a name="protect-the-virtual-machine"></a>Ochrana virtuálního počítače
Povolení ochrany virtuálního počítače řadiče a DNS domény ve službě Site Recovery. Nakonfigurujte nastavení obnovení lokality podle typu virtuálního počítače (Hyper-V nebo VMware). Řadiče domény replikovat pomocí Site Recovery se používá pro [testovací převzetí služeb při selhání](#test-failover-considerations). Ujistěte se, že splňuje následující požadavky:

1. Je řadič domény serverem globálního katalogu
2. Řadič domény musí být vlastníka role, které budou potřebovat během testovací převzetí služeb role FSMO (jinak bude nutné tyto role se [převzaty](http://aka.ms/ad_seize_fsmo) po převzetí služeb po)

### <a name="configure-virtual-machine-network-settings"></a>Konfigurace nastavení sítě virtuálního počítače
Pro virtuální počítač řadiče a DNS domény konfigurace nastavení sítě v Site Recovery, tak, aby virtuální počítač bude připojen k síti správné po převzetí služeb při selhání. 

![Nastavení sítě VM](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Ochrana služby Active Directory s replikací služby Active Directory
### <a name="site-to-site-protection"></a>Site-to-site ochrany
Vytvoření řadiče domény v sekundární lokalitě. Při povýšení serveru role řadiče domény, zadejte název stejné domény, který se používá v primární lokalitě. Můžete použít **Active Directory Sites and Services** modul snap-in ke konfiguraci nastavení v objektu propojení lokalit, do které jsou přidány webů. Konfigurace nastavení na propojení lokalit, můžete řídit, když se replikují mezi dva nebo více lokalit a jak často. Další informace najdete v tématu [plánování replikace mezi lokalitami](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Ochrana serveru Azure
Postupujte podle pokynů a [vytvořit řadič domény v virtuální sítě Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Při povýšení serveru role řadiče domény, zadejte se stejným názvem domény, který se používá v primární lokalitě.

Potom [překonfigurovat server DNS pro virtuální síť](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network)můžete použít DNS server v Azure.

![Síť Azure](./media/site-recovery-active-directory/azure-network.png)

**Služba DNS v produkční sítě Azure**

## <a name="test-failover-considerations"></a>Aspekty testovací převzetí služeb při selhání
Testovací převzetí služeb při selhání dojde v síti, která bude izolovaná od produkční sítě tak, aby v produkčním prostředí není žádný vliv.

Většina aplikací také vyžaduje přítomnost řadič domény a serveru DNS pro funkce. Proto předtím, než aplikace při selhání, řadič domény se musí vytvořit v izolované sítě, který se má použít pro testovací převzetí služeb při selhání. Nejjednodušším způsobem je replikovat virtuální počítač řadiče a DNS domény pomocí Site Recovery. Testovací převzetí služeb virtuálního počítače pro řadič domény pak spusťte před spuštěním testovací převzetí služeb při selhání plánu obnovení pro aplikaci. Zde je, jak můžete udělat:

1. [Replikovat](site-recovery-replicate-vmware-to-azure.md) domény řadiče a DNS virtuálního počítače pomocí Site Recovery.
1. Vytvořte izolovanou síť. Všechny virtuální sítě vytvořené v Azure ve výchozím nastavení je izolovaná od jiných sítích. Doporučujeme vám, že je stejná jako u sítě rozsah IP adres pro tuto síť. Nepovolíte připojení site-to-site v této síti.
1. Zadejte adresu IP serveru DNS v síti vytvořen jako IP adresu, která očekáváte, že virtuální počítač DNS získat. Pokud replikujete do Azure, zadejte IP adresu pro virtuální počítač, který se používá na převzetí služeb při selhání v **cílová IP adresa** nastavení v **výpočty a síť** nastavení. 

    ![Cílové IP](./media/site-recovery-active-directory/DNS-Target-IP.png) **cílové IP**

    ![Azure testovací síti](./media/site-recovery-active-directory/azure-test-network.png)

    **Služba DNS v síti Azure testu**

> [!TIP]
> Site Recovery se pokusí vytvořit testovací virtuální počítače v podsíti stejný název a použití stejnou IP Adresou, který je součástí **výpočty a síť** nastavení virtuálního počítače. Pokud podsíť stejný název není k dispozici ve virtuální síti Azure, zadaný pro testovací převzetí služeb při selhání, pak testovací virtuální počítač je vytvořen v první podsíť abecedy. Pokud cílová IP adresa je součástí vybrané podsítě, Site Recovery pokusí o vytvoření testovacího převzetí služeb při selhání virtuálního počítače pomocí cílová IP adresa. Pokud cílová IP adresa není součástí vybrané podsítě, pak testovacího převzetí služeb při selhání virtuálního počítače se vytvoří pomocí dostupnou IP adresu do vybrané podsítě. 
>
>


1. Pokud replikujete do jiné lokality v místě a používáte DHCP, postupujte podle pokynů a [nastavení DNS a DHCP pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Proveďte testovací převzetí služeb virtuálního počítače řadiče domény spustit v izolované sítě. Použijte nejnovější dostupné **aplikace konzistentní** bod obnovení virtuálního počítače pro řadič domény provedete testovací převzetí služeb. 
1. Spusťte testovací převzetí služeb při selhání pro plán obnovení, který obsahuje virtuální počítače, aplikace. 
1. Po dokončení testování **vyčistit testovací převzetí služeb při selhání** na virtuálním počítači řadiče domény. Tento krok odstraní řadič domény, který byl vytvořen pro testovací převzetí služeb při selhání.


### <a name="removing-reference-to-other-domain-controllers"></a>Odebrání odkazu do ostatních řadičů domény
Při provádění testovací převzetí služeb, nemusíte přepnutím do všech řadičů domény v testovací síti. Odebrat odkaz na ostatní řadiče domény, které existují v provozním prostředí, budete pravděpodobně potřebovat k [převzetí rolí FSMO služby Active Directory](http://aka.ms/ad_seize_fsmo) a [čištění metadat](https://technet.microsoft.com/library/cc816907.aspx) chybějící řadiče domény. 



> [!IMPORTANT]
> Některé konfigurace popsané v následující části nejsou standard nebo výchozí konfigurace řadiče domény. Pokud nechcete, aby k provedení těchto změn na řadič domény výroby, pak můžete vytvořit řadič domény vyhrazené Site Recovery testovací převzetí služeb při selhání a která tyto změny provést.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Problémy, kvůli ochrana virtualizace 

Od verze Windows Server 2012, [další bezpečnostní opatření se mají sestavily do služby Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Tato bezpečnostní opatření pomoc při ochraně virtualizovaných řadičů domény na vrácení hodnoty USN zpět, tak dlouho, dokud základní platformu hypervisoru VM-GenerationID podporuje. Azure podporuje VM-GenerationID, což znamená, že řadiče domény se systémem Windows Server 2012 nebo novější na Azure virtuální počítače mají další bezpečnostní opatření. 


Při obnovení VM-GenerationID invocationID databázi služby AD DS je také obnovit, se zahodí fond RID a adresáři SYSVOL je označena jako neautoritativní. Další informace najdete v tématu [Úvod do virtualizace služby Active Directory Domain](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) a [bezpečná virtualizace DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Převzetí služeb při selhání do Azure může způsobit, že resetování VM-GenerationID a který se spustí v další bezpečnostní opatření při spuštění virtuálního počítače pro řadič domény v Azure. Může dojít **významné zpoždění** uživatel moct přihlásit k virtuálnímu počítači řadiče domény. Vzhledem k tomu, že tento řadič domény se použije pouze v testovací převzetí služeb, nejsou nezbytná bezpečnostní opatření virtualizace. Aby se zajistilo, že VM-GenerationID virtuálního počítače řadiče domény nezmění, pak je můžete změnit hodnotu DWORD následující na 4 v řadiči domény místní.

        
        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start
 

#### <a name="symptoms-of-virtualization-safeguards"></a>Příznaky ochrana virtualizace
 
Pokud se ochrana virtualizace mít spuštěna po testovací převzetí služeb, mohou se zobrazit nejméně jeden z následujících příznaků:  

Změna ID generování

![Změna ID generování](./media/site-recovery-active-directory/Event2170.png)

Změna ID vyvolání

![Změna ID vyvolání](./media/site-recovery-active-directory/Event1109.png)

Sdílené složky SYSVOL a Netlogon nejsou k dispozici

![Sdílená složka SYSVOL](./media/site-recovery-active-directory/sysvolshare.png)

![NTFRS Sysvol](./media/site-recovery-active-directory/Event13565.png)

Všechny databáze DFSR jsou odstraněny.

![Odstranění databáze DFSR](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Některé konfigurace popsané v následující části nejsou standard nebo výchozí konfigurace řadiče domény. Pokud nechcete, aby k provedení těchto změn na řadič domény výroby, pak můžete vytvořit řadič domény vyhrazené Site Recovery testovací převzetí služeb při selhání a která tyto změny provést.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Řešení problémů řadič domény během testovacího převzetí služeb při selhání


Na příkazovém řádku spusťte následující příkaz a zkontrolujte, zda jsou sdílené složky SYSVOL a NETLOGON:

    NET SHARE

Na příkazovém řádku spusťte následující příkaz a ověřte, že řadič domény správně funguje.

    dcdiag /v > dcdiag.txt

V protokolu výstupu vyhledejte následující text k potvrzení, zda je řadič domény dobře funguje. 

* "předaný test připojení"
* "předaný testovací inzerování"
* "předaný test MachineAccount"

Jestliže výše uvedených podmínek jsou splněny, je pravděpodobné, že řadič domény dobře funguje. Pokud ne, zkuste následující kroky.


* Proveďte autoritativního obnovení řadiče domény.
    * I když je [nedoporučuje se používat FRS replikace](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), ale pokud jste ji stále používá pak postupujte podle kroků uvedených [sem](https://support.microsoft.com/kb/290762) udělat autoritativního obnovení. Můžete si přečíst další informace o Burflags věnovala v předchozí odkaz [zde](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Pokud používáte DFSR replikace, postupujte podle pokynů k dispozici [sem](https://support.microsoft.com/kb/2218556) udělat autoritativního obnovení. Můžete také použít funkce prostředí Powershell k dispozici v tomto [odkaz](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) pro tento účel. 
    
* Nepoužívat počáteční synchronizaci nastavením následující klíč registru na 0 v místní řadič domény. Pokud tato DWORD neexistuje, pak můžete vytvořit ji v uzlu "Parametry". Další informace o něm [sem](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Zakažte požadavek, že je k dispozici pro ověření přihlášení uživatele nastavením následující klíč registru na 1 v řadiči domény místní server globálního katalogu. Pokud tato DWORD neexistuje, můžete ho vytvořit v uzlu 'Lsa'. Další informace o něm [sem](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>DNS a řadič domény na různé počítače
Pokud DNS není ve stejné virtuální počítač jako řadič domény, budete muset vytvořit virtuální počítač DNS pro testovací převzetí služeb. Jestliže se nacházejí ve stejné virtuální počítač, můžete tuto část přeskočit.

Můžete použít nový server DNS a vytvořit všechny požadované zóny. Například pokud vaše doména služby Active Directory je contoso.com, můžete vytvořit zónu DNS s názvem contoso.com. Údaje pro služby Active Directory musí aktualizovat ve službě DNS, následujícím způsobem:

1. Zajistěte, aby že tato nastavení jsou na místě před žádným jiným virtuálním počítačem v plánu obnovení se zobrazí:
   
   * Zóna musí mít název po název kořenové doménové struktury.
   * Zóna musí být záložních souborů.
   * Zóna musí být povoleno pro zabezpečení a nezabezpečené aktualizace.
   * Překladač virtuálního počítače řadiče domény by měla odkazovat na adresu IP virtuálního počítače DNS.
2. Na virtuálním počítači řadiče domény, spusťte následující příkaz:
   
    `nltest /dsregdns`
3. Přidání zóny na serveru DNS, Povolit nezabezpečené aktualizace a přidejte položku DNS pro něj:
   
        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Další kroky
Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) Další informace o ochraně podnikové úlohy s Azure Site Recovery.

