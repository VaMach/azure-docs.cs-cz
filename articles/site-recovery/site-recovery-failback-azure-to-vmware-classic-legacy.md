---
title: "Selhání zálohování virtuálních počítačů VMware z Azure na portálu classic starší verze | Microsoft Docs"
description: "Tento článek popisuje postup navrácení služeb po obnovení virtuálního počítače VMware, který je replikována do Azure s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 3053fc622c6343898e2007b8aaafbe1fa8e6934e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Selhání back virtuální počítače VMware a fyzické servery z Azure do VMware s Azure Site Recovery (zastaralé)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [portál Azure Classic](site-recovery-failback-azure-to-vmware-classic.md)
> * [Portál Azure Classic (zastaralé)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Tento článek popisuje, jak selhání back virtuální počítače VMware a Windows nebo Linuxem fyzických serverů z Azure na místní lokality poté, co jste nastavili replikaci z místního serveru do Azure pomocí [Azure Site Recovery?](site-recovery-overview.md).

Tento článek popisuje původní konfiguraci a by nemělo být použito pro nové trezory.

## <a name="architecture"></a>Architektura
Tento diagram představuje scénář převzetí služeb při selhání a navrácení služeb po obnovení. Modré řádky představují připojení používá během převzetí služeb při selhání. Red řádky představují připojení používá během navrácení služeb po obnovení. Řádky se šipkami přejděte přes internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Než začnete
* Můžete by měl mít převzetí služeb při selhání virtuálních počítačů VMware nebo fyzických serverů a by měly být spuštěny v Azure.
* Všimněte si, že můžete pouze nezdaří back virtuální počítače VMware a fyzické servery Windows nebo Linuxem z Azure do virtuálních počítačů VMware v primární lokalitě místní.  Pokud po obnovení zpět fyzického počítače, převzetí služeb při selhání do Azure ho převést na virtuální počítač Azure a navrácení služeb po obnovení do VMware ho převést na virtuální počítače VMware.

Zde je, jak nastavit navrácení služeb po obnovení:

1. **Nastavit komponenty navrácení služeb po obnovení**: budete muset nastavit vContinuum server na místních počítačích a nasměrujete ho na konfiguračním serveru virtuálního počítače v Azure. Budete také nastavíte procesový server jako virtuální počítač Azure k odesílání dat zpět na místní hlavní cílový server. Zaregistrujte procesový server s konfiguračním serverem, který zpracovává převzetí služeb při selhání. Můžete nainstalovat místní hlavní cílový server. Pokud potřebujete hlavní cílový server systému Windows je nastavení automaticky při instalaci vContinuum. Pokud potřebujete Linux bude nutné jej nastavit ručně na samostatný server.
2. **Povolení ochrany a navrácení služeb po obnovení**: po nastavili jste si komponenty, v vContinuum budete muset povolit ochranu pro přes virtuální počítače Azure se nezdařilo. Můžete spustit kontrolu připravenosti na virtuálních počítačích a spustit převzetí služeb při selhání z Azure do místního serveru. Po dokončení navrácení služeb po obnovení je znovu nastavte ochranu místní počítače tak, aby jejich spuštění replikaci do Azure.

## <a name="step-1-install-vcontinuum-on-premises"></a>Krok 1: Instalace vContinuum na místě
Budete muset nainstalovat vContinuum server místně a nasměrovat ho na konfiguračním serveru.

1. [Stáhněte si vContinuum](http://go.microsoft.com/fwlink/?linkid=526305).
2. Pak stáhnout [vContinuum aktualizace](http://go.microsoft.com/fwlink/?LinkID=533813) verze.
3. Nainstalujte nejnovější verzi vContinuum. Na **úvodní** klikněte na stránce **Další**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Na první stránce průvodce zadejte IP adresu serveru CX a port serveru CX. Vyberte **používat protokol HTTPS**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Najít konfigurační server na IP adresu **řídicí panel** kartě serveru konfigurace virtuálního počítače v Azure.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Najít konfigurační server HTTPS veřejný port na **koncové body** karta Konfigurace serveru virtuálního počítače v Azure.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. Na **CS přístupové heslo podrobnosti** stránky zadejte heslo, které jste si poznamenali dolů při registraci konfigurační server. Pokud si nepamatujete ho zkontrolujte v **C:\\Program Files (x86)\\InMage systémy\\privátní\\connection.passphrase** na konfiguračním serveru virtuálního počítače.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. V **vyberte cílové umístění** stránky zadejte, kam chcete nainstalovat vContinuum server a klikněte na tlačítko **nainstalovat**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. Po dokončení instalace můžete spustit vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>Krok 2: Instalace procesový server v Azure
Budete muset nainstalovat procesní server v Azure tak, aby virtuální počítače v Azure může posílat data zpět do místní hlavní cílový server.

1. Na **konfigurační servery** v Azure vyberte, chcete-li přidat nový procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Zadejte server proces název a název a heslo pro připojení k virtuálnímu počítači jako správce. Vyberte konfigurační server, na který chcete zaregistrovat procesový server. To by měl být stejný server, kterou používáte k ochraně a selhání služeb virtuálních počítačů.
3. Zadejte síť Azure, ve kterém by měly být nasazeny procesový server. By mělo být stejné síti jako konfigurační server. Zadejte jedinečnou IP adresu vybranou podsíť a zahájit nasazení.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. Úlohu se aktivuje nasadit procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. Po nasazení procesový server v Azure může přihlásit k serveru pomocí přihlašovacích údajů, které jste zadali. Zaregistrujte procesový server stejným způsobem, jakým jste zaregistrovali na místním serveru procesu.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> Zaregistrovaných během navrácení služeb po obnovení není k dispozici v části Vlastnosti virtuálního počítače ve službě Site Recovery. Jsou pouze viditelné v rámci **servery** kartě konfigurační server, na které jste registrované. Může trvat asi 10 až 15 minut až poté, co, které procesový server se zobrazí na kartě.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>Krok 3: Instalace místní hlavní cílový server
V závislosti na operačním systému zdrojového virtuálního počítače je potřeba nainstalovat systémem Linux hlavní cílový server Windows-místním nebo.

### <a name="deploy-a-windows-master-target-server"></a>Nasazení systému Windows hlavní cílový server
Hlavní cíl Windows je již součástí vContinuum instalační program. Při instalaci vContinuum hlavní server je také nasazené na stejném počítači a zaregistrovaný na konfiguračním serveru.

1. Chcete-li zahájit nasazení, vytvořit prázdnou počítač místně na hostiteli ESX, na který chcete obnovit virtuální počítače z Azure.
2. Zajistěte, aby aspoň dva disky připojené k virtuálnímu počítači – jedna se používá pro operační systém a dalších se používá pro jednotka pro uchování.
3. Instalace operačního systému.
4. Nainstalujte vContinuum na serveru. Tím se také dokončí instalaci hlavní cílový server.

### <a name="deploy-a-linux-master-target-server"></a>Nasazení hlavní cílový server Linux
1. Chcete-li zahájit nasazení, vytvořit prázdnou počítač místně na hostiteli ESX, na který chcete obnovit virtuální počítače z Azure.
2. Zajistěte, aby aspoň dva disky připojené k virtuálnímu počítači – jedna se používá pro operační systém a dalších se používá pro jednotka pro uchování.
3. Instalace operačního systému Linux. Hlavní cílový systém Linux neměli používat LVM pro kořenovou nebo uchování prostory úložiště. Hlavní cílový server Linux je ve výchozím nastavení nakonfigurované předejdete LVM oddíly nebo disky zjišťování.
4. Oddíly, které můžete vytvořit:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Provádění níže post instalační kroky před zahájením instalace hlavní cíl.

#### <a name="post-os-installation-steps"></a>POST kroky instalace operačního systému
Chcete-li získat ID SCSI pro každý z pevného disku SCSI na virtuálním počítači Linux, povolte parametr "disk. EnableUUID = TRUE "následujícím způsobem:

1. Vypněte virtuální počítač.
2. Pravým tlačítkem na položku virtuálních počítačů v levém panelu > **upravit nastavení**.
3. Klikněte **možnosti** kartě. Vyberte **Upřesnit\>obecné položky** > **parametry konfigurace**. **Parametry konfigurace** možnost je dostupná, jenom když počítač je vypnutý.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Zkontroluje, jestli řádek s **disku. EnableUUID** existuje. Pokud nemá a je nastaven na **False** nastavte ji na **True** (ne velká a malá písmena). Pokud existuje a je nastaven na hodnotu true, klikněte na tlačítko **zrušit** a testování příkaz SCSI uvnitř hostovaného operačního systému, jakmile se spustí až. Pokud neexistuje, klikněte na tlačítko **přidat řádek**.
5. Přidáte disk. EnableUUID v **název** sloupce. Jeho hodnotu nastavte jako TRUE. Nepřidáte výše uvedené hodnoty společně s uvozovky.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Stáhněte a nainstalujte další balíčky
Poznámka: Ujistěte se, že systém má připojení k Internetu před stahování a instalace dalších balíčků.

\#YUM nainstalovat -y xfsprogs perl lsscsi rsync wget kexec nástroje

Tento příkaz stáhne tyto 15 balíčky z úložiště CentOS 6.6 a nainstaluje je:

BC. 1.06.95 1.el6.x86\_64. ot. / min

busybox. 1.15.1 20.el6.x86\_64. ot. / min

elfutils knihovny 0.158 3.2.el6.x86\_64. ot. / min

kexec nástroje 2.0.0 280.el6.x86\_64. ot. / min

lsscsi. 0.23 2.el6.x86\_64. ot. / min

lzo. 2.03 3.1.el6\_5.1.x86\_64. ot. / min

perlu. 5.10.1 136.el6\_6.1.x86\_64. ot. / min

Perl-Module-modulární-3.90-136.el6\_6.1.x86\_64. ot. / min

Perl-Pod – řídicí sekvence-1.04-136.el6\_6.1.x86\_64. ot. / min

Perl-Pod-jednoduché-3.13-136.el6\_6.1.x86\_64. ot. / min

Perl knihovny 5.10.1 136.el6\_6.1.x86\_64. ot. / min

Perl verze 0,77 136.el6\_6.1.x86\_64. ot. / min

RSync. 3.0.6 12.el6.x86\_64. ot. / min

Tenhle 1.1.0 1.el6.x86\_64. ot. / min

wget. 1.12 5.el6\_6.1.x86\_64. ot. / min

Pokud zdrojový počítač používá Reiser nebo XFS systému souborů pro kořenovou nebo spouštěcí zařízení, potom následující balíčky musí být stažen a nainstalován na hlavního cíle Linuxu před ochrany.

\#/usr/local disku CD

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#ot. / min - ivh kmod-reiserfs 0,0 1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. ot. / min

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#ot. / min - ivh xfsprogs-3.1.1-16.el6.x86\_64. ot. / min

#### <a name="apply-custom-configuration-changes"></a>Změny vlastní konfigurace
Před použitím těchto změn Ujistěte se, že po dokončení předchozí části a pak postupujte podle těchto kroků:

1. Zkopírujte RHEL 6-64 Unified Agent binární do nově vytvořené operačního systému.
2. Spusťte tento příkaz untar binárního souboru: **funkce vkládání - zxvf \<název souboru\>**
3. Spusťte tento příkaz poskytnout oprávnění: \# **./ApplyCustomChanges.sh chmod 755**
4. Spusťte skript:  **\# ./ApplyCustomChanges.sh**. Spusťte skript jenom jednou na serveru. Po spuštění skriptu, restartujte server.

### <a name="install-the-linux-server"></a>Nainstalovat server pro Linux
1. [Stáhněte si](http://go.microsoft.com/fwlink/?LinkID=529757) instalační soubor.
2. Zkopírujte soubor k virtuálnímu počítači hlavního cíle Linuxu pomocí nástroj pro klienta pomocí protokolu sftp, podle vašeho výběru. Případně můžete přihlašuje Linux hlavního cílového virtuálního počítače a použít wget pro stažení instalačního balíčku na zadaný odkaz
3. Na pomocí virtuálních počítačů Linux hlavní cílový server se přihlásit ssh klienta podle vašeho výběru
4. Pokud jste připojení k síti Azure, na kterém jste nasadili Linux hlavní cílový server přes připojení VPN můžete použít interní IP adresu pro server, které můžete najít ve virtuálním počítači **řídicí panel** kartě a port 22, aby Připojte k serveru pomocí Secure Shell hlavního cíle Linuxu.
5. Pokud se připojujete k hlavní cílový server Linux přes veřejný internet připojení použijte Linux hlavní cíl serveru veřejná virtuální IP adresy (z virtuálních počítačů **řídicí panel** kartu) a veřejný koncový bod vytvořen pro ssh Přihlaste se k Linux servder.
6. Extrahujte soubory z archivu algoritmem gzip Linux hlavní cílový Server instalační program vkládání spuštěním: *"funkce vkládání – xvzf Microsoft automatické obnovení systému\_uživatelský Agent\_8.2.0.0\_RHEL6 64\"* z adresáře, který obsahuje soubor Instalační služby.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Pokud jste extrahovali soubory do jiného adresáře přejděte do adresáře pro instalační program byly extrahovat který obsah vkládání archivu. Z této cesty adresáře spustit "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. Po zobrazení výzvy vyberte primární roli vyberte **2 (hlavní cílový)**. Nechte ostatní možnosti interaktivní instalace na jejich výchozí hodnoty.
9. Počkejte před pokračováním instalace a konfigurace rozhraní hostitele se objeví. Nástroj Konfigurace hostitele pro Linux hlavnímu cílovému serveru je nástroj příkazového řádku. Nemění velikost ssh okno nástroj klienta. Vyberte pomocí klávesy se šipkami **globální** možnost a stiskněte klávesu ENTER na klávesnici.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. V poli **IP** zadejte interní IP adresu konfiguračního serveru (najdete ji v **řídicí panel** kartě serveru konfigurace virtuálního počítače) a stiskněte klávesu ENTER. V **Port** zadejte **22** a stiskněte klávesu ENTER.
11. Nechte **použití HTTPS** jako **Ano**, a stiskněte klávesu ENTER.
12. Zadejte přístupové heslo, který byl vytvořen na konfiguračním serveru. Pokud používáte klienta PUTTY z počítače s Windows do ssh pro virtuální počítač Linux můžete vložit obsah schránky Shift + Insert. Zkopírovat heslo do místní schránky pomocí kombinace kláves Ctrl-C a vložte ji pomocí Shift + Insert. Stiskněte klávesu ENTER.
13. Pomocí klávesy se šipkou doprava přejděte k ukončení a stiskněte klávesu ENTER. Počkejte na dokončení instalace.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Pokud z nějakého důvodu, že jste se nepodařilo zaregistrovat hlavní cílový server Linux na konfiguračním serveru, můžete udělat tak znova tak, že spustíte nástroj Konfigurace hostitele /usr/local/ASR/Vx/bin/hostconfigcli (budete nejprve muset nastavit oprávnění k přístupu k tomuto adresář spuštěním chmod jako superuživatele).

#### <a name="validate-master-target-registration"></a>Ověření registrace hlavního cíle
Můžete ověřit, že byl úspěšně zaregistrován hlavní cílový server na konfiguračním serveru v trezoru Azure Site Recovery > **konfigurační Server** > **podrobnosti o serveru**.

> [!NOTE]
> Po registraci hlavní cílový server, pokud se zobrazí chyby konfigurace, které virtuální počítač možná byla odstraněna z Azure nebo koncových bodů nejsou správně nakonfigurovány, je to proto i když se hlavní cíl konfigurace se zjišťují pomocí Azure dndpoints při hlavního cíle nasazení do Azure, tato akce není pro místní hlavní cílový server místní na hodnotu true. To nebude mít vliv na navrácení služeb po obnovení a tyto chyby můžete ignorovat.
>
>

## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Krok 4: Ochrana virtuálních počítačů k místní lokalitě
Je třeba chránit virtuální počítače k místní lokalitě, než jste navrácení služeb po obnovení.

### <a name="before-you-begin"></a>Než začnete
Když virtuální počítač při selhání do Azure, přidá jednotce velmi dočasného souboru stránky. To je další jednotky, která není obvykle nutné ve vaší převzal virtuálních počítačů, protože je již můžete mít na jednotku vyhrazené pro stránkovací soubor. Než začnete zpětné ochranu virtuálních počítačů, je potřeba zajistit, že této jednotky do režimu offline, aby nejsou chráněny. Proveďte to následujícím způsobem:

1. Otevřete správu počítače a vyberte položku Správa úložiště tak, aby vypíše disky, online a připojené k počítači.
2. Vyberte dočasné disk připojený k počítači a zvolte a převeďte ho do režimu offline.

### <a name="protect-the-vms"></a>Ochrana virtuálních počítačů
1. Na portálu Azure zkontrolujte stav virtuálního počítače zajistit, že se při selhání.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. Spusťte vContinuum na váš počítač.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Klikněte na tlačítko **novou ochranu** a vyberte typ systému operaci
4. V novém okně, vyberte Otevřít **typ operačního systému** > **získat podrobnosti o** pro virtuální počítače, které chcete navrácení služeb po obnovení. V **primární server podrobnosti**, zjišťovat a vyberte virtuální počítače, které chcete chránit. Virtuální počítače jsou uvedené pod názvem hostitele vCenter, který se nacházely na před převzetí služeb při selhání.
5. Když vyberete virtuální počítač k ochraně (a již má převzal do Azure) automaticky otevírané okno obsahuje dvě položky pro virtuální počítač. Je to proto, že konfigurační server zjistí dvě instance virtuálních počítačů zaregistrovat do ní. Budete muset odebrat položku pro místní virtuální počítač tak, aby můžete chránit správný virtuální počítač. K identifikaci zde správnou položku virtuálního počítače Azure, můžete Přihlaste se k virtuálnímu počítači Azure a přejděte do C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. V souboru drscout.conf Identifikujte ID hostitele. V dialogovém okně vContinuum zůstala položka, pro které jste našli ID hostitele virtuálního počítače. Odstraňte všechny ostatní položky. Vyberte správný virtuální počítač může odkazovat na jeho IP adresu. IP adresa rozsahu místní bude místní počítač.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. V systému vCenter server zastavit virtuální počítač. Můžete také odstranit virtuálních počítačích na místě.
7. Pak zadejte místní MT server, do které chcete chránit virtuální počítače. K tomu, připojení k serveru vCenter, ke kterému chcete navrácení služeb po obnovení.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. Vyberte hlavní cílový server, na základě na hostiteli, na kterou chcete obnovit virtuální počítač.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Zadejte možnost replikace pro každý z virtuálních počítačů. K tomu je nutné vybrat úložiště dat straně obnovení, do které budou obnoveny virtuálních počítačů. Tabulka shrnuje různé možnosti, které potřebujete poskytovat pro každý virtuální počítač.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **Možnost** | **Možnost doporučená hodnota** |
   | --- | --- |
   |  IP adresa serveru procesů |Vyberte procesový server nasadit v Azure |
   |  Uchování velikost v MB | |
   |  Hodnotu uchování |1 |
   |  Počet dnů nebo hodin |Dnů |
   |  Interval konzistence |1 |
   |  Vyberte cílové úložiště |Úložiště k dispozici v lokalitě pro obnovení. Úložiště dat má dost místa a být k dispozici pro hostitele ESX, na který chcete obnovit virtuální počítač. |
10. Nakonfigurujte vlastnosti, které budou získat virtuální počítač po převzetí služeb při selhání k místní lokalitě. Vlastnosti jsou shrnuté v následující tabulce.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Vlastnost** | **Podrobnosti** |
    | --- | --- |
    | Konfigurace sítě |Pro každý síťový adaptér zjištěn, vyberte ho a klikněte na tlačítko **změnu** konfigurace IP adresy navrácení služeb po obnovení pro virtuální počítač. |
    | Konfigurace hardwaru |Zadejte procesoru a paměti pro virtuální počítač. Nastavení je použít pro všechny virtuální počítače chcete chránit. K identifikaci správné hodnoty pro procesor a paměť, můžete odkazovat na velikosti role virtuálních počítačů IAAS a zobrazit počet jader a paměti. |
    | Zobrazované jméno |Po navrácení služeb po obnovení místně můžete přejmenovat virtuálních počítačů, jak se objeví v inventáři vCenter. Výchozí název je název hostitele počítače virtuálního počítače. Chcete-li zjistit název virtuálního počítače, může odkazovat seznamu virtuálních počítačů ve skupině ochrany. |
    | Konfigurace NAT |Podrobněji níže |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfigurace nastavení NAT.
1. Pokud chcete povolit ochranu virtuálních počítačů, třeba vytvořit dva komunikační kanály. První kanál je mezi virtuálním počítačem a procesového serveru. Tento kanál shromažďuje data z virtuálního počítače a odešle ji na procesní server, který pak odesílá data na hlavním cílovém serveru. Pokud jsou procesový server a virtuální počítač, měly by být ve stejné virtuální síti Azure je nemusíte používat nastavení překladu síťových adres. V opačném případě zadejte nastavení překladu síťových adres. Zobrazte veřejnou IP adresu procesový server v Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. Druhý kanál je mezi procesový server a hlavní cílový server. Možnost použít NAT, nebo ne závisí na tom, jestli se pomocí připojení VPN na základě nebo komunikaci přes internet. Nevybírejte NAt, pokud používáte sítě VPN, ale jenom v případě, že používáte připojení k Internetu.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Pokud nebyly odstraněny místní virtuální počítače jako zadaný a pokud je úložiště dat, které jste se nedaří zpět, je stále obsahuje staré VMDK pak musíte zajistit, aby navrácení služeb po obnovení, který získá virtuální počítač vytvořený na nové místo. Vyberte **Upřesnit** nastavení a zadejte alternativní složku k obnovení v **nastavení název složky**. Nechte ostatní možnosti ve výchozím nastavení. Nastavení názvu složky platí pro všechny servery.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. Spusťte Kontrola připravenosti zajistit, že virtuální počítače jsou připraveny k chránit zpět na místní.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Počkejte na její dokončení. Pokud je úspěšně na všech virtuálních počítačích můžete zadat název plánu ochrany. Pak klikněte na tlačítko **chránit** zahájíte.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. Můžete sledovat průběh v vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Po kroku **aktivace plán ochranu** dokončení můžete monitorovat ochranu virtuálního počítače na portálu Site Recovery.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. Přesný stav monitorování jejím průběhu a kliknete na virtuální počítač můžete zobrazit.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Příprava plánu navrácení služeb po obnovení
Můžete připravit plán navrácení služeb po obnovení pomocí vContinuum, takže aplikace mohou být se nezdařilo zpět do místního serveru kdykoli. Tyto plány obnovení jsou velmi podobné do plánů obnovení ve službě Site Recovery.

1. Spusťte vContinuum a vyberte **Spravovat plány** > **obnovit.** Najdete v seznamu všechny plány, které byly použity k převzetí služeb virtuálních počítačů. Můžete použít stejné plány obnovení.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Vyberte plán ochranu a všechny virtuální počítače, kterou chcete obnovit v něm. Když vyberete jednotlivé virtuální počítače zobrazíte další podrobnosti, včetně cílový server ESX a zdrojový disk virtuálního počítače. Klikněte na tlačítko **Další** spustíte Průvodce obnovit a vyberte virtuální počítače, kterou chcete obnovit.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Můžete obnovit podle několik možností, ale doporučujeme použít **nejnovější značky** a vyberte **použít pro všechny virtuální počítače** zajistit, že se použije nejnovější data z virtuálního počítače.
4. Spustit **kontrolu připravenosti.** Pokud chcete povolit obnovení virtuálního počítače nakonfigurovány správné parametry se změnami. Klikněte na tlačítko **Další** Pokud všechny kontroly jsou úspěšné. Pokud není v protokolu a případné chyby opravte.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. V **konfigurace virtuálního počítače** ověřte, zda jsou správně nastaveny nastavení obnovení. Pokud potřebujete, můžete změnit nastavení virtuálního počítače.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Projděte si seznam virtuálních počítačů, které budou obnoveny a určete pořadí obnovení. Všimněte si, že virtuální počítače jsou uvedeny pomocí název hostitelského počítače. Může být obtížné mapovat název hostitelského počítače k virtuálnímu počítači.
   Mapování názvů, přejděte na virtuální počítače **řídicí panel** v Azure a zkontrolujte název hostitele virtuálního počítače.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Zadejte název plánu a vyberte **později obnovit**. Doporučujeme, abyste se později obnovit, protože počáteční ochrany nemusí být úplná.
8. Klikněte na **obnovit** uložíte plánu nebo aktivovat obnovení, pokud jste vybrali k obnovení teď a ne později. Můžete zkontrolovat stav chcete zobrazit, pokud byl plán uloženého obnovení.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Obnovení virtuálních počítačů
Po vytvoření plánu, můžete obnovit virtuální počítače. Před zkontrolujte, že virtuální počítače dokončili synchronizace. Pokud se bude zobrazovat stav replikace OK dokončení ochrany a prahovou hodnotu RPO byla splněna. Můžete ověřit stav ve vlastnostech virtuálního počítače.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Než zahájíte obnovení, vypněte virtuální počítače Azure. To zajišťuje neexistuje žádné schizofrenní a že uživatelé budou přistupovat pouze jedna kopie aplikace.

1. Spusťte uložený plán. V vContinuum vyberte **monitorování** plány. Toto jsou uvedeny všechny plány, které byly spuštěny.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. Vyberte plán v **obnovení** a klikněte na tlačítko **spustit**. Obnovení můžete sledovat. Po zapnuli virtuálních počítačů na můžete připojit k je v systému vCenter.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Chránit na Azure znovu po navrácení služeb po obnovení
Po dokončení navrácení služeb po obnovení budete pravděpodobně chtít chránit virtuální počítače znovu. Proveďte to následujícím způsobem:

1. Zkontrolujte, zda jsou virtuální počítače na místní pracovní a které aplikace jsou dostupné.
2. Na portálu Azure Site Recovery vyberte virtuální počítače a odstraňte je. Vyberte, chcete-li zakázat ochranu virtuálních počítačů. Zajistíte tak, že jsou virtuální počítače žádné další chráněné.
3. V Azure odstraňte neúspěšný přes virtuální počítače Azure
4. Odstranění původního virtuálního počítače na vSpehere. Toto jsou virtuální počítače, které jste dříve převzetí služeb při selhání do Azure.
5. Na portálu Site Recovery Chraňte virtuální počítače, které nedávno převzít služby při selhání. Po, že jsou chráněné můžete je přidat do plánu obnovení.

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o](site-recovery-vmware-to-azure-classic.md) replikace virtuálních počítačů VMware a fyzické servery do Azure pomocí rozšířeného nasazení.
