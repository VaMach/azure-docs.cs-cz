---
title: 
description: 
services: site-recovery
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 4871e5959483058aa957e3373ddc5121b398e1d9
ms.openlocfilehash: 1d6d595bf788fc3fdba0bbc98887614e17c791dc


---



### <a name="install-the-mobility-service"></a>Instalace služby Mobility
Prvním krokem při aktivaci ochrany virtuálních počítačů a fyzických serverů je instalace služby Mobility. Můžete to provést několika způsoby:

* **Nabízená oznámení procesového serveru:** Při aktivaci replikace na počítači přijměte nabízené oznámení a nainstalujte komponentu služby Mobility z procesového serveru. 
*Poznámka:* K nabízené instalaci nedojde, pokud na počítačích již běží aktuální verze komponenty.
* **Nabízená oznámení podniku:** Automaticky nainstalujte komponentu pomocí procesu nabízených oznámení vašeho podniku, jako je WSUS, System Center Configuration Manager nebo [Azure Automation a požadovaná konfigurace stavu](site-recovery-automate-mobility-service-install.md). Předtím, než to provedete, nastavte konfigurační server.
* **Ruční instalace:** Nainstalujte komponentu ručně na každý počítač, který chcete replikovat. Předtím, než to provedete, nastavte konfigurační server.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Příprava pro automatické nabízené oznámení na počítačích s Windows
Zde je postup přípravy počítačů s Windows pro automatickou instalaci služby Mobility procesovým serverem.

1. Vytvořte účet, pomocí kterého bude procesový server moct přistupovat k počítači. Účet musí mít oprávnění správce (místního správce nebo správce domény) a měl by sloužit pouze pro nabízenou instalaci.

   > [!NOTE]
   > Pokud nepoužíváte doménový účet, je nutné na místním počítači zakázat vzdálené řízení přístupu uživatele. Provedete to tak, že v registru pod položku HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System přidáte položku DWORD LocalAccountTokenFilterPolicy s hodnotou 1. Pokud chcete přidat položku registru z rozhraní příkazového řádku, zadejte příkaz **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.
   >
   >
2. V bráně Windows Firewall počítače, pro který chcete nastavit ochranu, vyberte **Povolit aplikaci nebo funkci průchod bránou Firewall**. Povolte **Sdílení souborů a tiskáren** a **rozhraní WMI (Windows Management Instrumentation)**. Pro počítače, které patří do nějaké domény, můžete nakonfigurovat nastavení brány firewall pomocí objektu zásad skupiny.

   ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Přidejte účet, který jste vytvořili:

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěný ve složce [UMÍSTĚNÍ INSTALACE]\home\svsystems\bin.
   * Na kartě **Správa účtů** klikněte na **Přidat účet**.
   * Přidejte účet, který jste vytvořili. Po přidání účtu je při aktivaci replikace pro počítač nutné zadat přihlašovací údaje.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Příprava pro automatické nabízené oznámení na serverech s Linuxem
1. Ověřte, že počítač s Linuxem, pro který chcete nastavit ochranu, je podporovaný, jak je popsáno v části s [požadavky na chráněný počítač](#protected-machine-prerequisites). Zkontrolujte, že existuje síťové připojení mezi počítačem s Linuxem a procesovým serverem.
2. Vytvořte účet, pomocí kterého bude procesový server moct přistupovat k počítači. Účet musí být na zdrojovém serveru s Linuxem uživatelem root, a měl by sloužit pouze pro nabízenou instalaci.

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše a umístěný ve složce [UMÍSTĚNÍ INSTALACE]\home\svsystems\bin.
   * Na kartě **Správa účtů** klikněte na **Přidat účet**.
   * Přidejte účet, který jste vytvořili. Po přidání účtu je při aktivaci replikace pro počítač nutné zadat přihlašovací údaje.
3. Zkontrolujte, že soubor /etc/hosts na zdrojovém serveru s Linuxem obsahuje položky, které mapují místní název hostitele na IP adresy přidružené ke všem síťovým adaptérům.
4. Na počítači, který chcete replikovat, nainstalujte nejnovější balíčky openssh, openssh-server a openssl.
5. Ujistěte se, že je povolený protokol SSH, a že běží na portu 22.
6. V souboru sshd_config povolte následujícím způsobem podsystém SFTP a ověřování heslem:

   * Přihlaste se jako uživatel root.
   * V souboru /etc/ssh/sshd_config vyhledejte řádek, který začíná na **PasswordAuthentication**.
   * Zrušte na řádku komentář a změňte hodnotu z **no** na **yes**.
   * Vyhledejte řádek, který začíná na **Subsystem** a zrušte na něm komentář.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Ruční instalace služby Mobility
Instalační programy jsou dostupné na konfiguračním serveru v umístění **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

| Zdrojový operační systém | Instalační soubor služby Mobility |
| --- | --- |
| Windows Server (pouze&64;bitová verze) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (pouze 64bitové verze) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitová verze) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitová verze) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Instalace služby Mobility na Windows Server
1. Stáhněte a spusťte odpovídající instalační program.
2. V části **Než začnete** vyberte **Služba Mobility**.

    ![Služba Mobility](./media/site-recovery-vmware-to-azure/mobility3.png)
3. V části **Podrobnosti konfiguračního serveru** zadejte IP adresu konfiguračního serveru a heslo, které se vygenerovalo při spuštění sjednocené instalace. Heslo můžete načíst spuštěním **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v** na konfiguračním serveru.

    ![Služba Mobility](./media/site-recovery-vmware-to-azure/mobility6.png)
4. V části **Umístění instalace** ponechejte výchozí nastavení a kliknutím na **Další** zahajte instalaci.
5. V části **Průběh instalace** sledujte instalaci a až se zobrazí výzva, restartujte počítač. Po nainstalování služby může aktualizace stavu na portálu trvat přibližně 15 minut.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Instalace služby Mobility na Windows Server pomocí rozhraní příkazového řádku
1. Zkopírujte instalační soubor do místní složky (třeba C:\Temp) na serveru, pro který chcete nastavit ochranu. Instalační program najdete na konfiguračním serveru v **[umístění instalace]\home\svsystems\pushinstallsvc\repository**. Balíček pro operační systémy Windows se jmenuje podobně jako Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe.
2. Přejmenujte tento soubor na MobilitySvcInstaller.exe.
3. Spusťte následující příkaz, který extrahuje instalační službu MSI:

    ``C:\> cd C:\Tempww
    ``C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted``
    ``C:\Temp> cd Extracted``
    ``C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP adresa konfiguračního serveru" /PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Úplná syntaxe příkazového řádku
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parametry**

* **/Role:** Povinný. Určuje, jestli se má nainstalovat služba Mobility. Vstupní hodnoty: Agent/MasterTarget
* **/InstallLocation:** Povinný. Určuje, kam se má služba nainstalovat.
* **/PassphraseFilePath:** Povinný. Heslo konfiguračního serveru.
* **/LogFilePath:** Povinný. Umístění, kde se mají vytvářet soubory protokolu instalace.

#### <a name="uninstall-the-mobility-service-manually"></a>Ruční odinstalace služby Mobility
Službu Mobility můžete odinstalovat pomocí nabídky Přidat nebo odebrat programy v Ovládacích panelech nebo pomocí instrukce příkazového řádku: MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="install-the-mobility-service-on-a-linux-server"></a>Instalace služby Mobility na serveru s Linuxem
1. Zkopírujte příslušný archiv tar, podle výše uvedené tabulky, na počítač s Linuxem, který chcete replikovat.
2. Otevřete program prostředí a extrahujte komprimovaný archiv tar do místní cesty spuštěním příkazu: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. V místním adresáři, do kterého jste extrahovali obsah archivu tar, vytvořte soubor passphrase.txt. Provedete to tak, že zkopírujete heslo ze souboru C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase na konfiguračním serveru a uložíte ho do souboru passphrase.txt spuštěním příkazu *`echo <passphrase> >passphrase.txt`* v prostředí.
4. Nainstalujte službu Mobility spuštěním příkazu *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Zadejte interní IP adresu konfiguračního serveru a ujistěte se, že je vybrán port 443. Po nainstalování služby může aktualizace stavu na portálu trvat přibližně 15 minut.

**Instalaci můžete provést také z příkazového řádku:**

Zkopírujte heslo ze souboru C:\Program Files (x86)\InMage Systems\private\connection na konfiguračním serveru a uložte ho jako soubor passphrase.txt na konfiguračním serveru. Potom spusťte tyto příkazy. V našem příkladu je IP adresa konfiguračního serveru 104.40.75.37 a port HTTPS by měl být 443:


Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Instalace na hlavním cílovém serveru:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt






<!--HONumber=Feb17_HO3-->


