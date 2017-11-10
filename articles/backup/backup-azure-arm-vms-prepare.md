---
title: "Zálohování Azure: Příprava k zálohování virtuálních počítačů | Microsoft Docs"
description: "Ujistěte se, že vaše prostředí je připravený pro zálohování virtuálních počítačů v Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování; zálohování;"
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/3/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: b8a770323d115390d323352826457eee62be5f6f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Příprava prostředí pro zálohování virtuálních počítačů s nasazením Resource Manageru
> [!div class="op_single_selector"]
> * [Model Resource Manager](backup-azure-arm-vms-prepare.md)
> * [Klasického modelu](backup-azure-vms-prepare.md)
>
>

Tento článek obsahuje kroky pro přípravu prostředí zálohování nasazených Resource Managerem virtuálního počítače (VM). Postupy v postupech pomocí portálu Azure.  

Služby zálohování Azure má dva typy trezorů (zálohování trezory a trezory služeb zotavení) pro ochranu virtuálních počítačů. Trezor záloh chrání virtuální počítače nasazené pomocí modelu nasazení Classic. Trezor služeb zotavení chrání **nasazení Classic i Resource Manager nasazené virtuální počítače**. Trezor služeb zotavení je nutné použít k ochraně virtuálních počítačů nasazených Resource Managerem.

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). V tématu [Příprava prostředí pro zálohování virtuálních počítačů Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.
>
>

Chcete-li chránit nebo zálohovat nasazených Resource Managerem virtuálního počítače (VM), zkontrolujte, zda že existují tyto požadavky:

* Vytvoření trezoru služeb zotavení (nebo identifikovat existující trezor služeb zotavení) *ve stejném umístění jako virtuální počítač*.
* Vyberte scénář, definování zásad zálohování a určení položek k ochraně.
* Kontrola instalace agenta virtuálního počítače na virtuálním počítači.
* Zkontrolujte připojení k síti
* Pro virtuální počítače s Linuxem, v případě, že chcete přizpůsobit zálohování prostředí pro aplikace, které jsou konzistentní zálohování prosím postupujte podle kroků [postup skripty snímek před a po pořízení snímku konfigurace](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Pokud znáte tyto podmínky již neexistuje ve vašem prostředí poté pokračujte [zálohování váš článek virtuálních počítačů](backup-azure-vms.md). Pokud potřebujete nastavit, nebo zkontrolovat, žádný z těchto požadavků, tento článek vás provede kroky při přípravě splnění tohoto požadavku.

##<a name="supported-operating-system-for-backup"></a>Podporovaný operační systém pro zálohování
 * **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), kromě základního OS Linux. _Další přineste-vaše – vlastní-Linuxových distribucích také může fungovat, dokud agent virtuálního počítače je k dispozici na virtuálním počítači a podpora pro Python existuje. Jsme však není neschvaluje těchto distribuce pro zálohování._
 * **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Omezení při zálohování a obnovení virtuálních počítačů
Než se připravíte prostředí, prosím vědět o omezeních.

* Zálohování virtuálních počítačů s více než 16 datových disků není podporována.
* Nepodporuje zálohování virtuálních počítačů s daty velikosti disku je větší než 1023GB.

> [!NOTE]
> Máme privátní Preview verzi pro podporu zálohování pro virtuální počítače s > 1TB nespravované disky. Podrobnosti najdete na [privátní Preview verzi pro podporu zálohování velkých disků virtuálních počítačů](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

* Zálohování virtuálních počítačů s vyhrazenou IP adresu a žádný definovaný koncový bod není podporována.
* Zálohování virtuálních počítačů, které jsou šifrované pomocí právě BEK není podporováno. Zálohování virtuálních počítačů Linux zašifrovaná pomocí šifrování na LUKS není podporováno.
* Zálohování virtuálních počítačů obsahující Volumes(CSV) sdílené clusteru nebo škálování se konfigurace souborového serveru se nedoporučuje, protože vyžadují zahrnující všechny virtuální počítače, které jsou součástí konfigurace clusteru během úlohy snímku. Zálohování Azure nepodporuje konzistence více virtuálních počítačů. 
* Zálohovaná data neobsahuje sítě připojené jednotky připojené k virtuálnímu počítači.
* Nahrazení existujícího virtuálního počítače během obnovení se nepodporuje. Pokud se pokusíte obnovit virtuální počítač, když virtuální počítač existuje, operaci obnovení se nezdaří.
* Mezi oblastmi zálohování a obnovení nejsou podporovány.
* Můžete zálohovat virtuální počítače ve všech veřejných oblastí ve službě Azure (viz [kontrolní seznam](https://azure.microsoft.com/regions/#services) z podporovaných oblastí). Pokud oblast, kterou hledáte, není podporován dnes, nezobrazí se v rozevíracím seznamu při vytváření trezoru.
* Obnovení řadiče domény (DC) virtuálního počítače, který je součástí konfigurace více – řadič domény je možné pouze pomocí prostředí PowerShell. Další informace o [obnovení řadiče domény, řadiče domény služby více](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Obnovení virtuálních počítačů, které mají následující zvláštní síťové konfigurace je podporována pouze pomocí prostředí PowerShell. Po dokončení operace obnovení, nebude mít virtuální počítače vytvořené pomocí pracovního postupu obnovení v uživatelském rozhraní tyto konfigurace sítě. Další informace najdete v tématu [obnovení virtuálních počítačů s konfigurací speciální síťových](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interních a externích)
  * Virtuální počítače s více vyhrazené IP adresy
  * Virtuální počítače s více síťových adaptérů

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Vytvoření trezoru Recovery Services pro virtuální počítač
Trezor služeb zotavení je entita, která ukládá zálohy a body obnovení, které byly vytvořeny v čase. Trezor služeb zotavení obsahuje také zásady zálohování, které jsou přidružené k chráněné virtuální počítače.

Vytvoření trezoru Recovery Services:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce centra klikněte na **Procházet** a v seznamu prostředků zadejte **Služby zotavení**. Během zadávání se seznam bude filtrovat podle zadávaného textu. Klikněte na **Trezor Recovery Services**.

    ![Klikněte na tlačítko Procházet a zadejte služeb zotavení. Když se zobrazí možnost trezoru služeb zotavení, klikněte na něj a otevřete okno trezoru služeb zotavení.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Zobrazí se seznam trezorů služeb zotavení.
3. V nabídce **Trezory Recovery Services** klikněte na **Přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Jako **Název** zadejte popisný název pro identifikaci trezoru. Název musí být jedinečný v rámci předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Kliknutím na **Předplatné** zobrazíte seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné. Více možností bude dostupných pouze pokud je váš účet organizace přidružený k více předplatným Azure.
6. Kliknutím na **Skupina prostředků** zobrazíte seznam dostupných skupin prostředků, nebo klikněte na **Nová**, chcete-li vytvořit novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru. Trezor **musí** být ve stejné oblasti jako virtuální počítače, které chcete chránit.

   > [!IMPORTANT]
   > Pokud si nejste jisti oblastí, ve které jsou vaše virtuální počítače, zavřete dialogové okno vytvoření trezoru a na portálu přejděte na seznam virtuálních počítačů. Pokud máte virtuální počítače v několika oblastech, bude potřeba vytvořit trezor Služeb zotavení v každé oblasti. Vytvořte trezor nejprve v první oblasti, poté přejděte k další oblasti. Není potřeba specifikovat účty úložiště pro ukládání zálohovaných dat – trezor Služeb zotavení a služba Azure Backup se o to postarají automaticky.
   >
   >

8. Klikněte na možnost **Vytvořit**. Vytvoření trezoru Služeb zotavení může chvíli trvat. Sledujte oznámení o stavu v horní pravé části portálu. Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení. Pokud svůj trezor nevidíte, klikněte na tlačítko **aktualizovat** na

    ![Seznam trezorů záloh](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Teď, když jste vytvořili trezor, se naučte, jak nastavit replikaci úložiště.

## <a name="set-storage-replication"></a>Nastavení replikace úložiště
Možnost replikace úložiště umožňuje výběr mezi geograficky redundantním úložištěm a místně redundantním úložištěm. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Ponechte možnost geograficky redundantní úložiště, pokud se jedná o vaši primární zálohu. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá.

Chcete-li upravit nastavení replikace úložiště:

1. Na **trezory služeb zotavení** okně vyberte svůj trezor.
    Když kliknete na tlačítko trezoru, v okně nastavení (*který má název trezoru v horní části*) a otevře se okno údaje trezoru.

    ![Zvolte svůj trezor ze seznamu trezorů záloh](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Na **nastavení** okně posuvníkem svislé přejděte dolů k položce **spravovat** části. Klikněte na tlačítko **infrastruktura zálohování** otevřete její okno. V **Obecné** klikněte na část **konfigurace zálohování** otevřete její okno. V okně **Konfigurace zálohování** zvolte pro svůj trezor možnost replikace úložiště. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud změníte typ replikace úložiště, klikněte na tlačítko **Uložit**.

    ![Seznam trezorů záloh](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání geograficky redundantního úložiště. Pokud používáte Azure jako koncový bod úložiště záloh není primární, zvolte místně redundantní úložiště. Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště naleznete v tématu [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).
    Po výběru možnosti úložiště pro svůj trezor jste připraveni k přidružení virtuálního počítače k trezoru. Chcete-li začít přidružení, měli byste vyhledat a zaregistrovat virtuální počítače Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Výběr cíle zálohování, nastavení zásad a určení položek k ochraně
Před registrací virtuálních počítačů k trezoru spusťte proces vyhledávání, abyste se ujistili, že byly identifikované všechny nové virtuální počítače přidané k předplatnému. Proces se dotáže Azure na seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast. Na webu Azure Portal výraz scénář vyjadřuje, co chcete vložit do trezoru Recovery Services. Zásada je plán, jak často a kdy jsou pořizovány body obnovení. Zásada také obsahuje rozsah uchování bodů obnovení.

1. Pokud již máte otevřený trezor Služeb zotavení, pokračujte ke kroku 2. Pokud nemáte trezoru služeb zotavení otevřít, otevřete [portál Azure](https://portal.azure.com/) a v nabídce centra klikněte na tlačítko **další služby**.

   * V seznamu prostředků zadejte **Recovery Services**.
   * Během zadávání se seznam bude filtrovat podle zadávaného textu. Až uvidíte **Trezory Služeb zotavení**, klikněte na ně.

     ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Objeví se seznam trezorů Služeb zotavení. Pokud ve vašem předplatném nejsou žádné trezory, bude tento seznam prázdný.

    ![Zobrazení seznamu trezorů služby Recovery Services](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Ze seznamu trezorů služeb zotavení vyberte trezor otevřete svůj řídicí panel.

     Otevře se okno nastavení a řídícím panelu trezoru pro zvolený trezor.

     ![Otevřené okno trezoru](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Kliknutím na **Zálohování** v nabídce řídicího panelu trezoru otevřete okno Zálohování.

    ![Otevřené okno Zálohování](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Otevřou se okna Zálohování a Cíl zálohování.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. V okně cíl zálohování nastavte **kde běží vaše úlohy** do Azure a **co chcete zálohovat** k virtuálnímu počítači klikněte **OK**.

    Tím se v trezoru zaregistruje rozšíření virtuálního počítače. Zavře se okno Cíl zálohování a otevře se okno **Zásady zálohování**.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. V okně Zásady zálohování vyberte zásadu zálohování, kterou chcete pro trezor použít.

    ![Výběr zásady zálohování](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Podrobnosti výchozí zásady jsou uvedené pod rozevírací nabídkou. Chcete-li vytvořit novou zásadu, vyberte z rozevírací nabídky **Vytvořit novou**. Pokyny k definování zásad zálohování naleznete v tématu [Definování zásad zálohování](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Kliknutím na **OK** přidružte zásadu zálohování k trezoru.

    Zavře se okno Zásady zálohování a otevře se okno **Výběr virtuálních počítačů**.
5. V okně **Výběr virtuálních počítačů** vyberte virtuální počítače, které se mají přidružit k určené zásadě a klikněte na **OK**.

    ![Výběr úlohy](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Vybraný virtuální počítač se ověří. Pokud se virtuální počítače, které jste chtěli najdete v části nezobrazí, zkontrolujte, zda budou existovat ve stejné oblasti Azure jako trezor služeb zotavení a už nejsou chráněné v jiné trezoru. Umístění trezoru služby Recovery Services je uvedené na řídicím panelu trezoru.

6. Teď, když jste definovali všechna nastavení trezoru, klikněte v okně Zálohování na **Povolit zálohování**. To nasadí zásadu pro trezor a virtuální počítače. Nevytvoří se tím prvotní bod obnovení pro virtuální počítač.

    ![Povolení zálohování](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Po úspěšném povolení zálohování se vaše zásada zálohování spustí podle plánu. Pokud chcete generovat úlohu zálohování na vyžádání pro zálohování virtuálních počítačů, vidět [aktivuje úloha zálohování](./backup-azure-arm-vms.md#triggering-the-backup-job).

Pokud máte potíže s registrací virtuální počítač, zobrazíte následující informace o instalaci agenta virtuálního počítače a na připojení k síti. Pravděpodobně ani nepotřebujete následující informace Pokud chráníte virtuální počítače vytvořené v Azure. Ale pokud jste migrovali virtuální počítače do Azure, je nutné správně instalaci agenta virtuálního počítače a virtuální počítač může komunikovat se službou virtuální sítě.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalace agenta virtuálního počítače na virtuální počítač
Pro fungování rozšíření Backup musí být na virtuálním počítači Azure nainstalovaný agent virtuálního počítače Azure. Pokud byl váš virtuální počítač vytvořen z Galerie Azure, pak agenta virtuálního počítače již existuje ve virtuálním počítači. Tyto informace jsou poskytovány pro situace, kdy jsou *není* pomocí virtuální počítač vytvořen z Galerie Azure – třeba migrovat virtuální počítač z překážek místní datacentra. V takovém případě musí být nainstalovaný za účelem ochrany virtuálního počítače agenta virtuálního počítače. Další informace o [agenta virtuálního počítače](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Pokud máte problémy se zálohováním virtuálního počítače Azure, zkontrolujte, zda je agent virtuálního počítače na virtuálním počítači správně nainstalovaný (viz následující tabulka). Následující tabulka poskytuje další informace o agentu virtuálního počítače pro virtuální počítače s Windows nebo Linuxem.

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace agenta virtuálního počítače |Stáhněte si a nainstalujte [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). K dokončení instalace budete potřebovat oprávnění správce. |<li> Nainstalujte si nejnovější verzi [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md). K dokončení instalace budete potřebovat oprávnění správce. Doporučujeme nainstalovat agenta z distribuční úložiště. Jsme **nedoporučujeme** instalaci agenta virtuálního počítače s Linuxem přímo z githubu.  |
| Aktualizace agenta virtuálního počítače |Aktualizace agenta virtuálního počítače je stejně jednoduchá, jako přeinstalace [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Ujistěte se, že během aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |Postupujte podle pokynů v tématu [Aktualizace agenta virtuálního počítače s Linuxem](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Doporučujeme aktualizovat agenta z distribuční úložiště. Jsme **nedoporučujeme** aktualizace agenta virtuálního počítače s Linuxem přímo z githubu.<br>Ujistěte se, že během aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |
| Ověření instalace agenta virtuálního počítače |<li>Ve virtuálním počítači Azure přejděte do složky *C:\WindowsAzure\Packages*. <li>Měl by být přítomný soubor WaAppAgent.exe.<li> Pravým tlačítkem myši klikněte na soubor, přejděte na **Vlastnosti** a poté vyberte kartu **Podrobnosti**. Pole Verze produktu by mělo být 2.6.1198.718 nebo vyšší. |Není dostupné. |

### <a name="backup-extension"></a>Rozšíření zálohování
Když je agent virtuálního počítače nainstalovaný na virtuálním počítači, služba Azure Backup nainstaluje do agenta virtuálního počítače rozšíření zálohování. Služba Azure Backup bezproblémově upgraduje a opravuje rozšíření zálohování.

Rozšíření zálohování je službou Backup nainstalováno bez ohledu na to, zda je virtuální počítač spuštěný, nebo ne. Spuštěný virtuální počítač poskytuje největší šanci získání bodu obnovení, který je konzistentní v rámci aplikace. Služba Azure Backup nicméně bude pokračovat v zálohování virtuálního počítače, i když je vypnutý a rozšíření nebylo možné nainstalovat. To se označuje jako Virtuální počítač v režimu offline. V takovém případě bude bod obnovení *konzistentní pro případ chyby*.

## <a name="network-connectivity"></a>Připojení k síti
Abyste mohli spravovat snímky virtuálních počítačů, potřebuje rozšíření zálohování připojení k Azure veřejné IP adresy. Bez správné připojení k Internetu vypršení časového limitu pro požadavky HTTP virtuálního počítače a operaci zálohování se nezdaří. Pokud vaše nasazení má omezení přístupu na místě (prostřednictvím skupinu zabezpečení sítě (NSG), např.), zvolte jednu z těchto možností pro poskytování zrušte cestu pro provoz zálohování:

* [Seznam povolených adres Azure datacenter IP rozsahy](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -najdete v článku pokyny na tom, jak povolených IP adres.
* Nasazení proxy server HTTP pro směrování provozu.

Při rozhodování, kterou možnost použít, jsou kompromisy mezi spravovatelnosti, přesná kontrola a náklady.

| Možnost | Výhody | Nevýhody |
| --- | --- | --- |
| Rozsahy povolených IP adres |Žádné dodatečné poplatky.<br><br>Pro přístup k otevření v skupinu NSG, použijte <i>Set-AzureNetworkSecurityRule</i> rutiny. |Komplexní správa jako ovlivněné časem změnit rozsahy IP adres.<br><br>Poskytuje přístup k celé Azure a ne jenom úložiště. |
| Server proxy protokolu HTTP |Přesná kontrola v proxy přes úložiště povolené adresy URL.<br>Jeden bod Internet přístup k virtuálním počítačům.<br>Není v souladu změny Azure IP adresy. |Další náklady pro spuštění virtuálního počítače s proxy software. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Seznam povolených adres Azure datacenter rozsahy IP adres
* Seznam povolených adres rozsahy IP datové centrum Azure, najdete [webu Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653) podrobnosti o rozsahy IP adres a pokyny.
* Značky služby můžete povolit připojení k úložišti určité oblasti použití [služby značky](../virtual-network/security-overview.md#service-tags). Ujistěte se, že pravidlo, které umožňuje přístup k účtu úložiště má vyšší prioritu než pravidlo blokování přístupu k Internetu. 

  ![Skupina NSG s značky úložiště pro oblast](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Značky úložiště jsou k dispozici pouze v určitých oblastí a jsou ve verzi preview. Seznam oblastí, najdete v části [služby značky pro úložiště](../virtual-network/security-overview.md#service-tags)

### <a name="using-an-http-proxy-for-vm-backups"></a>Pomocí proxy serveru HTTP pro zálohování virtuálních počítačů
Při zálohování virtuálních počítačů, rozšíření zálohování na virtuální počítač odešle snímek příkazy pro správu do služby Azure Storage pomocí rozhraní API HTTPS. Směrujte provoz rozšíření zálohování přes server proxy protokolu HTTP vzhledem k tomu, že je jedinou komponentou nakonfigurovaný pro přístup do veřejného Internetu.

> [!NOTE]
> Neexistuje žádná doporučení pro proxy software, který se má použít. Ujistěte se, že vyberete proxy server, který je kompatibilní s níže uvedené kroky konfigurace.
>
>

Následující příklad obrázek ukazuje tři konfiguračních kroků nezbytných k používání proxy serveru HTTP:

* Virtuální počítač aplikace směrovat veškerý provoz protokolu HTTP vázaný veřejného Internetu prostřednictvím proxy serveru virtuálního počítače.
* Proxy virtuálních počítačů umožňuje příchozí provoz z virtuálních počítačů ve virtuální síti.
* Skupina zabezpečení sítě (NSG), s názvem NFP uzamčení musí zabezpečení pravidlo povolení odchozí internetové přenosy z virtuálního počítače proxy serveru.

![Skupina NSG s diagram nasazení proxy serveru HTTP](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

Proxy server HTTP komunikaci do veřejného Internetu, postupujte podle těchto kroků:

#### <a name="step-1-configure-outgoing-network-connections"></a>Krok 1. Konfigurace odchozích síťových připojení
###### <a name="for-windows-machines"></a>Pro počítače s Windows
To se nastavit konfiguraci proxy serveru pro místní systémový účet.

1. Stáhněte si [nástroje PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Z řádku se zvýšenými oprávněními spusťte následující příkaz

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Otevře se okno internet Exploreru.
3. Přejděte na Nástroje -> Možnosti Internetu -> připojení -> Nastavení místní sítě.
4. Ověřte nastavení proxy serveru pro účet System. Nastavení proxy serveru IP adresy a portu.
5. Zavřete Internet Explorer.

To se nastavit konfiguraci proxy celého systému a se použije pro všechny odchozí přenosy HTTP/HTTPS.

Pokud jste nastavili proxy server na aktuální uživatelský účet (ne místní účet systému), použijte následující skript je aplikovat na SYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Pokud zjistíte v protokolu serveru proxy "(407) vyžadováno ověření proxy serveru", zkontrolujte, že ověřování je správně nastavená.
>
>

###### <a name="for-linux-machines"></a>Pro počítače se systémem Linux
Přidejte následující řádek na ```/etc/environment``` souboru:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Přidejte následující řádky, které se ```/etc/waagent.conf``` souboru:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Krok 2. Povolit příchozí připojení na proxy serveru:
1. Na serveru proxy otevřete bránu Windows Firewall. Nejjednodušší způsob, jak získat přístup k bráně firewall je pro vyhledávání pro bránu Windows Firewall s pokročilým zabezpečením.

    ![Otevření brány Firewall](./media/backup-azure-vms-prepare/firewall-01.png)
2. V dialogovém okně brány Windows Firewall, klikněte pravým tlačítkem na **příchozí pravidla** a klikněte na tlačítko **nové pravidlo...** .

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-02.png)
3. V **pravidla Průvodce vytvořením nového příchozího**, vyberte **vlastní** možnost **typ pravidla** a klikněte na tlačítko **Další**.
4. Na stránce vybrat **programu**, zvolte **všechny programy** a klikněte na tlačítko **Další**.
5. Na **protokol a porty** stránky, zadejte následující informace a klikněte na **Další**:

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-03.png)

   * pro *protokolu typ* zvolte *TCP*
   * pro *místního portu* zvolte *specifické porty*, do následujícího pole zadejte ```<Proxy Port>``` který byl nakonfigurovaný.
   * pro *vzdálených portů* vyberte *všechny porty*

     Pro zbytek průvodce klikněte na úplně na konec a pojmenujte toto pravidlo.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Krok 3. Přidejte pravidlo výjimky k této skupině:
V příkazovém řádku prostředí Azure PowerShell zadejte následující příkaz:

Následující příkaz přidá výjimku NSG. Tato výjimka umožňuje TCP provoz z jakéhokoli portu na 10.0.0.5 všechny internetové adresy na portu 80 (HTTP) nebo 443 (HTTPS). Pokud budete potřebovat specifického portu v veřejného Internetu, je nutné přidat tento port k ```-DestinationPortRange``` také.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*Tyto kroky používají konkrétní názvy a hodnoty v tomto příkladu. Použijte prosím názvy a hodnoty pro nasazení, při zadávání, nebo vyjímání a vkládání podrobnosti do vašeho kódu.*

Teď, když víte, že máte síťové připojení, jste připraveni k zálohování virtuálního počítače. V tématu [zálohování virtuálních počítačů nasazených Resource Managerem](backup-azure-arm-vms.md).

## <a name="questions"></a>Máte dotazy?
Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
Teď, když připravíte prostředí pro zálohování virtuálního počítače, vaše další logické krokem je vytvoření zálohy. Plánování článek poskytuje podrobnější informace o zálohování virtuálních počítačů.

* [Zálohování virtuálních počítačů](backup-azure-vms.md)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](backup-azure-vms-introduction.md)
* [Správa záloh virtuálních počítačů](backup-azure-manage-vms.md)
