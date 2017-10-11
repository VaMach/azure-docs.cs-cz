---
title: "Selhání zálohování virtuálních počítačů VMware z Azure na portálu classic | Microsoft Docs"
description: "Další informace o selhání zpátky do místní lokality po převzetí služeb při selhání virtuálních počítačů VMware a fyzické servery do Azure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 82d5eb7fd13b1e9700a3e9bc2d30775e9c129749
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site-classic-portal"></a>Selhání back virtuální počítače VMware a fyzické servery k místní lokalitě (portálu classic)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [portál Azure Classic](site-recovery-failback-azure-to-vmware-classic.md)
> * [Portál Azure Classic (zastaralé)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Tento článek popisuje, jak selhání zpět Azure virtuálních počítačů z Azure do místní lokality. Postupujte podle pokynů v tomto článku, až budete připraveni k selhání zpět vaše virtuální počítače VMware nebo Windows nebo Linuxem fyzických serverů po jste při selhání z místní lokality do Azure pomocí této [kurzu](site-recovery-vmware-to-azure-classic.md).

## <a name="overview"></a>Přehled
Tento diagram znázorňuje architekturu navrácení služeb po obnovení pro tento scénář.

Tato architektura používejte, když procesový server je místní a používáte ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Tato architektura použijte, pokud je procesový server na platformě Azure a máte síť VPN nebo připojení typu ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Pokud chcete zobrazit úplný seznam portů a navrácení služeb po obnovení architechture diagramu najdete následující obrázek

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Zde je, jak funguje navrácení služeb po obnovení:

* Poté, co jste při selhání do Azure, můžete se nepodařilo zpět na místní lokalitu v několika fázích:
  * **Fáze 1**: tak, aby jejich spuštění replikovat virtuální počítače VMware spuštěné ve vaší místní lokalitě nastavte znovu virtuální počítače Azure. Povolení nové provedení ochrany přesune virtuální počítač do skupiny ochrany navrácení služeb po obnovení, který byl automaticky vytvořen, když jste původně vytvořili skupinu ochrany převzetí služeb při selhání. Pokud jste přidali skupinu ochrany převzetí služeb při selhání pro plán obnovení pak navrácení služeb po obnovení skupiny ochrany se také automaticky přidají do plánu.  Při opětovné ochrany můžete určit způsob plánujete navrácení služeb po obnovení.
  * **Fáze 2**: po virtuální počítače Azure se replikující se do místního serveru, spusťte selhání selhání zpátky do Azure.
  * **Fáze 3**: po vašich dat se nezdařila zpět, je znovu nastavte ochranu místní virtuální počítače, které při selhání zpátky do tak, aby jejich spuštění replikaci do Azure.


  [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failback/player]

### <a name="failback-to-the-original-or-alternate-location"></a>Navrácení služeb po obnovení do původního nebo alternativního umístění
Pokud jste při selhání zpátky ke stejnému zdroji virtuálních počítačů může selhat, pokud stále existuje místní virtuální počítače VMware. V tomto scénáři se jenom rozdílové změny, bude zpět. Poznámky:

* Pokud při selhání fyzické servery navrácení služeb po obnovení je vždy na nový virtuální počítač VMware.
  * Před selháním fyzický počítač zpět Všimněte si, že
  * Fyzický počítač chráněný vrátí se znovu jako virtuální počítač při převzetí služeb při selhání zpět z Azure do VMware
  * Ujistěte se, že zjistíte alespoň jedno hlavní cílový server společně s nezbytné hostitelů ESX/ESXi, na které budete muset navrácení služeb po obnovení.
* Pokud předáte služby zpět na původní virtuální počítač je nutné provést následující:

  * Pokud je virtuální počítač spravován serverem vCenter hostitele ESX hlavního cíle měli přístup k úložišti dat virtuálních počítačů.
  * Pokud virtuální počítač na hostiteli ESX, ale není spravován nástrojem vCenter musí být pevný disk virtuálního počítače v dostupné úložiště dat MT hostitele.
  * Pokud virtuální počítač na hostiteli ESX a nepoužívá vCenter by měl předtím, než můžete znovu nastavte ochranu dokončit zjišťování o hostiteli ESX MT. To platí, pokud po obnovení zpět fyzických serverů příliš.
  * Další možností (pokud existuje místní počítač) je před provedením navrácení služeb po obnovení odstranit. Potom navrácení služeb po obnovení pak vytvoří nový virtuální počítač na stejném hostiteli jako hostitele ESX hlavní cíl.
* Pokud jste navrácení služeb po obnovení do alternativního umístění data budou obnoveny do stejné úložiště dat a na stejného hostitele ESX, který používá místní hlavní cílový server.

## <a name="prerequisites"></a>Požadavky
* V prostředí VMware budete potřebovat k selhání zálohovat virtuální počítače VMware a fyzické servery. Selhání zpět do fyzického serveru není podporována.
* Chcete-li navrácení služeb po obnovení měli jste vytvořili síť Azure při počátečním nastavení ochrany. Navrácení služeb po obnovení musí připojení VPN nebo ExpressRoute z síť Azure, ve které se nacházejí na místní web virtuální počítače Azure.
* Pokud virtuální počítače, které chcete navrácení služeb po obnovení se spravují přes vCenter server, budete potřebovat zajistit máte potřebná oprávnění pro zjišťování virtuálních počítačů na servery vCenter server. [Další informace](site-recovery-vmware-to-azure-classic.md).
* Pokud jsou k dispozici na virtuálním počítači snímky vytvoření selže. Můžete odstranit snímky nebo disky.
* Předtím, než jste navrácení služeb po obnovení budete potřebovat pro vytvoření počet součástí:
  * **Vytvořte procesový server v Azure**. Toto je virtuální počítač Azure, kterou budete potřebovat k vytvoření a dál běžet během navrácení služeb po obnovení. Počítač můžete odstranit po dokončení navrácení služeb po obnovení.
  * **Vytvořit hlavní cílový server**: hlavní cílový server odesílá a přijímá data navrácení služeb po obnovení. Má server pro správu jste vytvořili místní hlavní cílový server ve výchozím nastavení nainstalovaná. V závislosti na objem přenosu selhání back však může potřebovat vytvořit samostatné hlavní cílový server navrácení služeb po obnovení.
  * Pokud chcete vytvořit další hlavní cílový server se systémem Linux, budete muset nastavit virtuálního počítače s Linuxem před instalací hlavní cílový server, jak je popsáno níže.
* Konfigurační server je místní, jakmile to uděláte navrácení služeb po obnovení. Virtuální počítač musí během navrácení služeb po obnovení, neexistuje v databázi konfigurace serveru, nejsou-li být které navrácení služeb po obnovení nebude úspěšné. Proto zajistěte podniknout pravidelné plánované zálohy vašeho serveru. V případě havárie musíte obnovit pomocí stejné IP adresy, aby navrácení služeb po obnovení budou pracovat.

## <a name="set-up-the-process-server-in-azure"></a>Nastavit procesní server v Azure
Budete muset nainstalovat procesní server v Azure tak, aby virtuální počítače Azure může posílat data zpět na místní hlavní cílový server.

1. Na portálu Site Recovery > **konfigurační servery** vyberte, které chcete přidat nový procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Zadejte název serveru, který proces a zadejte název a heslo, které budete používat pro připojení k virtuálnímu počítači Azure jako správce. V **konfigurační Server** vyberte server pro správu na místě a zadejte síť Azure, ve kterém by měly být nasazeny procesový server. To by měl být sítě, ve které se nacházejí virtuální počítače Azure. Zadejte jedinečnou IP adresu z podsítě, vyberte a zahájit nasazení.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

   Aktivuje se úloha nasazení procesového serveru

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

   Po nasazení procesový server v Azure, můžete se přihlásit pomocí přihlašovacích údajů, které jste zadali. Se spustí při prvním přihlášení v dialogovém okně procesu serveru. Zadejte IP adresu serveru pro správu na místě a jeho heslo. Ponechte výchozí nastavení portu 443. Můžete také ponechat výchozí 9443 port pro replikaci dat pokud konkrétně změnit toto nastavení při nastavování serveru pro místní správu.

   > [!NOTE]
   > Server nebude viditelné v rámci **vlastnosti virtuálního počítače**. Je viditelný v části pouze **servery** kartě v serveru pro správu ke kterému je zaregistrován. Může trvat o 10 až 15 minut u procesového serveru, než se objeví.
   >
   >

## <a name="set-up-the-master-target-server-on-premises"></a>Nastavení hlavního cílového serveru na místní
Hlavní cílový server přijímá data navrácení služeb po obnovení. Hlavní cílový server se automaticky nainstaluje na místní server pro správu, ale pokud po obnovení zpět velké množství dat může být potřeba nastavit další hlavní cílový server. Proveďte to následujícím způsobem:

> [!NOTE]
> Pokud chcete instalovat hlavní cílový server v systému Linux, postupujte podle pokynů v dalším postupu.
>
>

1. Pokud instalujete hlavní cílový server v systému Windows, otevřete stránku rychlý Start z virtuálního počítače, na kterém jste instalaci hlavní cílový server a stáhněte instalační soubor pro Průvodce Unified instalace nástroje Azure Site Recovery.
2. Spusťte instalační program a v **před zahájením** vyberte **přidat další proces servery pro horizontální rozšíření kapacity nasazení**.
3. Stejně jako jste to udělali při dokončení průvodce můžete [nastavení serveru správy](site-recovery-vmware-to-azure-classic.md). Na **podrobnosti o konfiguraci serveru** stránky, zadejte IP adresu tento hlavní cílový server a přístupové heslo pro přístup k virtuálnímu počítači.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Nastavení virtuálního počítače s Linuxem jako hlavní cílový server
Nastavit server pro správu spuštěna hlavní cílový server jako virtuální počítač s Linuxem budete muset nainstalovat centů) 6.6 S minimální operační systém, načtení ID SCSI pro každý pevného disku SCSI, nainstalujte některé další balíčky a vlastní změny.

#### <a name="install-centos-66"></a>Nainstalujte CentOS 6.6
1. Nainstalujte CentOS 6.6 minimální operační systém na serveru pro správu virtuálních počítačů. Ponechte ISO v jednotce DVD a spuštění systému. Přeskočit testování média, vyberte angličtinu na jazyk, vyberte **základní zařízení úložiště**, zkontrolujte, zda pevný disk nemá mít žádné důležitých dat a klikněte na tlačítko **Ano**, zahodit žádná data. Zadejte název hostitele serveru pro správu a vyberte síťový adaptér serveru.  V **úpravy systému** dialogovém okně vyberte ** připojit automaticky, ** a přidejte statickou IP adresu, sítě a nastavení služby DNS. Zadejte časové pásmo a kořenové heslo pro přístup k serveru pro správu.
2. Pokud výzva typ instalace, byste chtěli vyberte **vytvořit vlastní rozložení** jako oddíl. Po kliknutí na tlačítko **Další** vyberte **volné** a klikněte na tlačítko vytvořit. Vytvoření  **/** , **/var/havárií** a **/home oddíly** s **FS typ:** **ext4**. Vytvoření partion odkládacího souboru jako **FS typ: prohození**.
3. Pokud již existující zařízení nalezeny, že zobrazí se zpráva upozornění. Klikněte na tlačítko **formát** k formátování disku s nastavení oddílu. Klikněte na tlačítko **zápisu změn do disku** se použít změny oddílu.
4. Vyberte **instalace spouštěcí zavaděč** > **Další** instalujete spouštěcí zavaděč kořenové oddílu.
5. Po dokončení instalace klikněte na tlačítko **restartovat**.

#### <a name="retrieve-the-scsi-ids"></a>Načtení ID SCSI
1. Po instalaci načíst ID SCSI pro každý SCSI pevný disk ve virtuálním počítači. Pokud chcete provést tento vypnout server pro správu virtuálních počítačů, ve vlastnostech virtuálního počítače v prostředí VMware pravým tlačítkem na položku počítač > **upravit nastavení** > **možnosti**.
2. Vyberte **Upřesnit** > **obecné položky** a klikněte na tlačítko **parametry konfigurace**. Tato možnost bude de-active, když je počítač spuštěn. Aktivujte musí být vypnout počítač.
3. Pokud řádek **disku. EnableUUID** existuje Ujistěte se, že je hodnota nastavená **True** (malá a velká písmena). Pokud už je můžete zrušit a poté, co je při spuštění testů příkaz SCSI uvnitř hostovaného operačního systému.
4. Pokud není existující klikněte na řádek **přidat řádek** – a přidejte ji s **True** hodnotu. Nepoužívejte uvozovky.

#### <a name="install-additional-packages"></a>Nainstalujte další balíčky
Budete muset stáhnout a nainstalovat některé další balíčky.

1. Ujistěte se, že je hlavní cílový server připojený k Internetu.
2. Spusťte tento příkaz ke stažení a instalaci 15 balíčky z úložiště CentOS: **# yum nainstalovat – y xfsprogs perl lsscsi rsync wget kexec nástroje**.
3. Pokud chráníte zdrojový počítač se systémem Linux wit Reiser nebo XFS systém pro kořenovou nebo spouštěcí zařízení, souborů, pak by měla stáhnout a nainstalovat další balíčky následujícím způsobem:

   * # <a name="cd-usrlocal"></a>/usr/local disku CD
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs modulu RPM – ivh kmod reiserfs 0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>ot. / min – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Použít vlastní změny
Proveďte následující použít vlastní změny po jste kroky po instalaci a nainstalovat balíčky:

1. Zkopírujte RHEL 6-64 Unified Agent binární k virtuálnímu počítači. Spusťte tento příkaz untar binárního souboru: **– zxvf funkce vkládání<file name>**
2. Spusťte tento příkaz poskytnout oprávnění: **./ApplyCustomChanges.sh chmod &#755;**
3. Spusťte skript: **#./ApplyCustomChanges.sh**. Skript měli spustit jenom jednou. Po úspěšném spuštění skriptu, restartujte server.

## <a name="run-the-failback"></a>Spustit navrácení služeb po obnovení
### <a name="reprotect-the-azure-vms"></a>Znovu nastavte ochranu virtuálních počítačů Azure
1. Na portálu Site Recovery > **počítače** vyberte virtuální počítač, který při selhání a klikněte na tlačítko **znovu nastavit ochranu**.
2. V **hlavní cílový Server** a **procesový Server** vyberte místní hlavní cílový server a procesový server virtuálního počítače Azure.
3. Vyberte účet, který jste nakonfigurovali pro připojení k virtuálnímu počítači.
4. Vyberte verzi navrácení služeb po obnovení skupiny ochrany. Pro příklad, pokud virtuální počítač je chráněn v so1 pak můžete budete muset vybrat PG1_Failback.
5. Pokud chcete obnovit do alternativního umístění, vyberte jednotka pro uchování a úložiště dat, které jsou nakonfigurované pro hlavní cílový server. Když jste se nepovede zpět do místního webu virtuální počítače VMware v plánu ochrany navrácení služeb po obnovení bude používat stejné úložiště dat jako hlavní cílový server. Pokud chcete obnovit repliku virtuálního počítače Azure na stejný místní virtuální počítač místní virtuální počítač by měl být již v úložišti dat stejné jako hlavní cílový server. Pokud neexistuje žádný virtuální počítač místně, vytvoří se při vytvoření nového.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. Po kliknutí na tlačítko **OK** zahájíte vytvoření úlohy začne replikovat virtuální počítač z Azure do místní lokality. Průběh můžete sledovat na **úlohy** kartě.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Spuštění převzetí služeb při selhání k místní lokalitě
Po vytvoření virtuálního počítače je přesunuta do verze navrácení služeb po obnovení skupiny ochrany a je automaticky přidán do plánu obnovení, které jste použili pro převzetí služeb při selhání do Azure, pokud existuje.

1. V **plány obnovení** vyberte plán obnovení, který obsahuje skupinu navrácení služeb po obnovení a klikněte na tlačítko **převzetí služeb při selhání** > **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání** ověřte směr převzetí služeb při selhání (do Azure) a vyberte bod obnovení, kterou chcete použít převzetí služeb při selhání (nejnovější). Pokud jste povolili **více virtuálních počítačů** při konfiguraci vlastností replikace můžete obnovit nejnovější aplikace nebo bod konzistentní při selhání obnovení. Kliknutím na značku zaškrtnutí zahájíte převzetí služeb při selhání.
3. Site Recovery při převzetí služeb při selhání se zastaví virtuální počítače Azure. Po zkontrolujte, že tento navrácení služeb po obnovení je dokončená, podle očekávání, můžete můžete můžete zkontrolovat, že virtuální počítače Azure mají byla vypnuta podle očekávání.

### <a name="reprotect-the--on-premises-site"></a>Znovu nastavte ochranu místního serveru
Po dokončení navrácení služeb po obnovení dat bude zpět na místní lokality, ale nebudou chráněné. Spustit replikaci do Azure znovu proveďte následující:

1. Na portálu Site Recovery > **počítače** kartě vyberte virtuální počítače, které selhaly zpět a klikněte na tlačítko **znovu nastavit ochranu**.
2. Po ověření, že replikace na Azure funguje podle očekávání, v Azure můžete odstranit virtuální počítače Azure (aktuálně není spuštěn), které byly zpět se nezdařilo.

### <a name="common-issues-in-failback"></a>Běžné problémy při navrácení služeb po obnovení
1. Pokud provádět zjišťování vCenter uživatelů jen pro čtení a chránit virtuální počítače, bude úspěšný a funguje převzetí služeb při selhání. Při opětovné ochrany se nezdaří, vzhledem k tomu, že datastores nemůže být zjištěny. Jako příznakem neuvidíte datastores uvedené při ochranu znovu. To vyřešit, můžete aktualizovat pověření vCenter odpovídajícímu účtu, který má oprávnění a opakujte úlohu. [Další informace](site-recovery-vmware-to-azure-classic.md)
2. Pokud jste navrácení služeb po obnovení virtuálního počítače s Linuxem a potom ho spusťte místní, zobrazí se, že byl správce sítě balíček odinstalován z počítače. Je to proto, že pokud virtuální počítač je obnovena v Azure, odebere se balíček správce sítě.
3. Když virtuální počítač je nakonfigurovaný se statickou IP adresou a při selhání do Azure, je pomocí protokolu DHCP získali IP adresu. Při selhání zpátky do místní virtuální počítač dál pro používání protokolu DHCP získat IP adresu. Musíte ručně přihlášení do počítače a IP adresu zpět na statickou adresu v případě potřeby sady.
4. Pokud používáte edice free ESXi 5.5 nebo edice free Hypervisor vSphere 6, by úspěšné převzetí služeb při selhání, ale navrácení služeb po obnovení nebude úspěšné. Budete ned k upgradu buď zkušební licence Povolit navrácení služeb po obnovení.

## <a name="failing-back-with-expressroute"></a>Chybě zpět s ExpressRoute
Vám může selhat zpět přes připojení VPN nebo Azure ExpressRoute. Pokud chcete použít ExpressRoute vezměte na vědomí následující:

* ExpressRoute měli nastavit na virtuální síť Azure, na zdroj, který se nachází selhání počítače přes a které virtuální počítače Azure, potom, co dojde převzetí služeb při selhání.
* Data se replikují do účtu úložiště Azure na veřejný koncový bod. Měli byste nastavit veřejný partnerský vztah v ExpressRoute s cílového datového centra pro Site Recovery replikaci pomocí ExpressRoute.
