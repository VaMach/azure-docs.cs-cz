---
title: "Pomocí serveru Azure Backup zálohovat úlohy na portál Azure classic | Microsoft Docs"
description: "Ujistěte se, že vaše prostředí správně připravena k zálohování úloh pomocí serveru Azure Backup"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "Azure backup server; úložiště záloh"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: db19729f652430c80ad0a7af1b630c1c5a1561d3
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Příprava zálohování úloh pomocí Azure Backup Serveru
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek je o přípravě vašeho prostředí a zálohování úloh pomocí serveru Azure Backup. Pomocí serveru Azure Backup Server může chránit úlohy aplikace jako virtuální počítače Hyper-V, Microsoft SQL Server, SharePoint Server, Microsoft Exchange a klienti systému Windows z jediné konzoly.

> [!WARNING]
> Azure Backup Server dědí funkce Data Protection Manager (DPM) pro úlohu zálohování. Najdete ukazatele odkazující na dokumentaci DPM pro některé z těchto funkcí. Azure zálohování serveru ale není zajistit ochranu na pásce nebo integrovat s nástrojem System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Počítače Windows serveru
![step1](./media/backup-azure-microsoft-azure-backup/step1.png)

Prvním krokem k serveru Azure Backup zprovoznění je do mají počítače s Windows serverem.

| Umístění | Minimální požadavky | Další pokyny |
| --- | --- | --- |
| Azure |Virtuální počítač Azure IaaS<br><br>A2 Standardní: 2 jádra, 3.5GB paměti RAM |Můžete začít s jednoduché Galerie bitové kopie systému Windows Server 2012 R2 Datacenter. [Ochrana zatížení IaaS v Azure Backup Server (DPM)](https://technet.microsoft.com/library/jj852163.aspx) má mnoho drobné odlišnosti. Přečtěte si článek úplně před nasazením na počítač. |
| Lokálně |Virtuální počítač technologie Hyper-V,<br> Virtuální počítač VMWare,<br> nebo adresu fyzického hostitele.<br><br>2 jádra a 4GB paměti RAM |Úložiště DPM využívající Windows Server odstranění duplicitních dat můžete duplicity odstranit. Další informace o [aplikace DPM a odstranění duplicit](https://technet.microsoft.com/library/dn891438.aspx) spolupracují při nasazení ve virtuálních počítačích Hyper-V. |

> [!NOTE]
> Doporučuje se nainstalovaný Azure Backup Server na počítači s Windows Server 2012 R2 Datacenter. Mnoho požadované součásti jsou automaticky popsané na nejnovější verzi operačního systému Windows.
>
>

Pokud máte v plánu připojit k serveru Azure Backup k doméně, se doporučuje, aby připojíte fyzický server nebo virtuální počítač k doméně, před instalací softwaru serveru Azure Backup. Přesunutí serveru Azure Backup pro novou doménu, po nasazení je *nepodporuje*.

## <a name="2-backup-vault"></a>2. Úložiště záloh
![step2](./media/backup-azure-microsoft-azure-backup/step2.png)

Ať už odesílání zálohovaných dat do Azure nebo udržovat místně, musí být zaregistrován Azure Backup Server do trezoru. Pokud jsou nového uživatele Azure Backup a chcete použít Azure Backup Server, najdete v části portálu Azure verzi tohoto článku - [Příprava zálohování úloh pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po 30. listopadu 2017 nebudete moct pomocí prostředí PowerShell vytvořit trezory Backup. **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>



## <a name="3-software-package"></a>3. Balíček softwaru
![ke kroku 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-the-software-package"></a>Stažení balíčku softwaru
Podobně jako přihlašovací údaje trezoru, si můžete stáhnout Microsoft Azure Backup pro zatížení aplikace z **stránky rychlý Start** úložiště záloh.

1. Klikněte na tlačítko **pro zatížení aplikace (Disk na Disk do cloudu)**. Tím přejdete na stránku Stažení softwaru, z něhož lze stáhnout balíček softwaru.

    ![Microsoft Azure Backup úvodní obrazovce](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Klikněte na **Stáhnout**.

    ![Stažení softwaru 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Vyberte všechny soubory a klikněte na **Další**. Stáhnout všechny soubory přicházející ze stránky pro stažení aplikace Microsoft Azure Backup a umístěte všechny soubory ve stejné složce.
   ![Stažení softwaru 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Vzhledem k tomu, že stažení velikost všech souborů je dohromady > 3G, na 10 MB/s stahoval odkazu, který může trvat až 60 minut ke stažení pro dokončení.

### <a name="extracting-the-software-package"></a>Extrahování balíček softwaru
Po stažení všechny soubory, klikněte na tlačítko **MicrosoftAzureBackupInstaller.exe**. Tato akce spustí **Průvodce instalací aplikace Microsoft Azure Backup** Extrahujte instalační soubory do umístění, které jste zadali. Postupujte podle pokynů průvodce a klikněte na **extrahovat** tlačítko Zahájit proces extrakce.

> [!WARNING]
> K extrakci instalačních souborů se vyžaduje minimálně 4GB volného místa.
>
>

![Průvodce instalací zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Po dokončení procesu extrahování políčko Spustit čerstvě extrahované *setup.exe* k zahájení instalace serveru Microsoft Azure Backup a klikněte na **Dokončit** tlačítko.

### <a name="installing-the-software-package"></a>Instalace balíčku softwaru
1. Klikněte na tlačítko **Microsoft Azure Backup** spustíte Průvodce instalací.

    ![Průvodce instalací zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Na úvodní obrazovce klikněte **Další** tlačítko. Tím přejdete *požadovaných součástí kontroluje* části. Na této obrazovce klikněte na **zkontrolujte** tlačítko k určení, pokud byly splněny požadavky hardware a software pro Azure Backup Server. Pokud jsou všechny požadované součásti byly úspěšně splněny, zobrazí se zpráva, že počítač splňuje požadavky. Klikněte na **Další** tlačítko.

    ![Zkontrolujte Server Azure Backup - uvítací a požadavky](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server vyžaduje SQL Server Standard a serveru Azure Backup instalační balíček obsahuje připojené příslušné binární soubory systému SQL Server potřebné. Při spouštění s novou instalací serveru Azure Backup, měli byste vybrat možnost **nainstalovat nové Instance systému SQL Server s tímto nastavením** a klikněte na **zkontrolovat a nainstalovat** tlačítko. Jakmile požadavky jsou úspěšně nainstalováni, klikněte na možnost **Další**.

    ![Server Azure Backup - kontrola SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Pokud dojde k selhání s doporučení k restartování tohoto počítače a klikněte na tlačítko **zkontrolujte znovu**.

   > [!NOTE]
   > Azure Backup Server nebude fungovat se vzdálenou instanci systému SQL Server. Instance serveru Azure Backup používá musí být místní.
   >
   >

4. Zadejte umístění pro instalaci souborů serveru Microsoft Azure Backup a klikněte na **Další**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Pomocné umístění je požadavek pro zálohování na Azure. Zkontrolujte, zda pomocné umístění nejméně 5 % dat plánované zálohování do cloudu. U ochrany disku samostatné disky potřeba nakonfigurovat po dokončení instalace. Další informace týkající se fondů úložiště najdete v tématu [nakonfigurujte fondy úložiště a disk úložiště](https://technet.microsoft.com/library/hh758075.aspx).
5. Zadejte silné heslo pro omezené místní uživatelské účty a klikněte na **Další**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Vyberte, zda chcete použít *Microsoft Update* vyhledávat aktualizace, a klikněte na tlačítko **Další**.

   > [!NOTE]
   > Doporučujeme vám, že přesměrování na webu Microsoft Update nabízí aktualizace zabezpečení a důležité aktualizace pro Windows a další produkty, jako je Microsoft Azure Backup Server služby Windows Update.
   >
   >

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Zkontrolujte *souhrn nastavení* a klikněte na tlačítko **nainstalovat**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Instalace probíhá fáze. V první fázi agenta služeb zotavení Microsoft Azure je nainstalován na serveru. Průvodce také zkontroluje připojení k Internetu. Pokud je k dispozici připojení k Internetu může pokračovat v instalaci, pokud ne, budete muset zadat podrobnosti o proxy serveru pro připojení k Internetu.

    Dalším krokem je konfigurace agenta služeb zotavení Microsoft Azure. Jako součást konfigurace je nutné zajistit, že jste přihlašovací údaje trezoru, registraci počítače do trezoru záloh. Bude také zadat přístupové heslo k šifrování a dešifrování dat posílaných mezi Azure a vaše místní. Může automaticky generovat přístupové heslo nebo zadejte minimální heslo 16 znaků. Pokračujte v průvodci, dokud agent nebyl nakonfigurován.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Po úspěšném dokončení registrace Microsoft Azure Backup server pokračuje celkové Průvodce instalací k instalaci a konfiguraci systému SQL Server a součásti serveru Azure Backup. Po dokončení instalace součástí systému SQL Server, jsou nainstalované komponenty serveru Azure Backup.

    ![Server Azure Backup](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Po dokončení kroku instalace produktu ikony na ploše se vytvořily také. Právě dvakrát klikněte na ikonu pro spuštění produktu.

### <a name="add-backup-storage"></a>Přidání úložiště záloh
První záložní kopie se ukládají na úložiště připojená k počítači Azure Backup Server. Další informace o přidávání disků, najdete v tématu [nakonfigurujte fondy úložiště a disk úložiště](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Je nutné přidat úložiště záloh, i v případě, že chcete odesílat data do Azure. V architektuře aktuální Azure Backup Server úložiště záloh Azure obsahuje *druhý* kopie dat, zatímco místní úložiště obsahuje záložní kopie první (a povinné).  
>
>

## <a name="4-network-connectivity"></a>4. Připojení k síti
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Azure Backup Server vyžaduje připojení ke službě Azure Backup pro produkt, nemusí fungovat správně. Chcete-li ověřit, zda je počítač připojen k Azure, použijte ```Get-DPMCloudConnection``` příkaz v konzole prostředí PowerShell pro Server Azure Backup. Pokud výstup příkaz hodnotu TRUE, pak připojení existuje, jinak je k dispozici žádné připojení.

Předplatné Azure ve stejnou dobu, musí být v dobrém stavu. Chcete-li zjistit stav předplatného a k její správě, přihlaste se k [předplatné portál](https://account.windowsazure.com/Subscriptions).

Jakmile víte o stavu připojení Azure a předplatné Azure, můžete zjistit dopad na zálohování a obnovení funkce nabízené následující tabulka.

| Stav připojení | předplatné Azure | Zálohování do Azure | Zálohování na disk | Obnovení z Azure | Obnovení z disku |
| --- | --- | --- | --- | --- | --- |
| připojení |Aktivní |Povoleno |Povoleno |Povoleno |Povoleno |
| připojení |Platnost |Zastaveno |Zastaveno |Povoleno |Povoleno |
| připojení |Zrušit |Zastaveno |Zastaveno |Body obnovení zastaven a Azure odstranit |Zastaveno |
| Ke ztrátě připojení > 15 dnů |Aktivní |Zastaveno |Zastaveno |Povoleno |Povoleno |
| Ke ztrátě připojení > 15 dnů |Platnost |Zastaveno |Zastaveno |Povoleno |Povoleno |
| Ke ztrátě připojení > 15 dnů |Zrušit |Zastaveno |Zastaveno |Body obnovení zastaven a Azure odstranit |Zastaveno |

### <a name="recovering-from-loss-of-connectivity"></a>Obnovení z ztráty připojení
Pokud máte bránu firewall nebo proxy server, který je brání přístupu k Azure, musíte do seznamu povolených IP adres následující domény adresy v profilu brány firewall a proxy:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Po připojení k Azure byla obnovena do počítače serveru Azure Backup, operace, které lze provést Určuje stav předplatného Azure. Výše uvedené tabulce obsahuje podrobnosti o činnosti povolené počítač po "připojení".

### <a name="handling-subscription-states"></a>Zpracování – stavy předplatného
Je možné provést z předplatného Azure *platnost vypršela* nebo *Deprovisioned* do stavu *Active* stavu. To má ale některé vliv na chování produktu při stav není *Active*:

* A *Deprovisioned* předplatné ztratí funkce pro dobu, po kterou je zrušit. Na měnící *Active*, je obnovená produktu funkce zálohování a obnovení. Také možné načíst zálohovaná data na místní disk, pokud byla u s dobou uchování dostatečně velké. Zálohování dat v Azure je však nenahraditelně ztraceny po zadání předplatné *Deprovisioned* stavu.
* *Platnost vypršela* předplatné pouze ztratí funkce pro, dokud byly provedeny *Active* znovu. Všechny záloh naplánovaných pro dobu, po kterou byl odběr *platnost vypršela* se nespustí.

## <a name="troubleshooting"></a>Řešení potíží
Pokud Microsoft Azure Backup server selže s chybami během fáze instalace (nebo zálohování nebo obnovení), podívejte se na to [dokumentu kódy chyb](https://support.microsoft.com/kb/3041338) Další informace.
Můžete se také podívat na [nejčastější dotazy týkající se Azure Backup](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Další kroky
Můžete získat podrobné informace [Příprava prostředí pro aplikaci DPM](https://technet.microsoft.com/library/hh758176.aspx) na webu Microsoft TechNet. Také obsahuje informace o podporovaných konfiguracích, na kterých serveru Azure Backup lze nasadit a použít.

Tyto články vám pomůže získat lepší představu o ochrana pracovního vytížení pomocí Microsoft Azure Backup server.

* [Zálohování serveru SQL Server](backup-azure-backup-sql.md)
* [Zálohování serveru SharePoint](backup-azure-backup-sharepoint.md)
* [Alternativní server zálohování](backup-azure-alternate-dpm-server.md)
