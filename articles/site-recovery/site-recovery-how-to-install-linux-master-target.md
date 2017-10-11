---
title: "Jak nainstalovat Linux hlavní cílový server pro převzetí služeb při selhání z Azure do místní | Microsoft Docs"
description: "Před opětovnou ochranu virtuální počítač s Linuxem, potřebujete hlavní cílový server Linux. Zjistěte, jak k jeho instalaci."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 5341e3e56e0c366079958dd9a885f6ee3e8436cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-a-linux-master-target-server"></a>Instalovat hlavní cílový server Linux
Po selhání virtuálních počítačů, můžete můžete navrácení služeb po obnovení virtuálních počítačů k místní lokalitě. Chcete-li navrácení služeb po obnovení, je potřeba znovu nastavte ochranu virtuálního počítače z Azure do místní lokality. Pro tento proces budete potřebovat místní hlavní cílový server příjem provozu. 

Pokud chráněný virtuální počítač je virtuálního počítače s Windows, musíte Windows hlavní cíl. Pro virtuální počítač s Linuxem budete potřebovat hlavního cíle Linuxu. Přečtěte si následující kroky a zjistěte, jak vytvořit a nainstalovat hlavního cíle Linuxu.

> [!IMPORTANT]
> Od verze 9.10.0 hlavní cílový server, nejnovější hlavní cílový server můžete nainstalovat jenom na Ubuntu 16.04 server. Nové instalace nejsou povoleny u CentOS6.6 servery. Však můžete nadále upgradu vaší starého hlavního cílové servery pomocí 9.10.0 verze.

## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny k instalaci hlavního cíle Linuxu.

POST dotazy nebo připomínky můžete na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky

* Zvolte hostitele, do které chcete nasadit na hlavním cíli, určete, pokud navrácení služeb po obnovení bude do existující virtuální počítač místně nebo do nového virtuálního počítače. 
    * Pro existující virtuální počítač hostitele hlavního cíle mají mít přístup k úložišti dat virtuálního počítače.
    * Pokud na místním virtuálním počítači neexistuje, je navrácení služeb po obnovení virtuálního počítače vytvořit na stejném hostiteli jako hlavní cíl. Můžete vybrat libovolného hostitele ESXi k instalaci na hlavním cíli.
* Hlavní cíl musí být v síti, který může komunikovat s procesovým serverem a konfigurační server.
* Verze hlavního cíle musí být rovna nebo starší než verze procesového serveru a konfigurační server. Například pokud je verze konfigurace serveru 9.4, verze hlavního cíle může být 9.4 nebo 9.3, ale není 9.5.
* Na hlavním cíli lze pouze virtuální počítač VMware, nikoli na fyzický server.

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a>Vytvoření hlavního cíle podle pokynů pro změnu velikosti

Vytvoření hlavního cíle podle následujících pokynů pro změnu velikosti:
- **Paměť RAM**: 6 GB nebo více
- **Velikost disku operačního systému**: 100 GB nebo více (pro instalaci CentOS6.6)
- **Velikost disku Další jednotka pro uchování**: 1 TB
- **Jader procesoru**: 4 jádra nebo více

Jsou podporovány následující podporované Ubuntu jádra.


|Řada jádra  |Podporovat až  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>Nasazení hlavního cílového serveru

### <a name="install-ubuntu-16042-minimal"></a>Nainstalujte Ubuntu 16.04.2 minimální

Proveďte následující kroky pro instalaci Ubuntu 16.04.2 64bitový operační systém.

**Krok 1:** přejít na [stáhnout odkaz](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) a zvolte nejbližší zrcadlení z které stáhnout soubor ISO Ubuntu 16.04.2 minimální 64-bit.

Ponechte soubor ISO Ubuntu 16.04.2 minimální 64-bit do jednotky DVD a spuštění systému.

**Krok 2:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.

![Výběr jazyka](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Krok 3:** vyberte **nainstalovat Ubuntu Server**a potom vyberte **Enter**.

![Vyberte možnost instalace Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Krok 4:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.

![Vyberte angličtina jako upřednostňovaný jazyk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Krok 5:** vyberte příslušnou možnost **časové pásmo** seznam možností a potom vyberte **Enter**.

![Vyberte správné časové pásmo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Krok 6:** vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.


![Konfigurace klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Krok 7:** vyberte **angličtinu (US)** jako země původu klávesnice, a potom vyberte **Enter**.

![Vyberte USA jako země původu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Krok 8:** vyberte **angličtinu (US)** rozložení klávesnice a pak vyberte **Enter**.

![Vyberte angličtinu jako rozložení klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Krok 9:** zadejte název hostitele pro server v **Hostname** a pak vyberte **pokračovat**.

![Zadejte název hostitele pro server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Krok 10:** vytvoření uživatelského účtu, zadejte uživatelské jméno a potom vyberte **pokračovat**.

![Vytvoření uživatelského účtu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Krok 11:** zadejte heslo pro nový uživatelský účet a potom vyberte **pokračovat**.

![Zadejte heslo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Krok 12:** potvrďte heslo pro nového uživatele a pak vyberte **pokračovat**.

![Potvrzení hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Krok 13:** vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.

![Nastavit uživatele a hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Krok 14:** Pokud časové pásmo, které se zobrazí správný, vyberte **Ano** (výchozí možnost) a potom vyberte **Enter**.

Chcete-li překonfigurovat časové pásmo, vyberte **ne**.

![Nakonfigurujte hodiny](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Krok 15:** dělicí metody možnosti, vyberte **na základě - použít celý disk**a potom vyberte **Enter**.

![Vyberte možnost použít pro dělicí metody](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Krok 16:** vyberte příslušný disk z **vyberte disk do oddílu** možnosti a pak vyberte **Enter**.


![Vyberte disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Krok 17:** vyberte **Ano** k zápisu změn na disk a potom vyberte **Enter**.

![Zápis změn na disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Krok 18:** vyberte možnost výchozí, vyberte **pokračovat**a potom vyberte **Enter**.

![Vyberte možnost výchozí](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Krok 19:** vyberte příslušnou možnost pro správu upgrady systému a pak vyberte **Enter**.

![Vyberte, jak spravovat upgrady](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Protože Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi Ubuntu, musíte zajistit, aby jádra zakázali upgrady pro virtuální počítač. Pokud se povolí, nějaké regulární upgrady způsobit hlavní cílový server fungovat správně. Zkontrolujte, zda jste vybrali **žádné automatické aktualizace** možnost.


**Krok 20:** vyberte výchozí možnosti. Pokud chcete openSSH pro připojení SSH, vyberte **OpenSSH server** a pak vyberte možnost **pokračovat**.

![Vyberte software](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Krok 21:** vyberte **Ano**a potom vyberte **Enter**.

![Isntall spouštěcí zavaděč GRUB](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Krok 22:** vyberte odpovídající zařízení, pro instalaci spouštěcí zavaděč (pokud možno **/dev/sda**) a potom vyberte **Enter**.

![Vyberte zařízení, které spouštěcí zavaděč instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Krok 23:** vyberte **pokračovat**a potom vyberte **Enter** k dokončení instalace.

![Dokončení instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Po dokončení instalace, přihlaste se k virtuálnímu počítači s novými pověřeními uživatele. (Odkazovat na **krok 10** Další informace.)

Proveďte kroky, které jsou popsány v následující snímek obrazovky nastavení hesla uživatele ROOT. Přihlaste se jako KOŘENOVÉ uživatele.

![Nastavení KOŘENOVÉ heslo uživatele](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a>Příprava pro konfiguraci počítače jako hlavní cílový server
V dalším kroku Příprava počítače pro konfiguraci jako hlavní cílový server.

Chcete-li získat ID pro každý SCSI pevný disk ve virtuálním počítači Linux, povolte **disku. EnableUUID = TRUE** parametr.

Chcete-li tento parametr, proveďte následující kroky:

1. Vypněte virtuální počítač.

2. Pravým tlačítkem na položku pro virtuální počítač v levém podokně a pak vyberte **upravit nastavení**.

3. Vyberte **možnosti** kartě.

4. V levém podokně vyberte **Upřesnit** > **Obecné**a pak vyberte **parametry konfigurace** tlačítko v pravé dolní části obrazovky.

    ![Karta Možnosti](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    **Parametry konfigurace** možnost není k dispozici, když je počítač spuštěn. Chcete-li na této kartě aktivní, vypněte virtuální počítač.

5. Zda řádek s **disku. EnableUUID** již existuje.

    - Pokud hodnota existuje a je nastavená na **False**, změňte hodnotu na **True**. (Hodnoty nejsou malá a velká písmena.)

    - Pokud hodnota existuje a je nastavená na **True**, vyberte **zrušit**.

    - Pokud hodnota neexistuje, vyberte **přidat řádek**.

    - Ve sloupci Název přidat **disku. EnableUUID**a pak nastavte hodnotu na **TRUE**.

    ![Kontroluje, zda disk. EnableUUID již existuje.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Zakázat upgrady jádra

Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi Ubuntu, zkontrolujte, zda jsou pro virtuální počítač vypnutá upgrady jádra.

Pokud upgrady jádra jsou povolené, jakékoli regulární upgrady způsobit hlavní cílový server fungovat správně.

#### <a name="download-and-install-additional-packages"></a>Stáhněte a nainstalujte další balíčky

> [!NOTE]
> Ujistěte se, že máte připojení k Internetu stáhněte a nainstalujte další balíčky. Pokud nemáte připojení k Internetu, budete muset ručně najít tyto balíčky ot. / min a nainstalovat je.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a>Získání Instalační program pro instalaci

Pokud hlavní cíl má připojení k Internetu, můžete použít následující kroky stáhnout instalační program. Jinak můžete zkopírujte instalační službu z procesového serveru a nainstalujte ji.

#### <a name="download-the-master-target-installation-packages"></a>Stáhněte si instalační balíčky hlavní cíl

[Stáhněte si nejnovější bits instalace hlavní cíl Linux](https://aka.ms/latestlinuxmobsvc).

Chcete-li stáhnout ji pomocí Linux, zadejte:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Ujistěte se, stáhněte a rozbalte instalační program v domovském adresáři. Pokud rozbalte k **/usr/místní**, instalace selže.


#### <a name="access-the-installer-from-the-process-server"></a>Přístup k Instalační program z procesového serveru

1. Na procesní server, přejděte na **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Instalační program vyžaduje soubor zkopírovat z procesového serveru a uložte ho jako **latestlinuxmobsvc.tar.gz** v domovském adresáři.


### <a name="apply-custom-configuration-changes"></a>Změny vlastní konfigurace

Chcete-li použít změny v vlastní konfigurace, použijte následující kroky:


1. Spusťte následující příkaz, který untar binárního souboru.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Snímek obrazovky příkaz ke spuštění](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Spusťte následující příkaz, který udělit oprávnění.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Spusťte následující příkaz pro spuštění skriptu.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Spusťte skript jenom jednou na serveru. Vypněte server. Restartujte server po přidat disk, jak je popsáno v následující části.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Přidejte uchování disk k virtuálnímu počítači hlavního cíle Linuxu

Pomocí následujících kroků můžete vytvořit disku pro uchování:

1. Připojit nový disk 1 TB k virtuálnímu počítači hlavního cíle Linuxu a potom počítač spustit.

2. Použití **vícenásobný -udou** příkaz Další funkce multipath ID disku pro uchování.

    ```
    multipath -ll
    ```
    ![Vícenásobný ID disku pro uchování](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formátování disku a pak vytvořit systém souborů na nový disk.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Vytvoření systému souborů na disku](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Po vytvoření systému souborů, připojte disk uchovávání informací.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Připojení disku pro uchování](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Vytvořte **fstab** položka připojit jednotka pro uchování pokaždé, když bude systém při spuštění.
    ```
    vi /etc/fstab
    ```
    Vyberte **vložit** zahájíte úpravy souboru. Vytvořte nový řádek a potom vložte následující text. Upravte vícenásobný ID disku na základě Identifikátoru zvýrazněná více cest z předchozí příkaz.

     **/dev/mapper/ <Retention disks multipath id> /mnt/uchování ext4 rw 0 0**

    Vyberte **Esc**a pak zadejte **: QW** (zápisu a ukončení) zavřete okno editor.

### <a name="install-the-master-target"></a>Nainstalujte na hlavním cíli

> [!IMPORTANT]
> Verze hlavního cílového serveru musí být rovna nebo starší než verze procesového serveru a konfigurační server. Pokud není tato podmínka splněná, opětovné ochrany úspěšná, ale replikace selže.


> [!NOTE]
> Než nainstalujete hlavní cílový server, zkontrolujte, zda **/etc/hosts** souboru na virtuální počítač obsahuje položky, které mapují názvem místního hostitele na IP adresy, které jsou přidruženy všechny síťové adaptéry.

1. Kopírovat heslo z **C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase** na konfiguračním serveru. Potom uložte ho jako **passphrase.txt** ve stejném adresáři místní spuštěním následujícího příkazu:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Příklad: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Všimněte si, IP adresa konfiguračního serveru. Budete ho potřebovat v dalším kroku.

3. Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Příklad: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Počkejte na dokončení skriptu. Pokud se hlavní cíl zaregistruje úspěšně, se hlavní cíl je uvedený na **infrastruktura Site Recovery** stránce portálu.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Instalaci se hlavní cíl pomocí interaktivní instalace

1. Spusťte následující příkaz k instalaci na hlavním cíli. Pro roli agenta, vyberte **hlavního cíle**.

    ```
    ./install
    ```

2. Vyberte výchozí umístění pro instalaci a potom vyberte **Enter** pokračujte.

    ![Volba výchozí umístění pro instalaci hlavní cíl](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Po dokončení instalace zaregistrujte konfigurační server pomocí příkazového řádku.

1. Všimněte si IP adresu konfiguračního serveru. Budete ho potřebovat v dalším kroku.

2. Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Příklad: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Počkejte na dokončení skriptu. Pokud se hlavní cíl je úspěšně registrovaná, se hlavní cíl je uvedený na **infrastruktura Site Recovery** na portálu.


### <a name="upgrade-the-master-target"></a>Upgrade na hlavním cíli

Spusťte instalační program. Automaticky zjišťuje, zda je agent nainstalovaný na hlavním cíli. Pokud chcete upgradovat, vyberte **Y**.  Po dokončení instalace, zkontrolujte verzi hlavního cíle nainstalován pomocí následujícího příkazu.

    ```
    cat /usr/local/.vx_version
    ```

Uvidíte, že **verze** pole obsahuje číslo verze se hlavní cíl.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Nainstalujte nástroje VMware na hlavním cílovém serveru

Musíte nainstalovat nástroje VMware na hlavním cíli, aby ho můžete zjistit datová úložiště. Pokud nejsou nainstalovány nástroje, není v úložištích dat, uvedené na obrazovce opětovné ochrany. Po instalaci nástroje VMware je potřeba restartovat.

## <a name="next-steps"></a>Další kroky
Po instalaci a registraci hlavního cíle má finsihed, zobrazí se zobrazí na hlavním cíli **hlavního cíle** kapitoly **infrastruktura Site Recovery**, v části Přehled konfigurace serveru.

Teď můžete pokračovat s [vytvoření](site-recovery-how-to-reprotect.md), za nímž následují navrácení služeb po obnovení.

## <a name="common-issues"></a>Běžné problémy

* Ujistěte se, že jste nezapínejte řešení Storage vMotion na žádné součásti správy, jako je hlavní cíl. Pokud se hlavní cíl přesune po úspěšné opětovné ochrany, disků virtuálního počítače (VMDKs) nelze odpojit. V takovém případě navrácení služeb po obnovení selže.

* Hlavní cíl by neměl mít všechny snímky na virtuálním počítači. Pokud existují snímky, navrácení služeb po obnovení se nezdaří.

* Z důvodu některých vlastních konfigurací síťový adaptér v někteří zákazníci síťové rozhraní je zakázané během spouštění a nemůže inicializovat agenta hlavní cíl. Ujistěte se, že následující vlastnosti jsou správně nastaveny. Zkontrolujte tyto vlastnosti v Ethernet karty souboru /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = Ano
