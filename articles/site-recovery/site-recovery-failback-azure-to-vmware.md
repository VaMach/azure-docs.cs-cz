---
title: "Virtuální počítače VMware navrácení služeb po obnovení z Azure do místní | Microsoft Docs"
description: "Další informace o selhání zpátky do místní lokality po převzetí služeb při selhání virtuálních počítačů VMware a fyzické servery do Azure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: dde0bb6b4f6bc10afdd7d40adc6689d42b37de81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Selhání back virtuální počítače VMware a fyzické servery k místní lokalitě


Tento článek popisuje, jak Azure navrácení služeb po obnovení virtuálních počítačů z Azure do místní lokality. Postupujte podle pokynů v tomto poli, když jste připraveni navrácení služeb po obnovení virtuálních počítačů VMware nebo fyzických serverů systému Windows nebo Linux po můžete znovu chrání vaše počítače pomocí této [odkaz](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Pokud používáte portál Azure classic, prosím postupovat podle pokynů uvedených [sem](site-recovery-failback-azure-to-vmware-classic.md) pro rozšířené VMware do Azure architektury a [sem](site-recovery-failback-azure-to-vmware-classic-legacy.md) pro starší verze architektury

## <a name="overview"></a>Přehled
V této části diagramech architektuře navrácení služeb po obnovení pro tento scénář.

Když procesový Server je místní a používáte připojení k Azure ExpressRoute, použijte tato architektura:

![Diagram architektury pro ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Pokud je procesový Server na platformě Azure a máte síť VPN nebo připojení ExpressRoute, použijte tato architektura:

![Diagram architektury pro síť VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Úplný seznam portů a diagram architektury navrácení služeb po obnovení naleznete na následujícím obrázku:

![Převzetí služeb při selhání-navrácení služeb po obnovení seznamu všechny porty](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Poté, co jste při selhání do Azure, můžete se nepodařilo zpět na místní lokalitu ve třech fázích:

* **Fáze 1**: tak, aby jejich spuštění replikovat virtuální počítače VMware, na místní lokalitu nastavte znovu virtuální počítače Azure.
* **Fáze 2**: po virtuální počítače Azure se replikují do místního serveru, můžete spustit převzetí služeb při selhání zpátky do Azure.
* **Fáze 3**: po vašich dat se nezdařila zpět, je znovu nastavte ochranu místní virtuální počítače, které při selhání zpátky do tak, aby jejich spuštění replikaci do Azure.

### <a name="fail-back-to-the-original-location-or-an-alternate-location"></a>Navrácení služeb po obnovení do původního umístění nebo alternativního umístění
Po selhání virtuální počítače VMware můžete Pokud můžou selhat zpět do stejné zdrojový virtuální počítač stále existuje místní. V tomto scénáři pouze rozdílů došlo k selhání zpět.

Pokud jste při selhání fyzické servery, navrácení služeb po obnovení je vždy na nový virtuální počítač VMware. Před selháním zpět fyzického počítače, Všimněte si, že:
* Chráněné fyzický počítač, vrátí se znovu jako virtuální počítač, když přes zpět z Azure se nezdařila VMware. Fyzický počítač systému Windows Server 2008 R2 SP1, pokud je chráněný a převzetí služeb při selhání do Azure, nemůže se zpět. Windows Server 2008 R2 SP1 počítač, který spuštěna jako virtuální počítač na místě, bude moct navrácení služeb po obnovení.
* Ujistěte se, že zjistíte alespoň jeden hlavní cílový server společně s nezbytné hostitelů ESX/ESXi, které potřebujete k navrácení služeb po obnovení.

Pokud tak zpět na původní virtuální počítač, je potřeba splnit následující:

* Pokud virtuální počítač spravuje vCenter server, hostitele ESX se hlavní cíl měli přístup k úložišti dat virtuálních počítačů.
* Pokud virtuální počítač na hostiteli ESX, ale není spravován nástrojem vCenter, musí být pevný disk virtuálního počítače v úložišti dat, která je přístupná pomocí MT hostitele.
* Pokud virtuální počítač je na hostiteli ESX a nepoužívá vCenter, by měl dokončit zjišťování o hostiteli ESX MT, než můžete znovu nastavte ochranu. To platí, pokud po obnovení zpět fyzických serverů příliš.
* Další možností (pokud existuje místní počítač) je před provedením navrácení služeb po obnovení odstranit. Navrácení služeb po obnovení pak vytvoří nový virtuální počítač na stejném hostiteli jako hostitele ESX hlavní cíl.

Pokud žádnou zpět do alternativního umístění, data je obnovena na stejné úložiště dat a na stejného hostitele ESX, který používá místní hlavní cílový server.

## <a name="prerequisites"></a>Požadavky
* K selhání zálohovat virtuální počítače VMware a fyzické servery, budete potřebovat prostředí VMware. Selhání zpět do fyzického serveru není podporována.
* K navrácení služeb po obnovení, musí jste vytvořili síť Azure při počátečním nastavení ochrany. Navrácení služeb po obnovení musí připojení VPN nebo ExpressRoute z síť Azure, ve které se nacházejí na místní web virtuální počítače Azure.
* Pokud virtuální počítače, které chcete navrácení služeb po obnovení se spravují přes vCenter server, ujistěte se, že máte potřebná oprávnění pro zjišťování virtuálních počítačů na servery vCenter server. Další informace najdete v tématu [VMware replikaci virtuálních počítačů a fyzických serverů do Azure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
* Pokud snímky jsou v něm na virtuálním počítači, nové provedení ochrany se nezdaří. Můžete odstranit snímky nebo disky.
* Předtím, než jste navrácení služeb po obnovení, vytvořte tyto komponenty:
  * **Vytvořte procesový Server v Azure**. Tato součást je virtuální počítač Azure, které vytvoříte a dál běžet během navrácení služeb po obnovení. Virtuální počítač můžete odstranit po dokončení navrácení služeb po obnovení.
  * **Vytvořit hlavní cílový server**: hlavní cílový server odesílá a přijímá data navrácení služeb po obnovení. Hlavní cílový server, který je nainstalován ve výchozím nastavení má server pro správu, že jste vytvořili místní. V závislosti na objem provozu zpět se nezdařilo, však může potřebovat vytvořit samostatné hlavní cílový server navrácení služeb po obnovení.
  * Pokud chcete vytvořit další hlavní cílový server, který běží na systému Linux, nastavení virtuálního počítače s Linuxem před instalací hlavní cílový server, jak je popsáno dále.
* Konfigurační server je místní, jakmile to uděláte navrácení služeb po obnovení. Virtuální počítač musí během navrácení služeb po obnovení, neexistuje v databázi konfigurací serveru. Pokud konfigurační databáze serveru obsahuje žádné virtuálních počítačů, nelze úspěšně dokončit navrácení služeb po obnovení. Proto zajistěte, abyste vytvořili pravidelných naplánovaných záloh serveru. Po havárii je nutné obnovit se stejnou adresou IP, který navrácení služeb po obnovení fungovat.
* Nastavení disk.enableUUID=true **parametry konfigurace** hlavního cíle virtuálních počítačů v prostředí VMware. Pokud tento řádek neexistuje, přidejte ji. Toto nastavení je nutné k zajištění konzistentní univerzálně jedinečný identifikátor (UUID) do souboru na disku (VMDK) virtuálního počítače tak, aby se správně připojené.
* Mějte "hlavní cílový server nemůže být úložiště vMotioned" stav, který může způsobit navrácení služeb po obnovení selhání. Virtuální počítač nelze vyvolat, protože nejsou k dispozici disky.
* Přidáním jednotky, názvem na uchování jednotku, na hlavním cílovém serveru. Přidejte disk a formátování disku.

## <a name="failback-policy"></a>Navrácení služeb po obnovení zásad
K replikaci zpět na místní, musíte zásadu navrácení služeb po obnovení. Zásady se automaticky vytvoří, když vytvoříte zásadu, předat dál směr, který je automaticky spojeny s konfiguračním serverem. Se nedá upravovat. Zásady má následující nastavení replikace:

* Prahová hodnota plánovaného bodu obnovení = 15 minut
* Uchování bodu obnovení = 24 hodin
* Frekvence snímků konzistentní vzhledem = 60 minut

 ![Nastavení replikace zásad navrácení služeb po obnovení](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Nastavit procesní Server v Azure
Nainstalujte procesní Server v Azure tak, aby virtuální počítače Azure může posílat data zpět na místní hlavní cílový server.

Pokud chráníte virtuální počítače jako klasické prostředky (virtuální počítač obnovit v Azure je virtuální počítač, který byl vytvořen pomocí modelu nasazení classic), budete potřebovat procesový Server v Azure. Pokud jste obnovili virtuálních počítačů pomocí Azure Resource Manageru jako typ nasazení, procesový Server musí mít správce prostředků jako typ nasazení. Typ nasazení je vybrána jako virtuální síť Azure, můžete nasadit procesový Server na.

1. V **trezoru** > **nastavení** > **infrastruktura Site Recovery** (v části **spravovat**) > **konfigurační servery** (v části **pro VMware a fyzické počítače**), vyberte konfigurační server.
2. Klikněte na tlačítko **zpracovat serveru**.

  ![Proces serveru stisknutím tlačítka](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Zvolit pro nasazení procesový Server jako **nasadit procesový Server v Azure, navrácení služeb po obnovení**.
4. Vyberte předplatné, které jste obnovili virtuálních počítačů do.
5. Vyberte síť Azure, kterou jste obnovili virtuálních počítačů do. Procesový Server musí být ve stejné síti, aby mohl komunikovat obnovené virtuálních počítačů a procesového serveru.
6. Pokud jste vybrali *modelu nasazení classic* sítě, vytvoření virtuálního počítače prostřednictvím Azure Marketplace a poté nainstalujte procesový Server v něm.

 ![Okno "Přidat Server proces"](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 Jak vytváříte procesový Server, věnujte pozornost následujícím:
 * Název bitové kopie je *Microsoft Azure Site obnovení proces serveru V2*. Vyberte **Classic** jako model nasazení.

       ![Select "Classic" as the Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Instalace serveru proces podle pokynů v [VMware replikaci virtuálních počítačů a fyzických serverů do Azure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
7. Pokud jste vybrali *Resource Manager* Azure síť, nasazení procesového serveru tím, že poskytuje následující informace:

  * Název skupiny prostředků, kterou chcete nasadit server
  * Název serveru
  * Uživatelské jméno a heslo pro přihlášení k serveru
  * Účet úložiště, který chcete nasadit server
  * Podsíť a síťové rozhraní, které se chcete připojit k němu
   >[!NOTE]
   >Musíte vytvořit vlastní [síťové rozhraní](../virtual-network/virtual-networks-multiple-nics.md) (NIC) a vyberte ho při nasazování procesový Server.

    ![Zadejte informace v dialogovém okně "Přidat Server proces"](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Klikněte na **OK**. Tato akce spustí úlohu, která vytvoří virtuální počítač typu nasazení Správce prostředků během instalace procesový Server. Chcete-li zaregistrovat server na konfiguračním serveru, spusťte instalaci ve virtuálním počítači podle pokynů v [VMware replikaci virtuálních počítačů a fyzických serverů do Azure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md). Také se aktivuje úlohy nasazení procesového serveru.

  Procesový Server je uveden na **konfigurační servery** > **přidružené servery** > **proces servery** kartě.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > Procesový Server není viditelné v rámci **vlastnosti virtuálního počítače**. Je zobrazeno pouze na **servery** kartě v serveru pro správu, které je registrované k. Může trvat 10 až 15 minut u procesového serveru, než se objeví.


## <a name="set-up-the-master-target-server-on-premises"></a>Nastavení hlavního cílového serveru na místní
Hlavní cílový server přijímá data navrácení služeb po obnovení. Server se automaticky nainstaluje na místní server pro správu, ale pokud po obnovení zpět příliš velké množství dat, možná budete muset zřídit další hlavní cílový server. Nastavit hlavní cílový server místně, postupujte takto:

> [!NOTE]
> Pokud chcete nastavit hlavní cílový server v systému Linux, přejděte k dalšímu postupu. Použijte pouze CentOS 6.6 minimální operační systém jako hlavní cílový operační systém.

1. Pokud nastavujete hlavní cílový server v systému Windows, otevřete úvodní stránku z virtuálního počítače, který instalujete na hlavním cílovém serveru.
2. Stáhněte instalační soubor pro Průvodce Unified instalace nástroje Azure Site Recovery.
3. Spusťte instalační program a v **před zahájením**, vyberte **přidat další proces servery pro horizontální rozšíření kapacity nasazení**.
4. Stejně jako jste to udělali při dokončení průvodce můžete [nastavení serveru správy](site-recovery-vmware-to-azure-classic.md). Na **podrobnosti o konfiguraci serveru** stránky, zadejte IP adresu hlavního cílového serveru a zadejte přístupové heslo pro přístup k virtuálnímu počítači.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Nastavení virtuálního počítače s Linuxem jako hlavní cílový server
Pokud chcete nastavit server pro správu spuštěna hlavní cílový server jako virtuální počítač s Linuxem, nainstalujte CentOS 6.6 minimální operační systém. V dalším kroku načíst ID SCSI pro každý SCSI pevný disk, nainstalujte některé další balíčky a vlastní změny.

#### <a name="install-centos-66"></a>Nainstalujte CentOS 6.6

1. Nainstalujte CentOS 6.6 minimální operační systém na serveru pro správu virtuálních počítačů. Zachovat ISO v jednotce DVD a spusťte systém. Přeskočte testování média. Vyberte **angličtinu** jako jazyk, vyberte **základní zařízení úložiště**, zkontrolujte, že pevný disk nemá žádné důležitých dat, klikněte na tlačítko **Ano**a zrušení žádná data. Zadejte název hostitele serveru pro správu a vyberte síťový adaptér serveru.  V **úpravy systému** dialogové okno, vyberte **připojit automaticky,**a poté přidejte statickou IP adresu, sítě a nastavení služby DNS. Zadejte časové pásmo. Pro přístup k serveru pro správu, zadejte kořenové heslo.
2. Jakmile se zobrazí výzva, jaký typ instalace chcete, vyberte **vytvořit vlastní rozložení** jako oddíl. Klikněte na **Další**. Vyberte **volné**a potom klikněte na **vytvořit**. Vytvoření  **/** , **/var/havárií**, a **/home oddíly** s **FS typ:** **ext4**. Vytvořit oddíl odkládacího souboru jako **FS typ: prohození**.
3. Pokud se najde existující zařízení, zobrazí se zpráva s upozorněním. Klikněte na tlačítko **formát** k formátování disku s nastavení oddílu. Klikněte na tlačítko **zápisu změn do disku** se použít změny oddílu.
4. Vyberte **instalace spouštěcí zavaděč** > **Další** instalujete spouštěcí zavaděč kořenové oddílu.
5. Po dokončení instalace klikněte na tlačítko **restartovat**.

#### <a name="retrieve-the-scsi-ids"></a>Načtení ID SCSI

1. Po instalaci načíst ID SCSI pro každý SCSI pevný disk ve virtuálním počítači. Uděláte to tak, vypněte server pro správu virtuálních počítačů. Ve vlastnostech virtuálního počítače v prostředí VMware, pravým tlačítkem na položku počítač > **upravit nastavení** > **možnosti**.
2. Vyberte **Upřesnit** > **obecné položky**a potom klikněte na **parametry konfigurace**. Tato možnost není dostupná, když je počítač spuštěn. Pro možnost je k dispozici musí být vypnout počítač.
3. Proveďte jednu z následujících akcí:
 * Pokud řádek **disku. EnableUUID** existuje, ujistěte se, že je hodnota nastavena **True** (malá a velká písmena). Pokud hodnota je již nastaven na hodnotu True, můžete zrušit a poté, co je při spuštění testů příkaz SCSI uvnitř hostovaného operačního systému.
 * Pokud řádek **disku. EnableUUID** neexistuje, klikněte na tlačítko **přidat řádek**a poté přidejte její **True** hodnotu. Nepoužívejte dvojitých uvozovek.

#### <a name="install-additional-packages"></a>Nainstalujte další balíčky
Stáhněte a nainstalujte další balíčky.

1. Ujistěte se, že je hlavní cílový server připojený k Internetu.
2. Chcete-li stáhnout a nainstalovat balíčky 15 z úložiště CentOS, spusťte tento příkaz: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Pokud chráníte zdrojový počítač se systémem Linux s Reiser nebo XFS systému pro zařízení root nebo spouštění souborů, stáhněte a nainstalujte další balíčky následujícím způsobem:

   * \#/usr/local disku CD
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#reiserfs modulu RPM – ivh kmod reiserfs 0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#ot. / min – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \#YUM instalaci zařízení mapper-multipath (nutné k povolení funkce multipath balíčků na hlavním cílovém serveru)

#### <a name="apply-custom-changes"></a>Použít vlastní změny
Po dokončení kroků po instalaci a nainstalované balíčky, použít vlastní změny následujícím způsobem:

1. Zkopírujte RHEL 6-64 Unified Agent binární k virtuálnímu počítači. Chcete-li untar binárního souboru, spusťte tento příkaz: `tar –zxvf <file name>`.
2. Chcete-li udělit oprávnění, spusťte tento příkaz: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Spusťte následující skript: `# ./ApplyCustomChanges.sh`. Spusťte jenom jednou. Po úspěšném spuštění, restartujte server.

## <a name="run-the-failback"></a>Spustit navrácení služeb po obnovení
### <a name="reprotect-the-azure-vms"></a>Znovu nastavte ochranu virtuálních počítačů Azure
1. V **trezoru**v **replikované položky**, klikněte pravým tlačítkem na virtuální počítač, který se po selhání a pak vyberte **znovu nastavit ochranu**.
2. V okně můžete uvidíte, že směr ochrany **Azure k místnímu** je již vybrána.
3. V **hlavní cílový Server** a **procesový Server**, vyberte místní hlavní cílový server a procesový Server virtuálního počítače Azure.
4. Vyberte úložiště dat, které chcete obnovit disky místní do. Tuto možnost použijte, když se odstraní místní virtuální počítač a budete muset vytvořit nové disky. Možnost ignorujte, pokud disky již existují, ale stále je třeba zadat hodnotu.
5. Zastavit body v čase, když je virtuální počítač replikovat zpět na místní pomocí jednotka pro uchování. Jsou zde uvedeny některé kritéria jednotka pro uchování. Bez těchto kritérií jednotka není uvedena v seznamu pro hlavní cílový server.

  * Svazek by neměl být používán k žádnému jinému účelu (Cílem replikace a tak dále).
  * Svazek by neměl být v režimu zámku.
  * Svazek by neměl být svazkem mezipaměti. (Instalace hlavního cíle nesmí existovat na tomto svazku. Procesový Server a hlavní cíl vlastní instalaci svazku nejsou vhodné pro svazek pro uchovávání. Zde nainstalované procesový Server a hlavní cíl svazek je svazek mezipaměti hlavního cíle.)
  * Typ systému souborů svazku by neměl být FAT a FAT32.
  * Kapacita svazku by neměla být nulová.
  * Svazek pro uchovávání dat výchozí pro Windows je R svazku.
  * Výchozí svazek pro uchovávání dat pro Linux je /mnt/retention.

6. Zásady navrácení služeb po obnovení je automaticky vybrán.
7. Po kliknutí na tlačítko **OK** zahájíte vytvoření úlohy začne replikovat virtuální počítač z Azure do místní lokality. Průběh můžete sledovat na **úlohy** kartě.

Pokud chcete obnovit do alternativního umístění, vyberte jednotka pro uchování a úložiště dat, které jsou nakonfigurované pro hlavní cílový server. Pokud žádnou zpět na místní lokalitu, virtuální počítače VMware v plánu ochrany navrácení služeb po obnovení použití stejné úložiště jako hlavní cílový server. Pokud chcete obnovit virtuální počítač Azure na stejný místní virtuální počítač repliky, místní virtuální počítač by měl být již v úložišti dat stejné jako hlavní cílový server. Pokud žádný virtuální počítač na místě, vytvoří se během vytvoření novou.

![Vyberte "Azure k místnímu" v rozevírací nabídce](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Znovu nastavte ochranu můžete také na úrovni plánu obnovení. Pokud máte skupinu replikace, můžete jen pomocí plán obnovení nastavte ji znovu. Pokud jste znovu nastavte ochranu pomocí plán obnovení, pro každý chráněný počítač použijte předchozí hodnoty.

> [!NOTE]
> Zpět s stejný hlavní cíl by měly být chráněné replikační skupiny. Pokud jsou chráněny zpět k jiným hlavním cílovým serverům, společný bod v čase nelze určit pro ně.

### <a name="run-a-failover-to-the-on-premises-site"></a>Spuštění převzetí služeb při selhání k místní lokalitě
Po znovu nastavte ochranu virtuálního počítače, můžete spustit převzetí služeb při selhání z Azure do místní.

1. Na stránce replikované položky, klikněte pravým tlačítkem na virtuální počítač a potom vyberte **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání**, ověřte směr převzetí služeb při selhání (z Azure) a pak vyberte bod obnovení, který chcete používat pro převzetí služeb při selhání (nejnovější nebo bod nejnovější konzistentní obnovení). Bod obnovení konzistentních s aplikací dojde před nejnovější bod v čase a může to způsobit ztrátu dat.
3. Během převzetí služeb při selhání Site Recovery vypne virtuální počítače Azure. Po můžete zkontrolovat, že tento navrácení služeb po obnovení se dokončila podle očekávání, můžete zkontrolovat, ujistěte se, že virtuální počítače Azure mají byla vypnuta podle očekávání.

### <a name="reprotect-the-on-premises-site"></a>Znovu nastavte ochranu místního serveru
Po dokončení navrácení služeb po obnovení potvrďte tento virtuální počítač k zajištění, že se odstraní virtuální počítače v Azure. Uděláte to tak, klikněte pravým tlačítkem na chráněné položky a pak klikněte na tlačítko **potvrdit**. Tato akce spustí úlohu, která odebere bývalé obnovené virtuální počítače v Azure.

Po dokončení potvrzení data by měla být zpět na místní lokality, ale nebudou chráněné. Pokud chcete spustit znovu replikaci do Azure, postupujte takto:

1. V **trezoru**v **nastavení** > **replikované položky**, vyberte virtuální počítače, které selhaly zpět a potom klikněte na **znovu nastavit ochranu**.
2. Zadejte hodnotu procesového serveru, které se musí použít k odesílání dat zpět do Azure.
3. Klikněte na **OK**.

Po dokončení vytvoření replikuje virtuální počítač zpět do Azure a můžete provést převzetí služeb při selhání.

### <a name="resolve-common-failback-issues"></a>Řešení běžných potíží navrácení služeb po obnovení
* Pokud provádět zjišťování vCenter uživatele jen pro čtení a chránit virtuální počítače, bude úspěšný a funguje převzetí služeb při selhání. Při vytvoření převzetí služeb při selhání se nezdaří, protože datastores nemůže být zjištěny. Jako příznakem neuvidíte datastores uvedené během vytvoření. Chcete-li vyřešit tento problém, můžete aktualizovat pověření vCenter odpovídajícímu účtu, který má oprávnění a opakujte úlohu. Další informace najdete v tématu [VMware replikaci virtuálních počítačů a fyzických serverů do Azure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md)
* Při navrácení služeb po obnovení virtuálního počítače s Linuxem a spusťte ho místně, uvidíte, že byl balíček správce sítě odinstalován z počítače. Tato odinstalace se stane, protože balíček správce sítě odebrán, pokud je virtuální počítač obnovit v Azure.
* Když virtuální počítač je nakonfigurovaný se statickou IP adresou a při selhání do Azure, je pomocí protokolu DHCP získali IP adresu. Při selhání zpátky do místního virtuálního počítače budou nadále používat DHCP získat IP adresu. Ručně přihlaste k počítači a nastavit IP adresu zpět na statickou adresu v případě potřeby.
* Pokud používáte edice free ESXi 5.5 nebo edice free Hypervisor vSphere 6, by úspěšné převzetí služeb při selhání, ale navrácení služeb po obnovení nebude úspěšné. Chcete-li povolit navrácení služeb po obnovení, upgradujte na buď program zkušební licence.
* Pokud nelze kontaktovat konfigurační server z procesového serveru, zkontrolujte připojení k serveru configuration podle Telnet do počítače serveru konfigurace na portu 443. Můžete také zkusit na příkaz ping konfigurační server z počítače procesový Server. Procesový Server musí být také prezenční signál při připojení k serveru configuration.
* Pokud se pokoušíte navrácení služeb po obnovení alternativní vCenter, ujistěte se, že vaše nové vCenter zjišťuje a že je hlavní cílový server také zjišťuje. Typické symptomem je, že datastores nejsou přístupné nebo viditelné v **znovu nastavte ochranu** dialogové okno.
* WS2008R2SP1 počítač, který je chráněn jako fyzický na místním počítači nelze zpět z Azure se místně.

## <a name="fail-back-with-expressroute"></a>Neúspěšné a zobrazí zpět se ExpressRoute
Může selhat zpět prostřednictvím připojení VPN nebo pomocí připojení ExpressRoute. Pokud chcete použít připojení ExpressRoute, vezměte na vědomí následující:

* Připojení ExpressRoute měli nastavit na virtuální síť Azure, zdrojový počítač převzetí služeb při selhání a kde jsou virtuální počítače Azure umístěné po převzetí služeb při selhání.
* Data se replikují do účtu úložiště Azure na veřejný koncový bod. Použití připojení ExpressRoute, nastavte veřejný partnerský vztah v ExpressRoute s cílového datového centra pro replikaci Site Recovery.
