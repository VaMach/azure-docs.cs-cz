---
title: "Vzdálené připojení k zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 923377aa-f451-4656-87de-5e95a34a6a2a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec625a604660489812b1a8c133fa0156a422b605
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>Vzdálené připojení k zařízení řady StorSimple 8000
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [připojit vzdáleně na vaše zařízení řady StorSimple 8000](storsimple-8000-remote-connect.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Přehled
Vzdálená komunikace prostředí Windows PowerShell můžete použít pro připojení k zařízení StorSimple. Když připojíte tímto způsobem, neuvidíte nabídky. (Zobrazí nabídky pouze v případě, že použijete konzole sériového portu v zařízení pro připojení.) Díky vzdálenou komunikaci prostředí Windows PowerShell můžete připojit k konkrétní prostředí runspace. Můžete také zadat jazyk zobrazení. 

Další informace o používání vzdálenou komunikaci prostředí Windows PowerShell ke správě vašich zařízení, přejděte na [pomocí Windows Powershellu pro StorSimple ke správě zařízení StorSimple](storsimple-windows-powershell-administration.md).

Tento kurz vysvětluje, jak nakonfigurovat zařízení pro vzdálenou správu a jak se připojit k Windows Powershellu pro StorSimple. Připojení přes vzdálenou komunikaci prostředí Windows PowerShell můžete použít protokol HTTP nebo HTTPS. Při výběru jak se připojit k Windows Powershellu pro StorSimple, však zvažte následující: 

* Připojení přímo ke konzole sériového portu zařízení je bezpečné, ale připojující se ke konzole sériového portu přes síťové přepínače není. Buďte opatrní rizika zabezpečení při připojování ke konzole sériového portu zařízení přes síťové přepínače. 
* Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly přes síť. I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích. 
* Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je nejbezpečnější a doporučená možnost.

Můžete vzdáleně připojit k rozhraní Windows PowerShell. Ve výchozím nastavení však není povolen vzdálený přístup k zařízení StorSimple pomocí rozhraní Windows PowerShell. Je nutné nejprve povolit vzdálené správy na zařízení, a pak v klientském počítači používané pro přístup k zařízení.

Podle pokynů popsaných v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.

## <a name="connect-through-http"></a>Připojení prostřednictvím protokolu HTTP
Připojení k Windows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím konzoly sériového portu zařízení StorSimple. I když to není nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.

Konfigurace vzdálené správy můžete portál Azure classic nebo konzole sériového portu. Vyberte z následujících postupů:

* [Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure classic](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu](#use-the-serial-console-to-enable-remote-management-over-http)

Po povolení vzdálené správy pomocí následujícího postupu Příprava klienta pro vzdálené připojení.

* [Příprava klienta pro připojení ke vzdálené](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Povolení vzdálené správy přes protokol HTTP pomocí portálu Azure classic
Proveďte následující kroky na portálu Azure classic povolení vzdálené správy přes protokol HTTP.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Chcete-li povolit vzdálenou správu prostřednictvím portálu Azure classic
1. Přístup k **zařízení** > **konfigurace** pro vaše zařízení.
2. Přejděte dolů do části **Vzdálená správa**.
3. U položky **Povolit vzdálenou správu** zvolte **Ano**.
4. Teď můžete zvolit připojení pomocí protokolu HTTP. (Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.
   
   > [!NOTE]
   > Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.
   > 
   > 
5. V dolní části stránky klikněte na **Uložit**.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Povolení vzdálené správy přes protokol HTTP pomocí konzoly sériového portu
Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení
1. V nabídce konzoly sériového portu vyberte možnost 1. Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Do příkazového řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`
3. Budete informováni o ohrožení zabezpečení zabezpečení pomocí protokolu HTTP pro připojení k zařízení. Po zobrazení výzvy potvrďte zadáním **Y**.
4. Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`
5. Ověřte, zda **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Příprava klienta pro připojení ke vzdálené
Proveďte následující kroky na klientovi se povolení vzdálené správy.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Příprava klienta pro připojení ke vzdálené
1. Spusťte relaci prostředí Windows PowerShell jako správce.
2. Zadejte následující příkaz Přidat IP adresu zařízení StorSimple do seznamu důvěryhodných hostitelů klienta: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Nahraďte <*device_ip*> s IP adresou vašeho zařízení; například: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Zadejte následující příkaz pro uložení přihlašovacích údajů zařízení v proměnné: 
   
    ```
    $cred = Get-Credential
    ```
    
4. V dialogovém okně, které se zobrazí:
   
   1. Zadejte uživatelské jméno v tomto formátu: *device_ip\SSAdmin*.
   2. Zadejte heslo správce zařízení, která byla nastavena, když v zařízení byl nakonfigurovaný pomocí Průvodce instalací. Výchozí heslo je *Heslo1*.
5. Spusťte relaci prostředí Windows PowerShell na zařízení tak, že zadáte tento příkaz:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > Chcete-li vytvořit relaci prostředí Windows PowerShell pro použití s virtuální zařízení StorSimple, připojte `–Port` parametr a zadejte veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.
   > 
   > 
   
     V tomto okamžiku byste měli mít aktivní vzdálené relace prostředí Windows PowerShell na zařízení.
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Připojení přes protokol HTTPS
Připojení k Windows Powershellu pro StorSimple prostřednictvím relace HTTPS je nejbezpečnější a doporučená metoda vzdáleně připojit k zařízení s Microsoft Azure StorSimple. Následující postupy popisují, jak nastavit sériové konzoly a klientské počítače tak, že použijete protokol HTTPS pro připojení k Windows Powershellu pro StorSimple.

Konfigurace vzdálené správy můžete portál Azure classic nebo konzole sériového portu. Vyberte z následujících postupů:

* [Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure classic](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu](#use-the-serial-console-to-enable-remote-management-over-https)

Po povolení vzdálené správy pomocí následujících postupů můžete připravit hostitele pro vzdálenou správu a připojte k zařízení od vzdáleného hostitele.

* [Příprava pro vzdálenou správu hostitele](#prepare-the-host-for-remote-management)
* [Připojení k zařízení od vzdáleného hostitele](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Povolení vzdálené správy přes protokol HTTPS pomocí portálu Azure classic
Proveďte následující kroky na portálu Azure classic povolení vzdálené správy přes protokol HTTPS.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Povolení vzdálené správy přes protokol HTTPS z portálu Azure classic
1. Přístup k **zařízení** > **konfigurace** pro vaše zařízení.
2. Přejděte dolů do části **Vzdálená správa**.
3. U položky **Povolit vzdálenou správu** zvolte **Ano**.
4. Nyní můžete připojit pomocí protokolu HTTPS. (Ve výchozím nastavení se připojují přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS. 
5. Klikněte na tlačítko **stáhnout certifikát pro vzdálenou správu**. Zadejte umístění pro uložení tohoto souboru. Musíte nainstalovat tento certifikát na klientský nebo hostitelský počítač, který budete používat pro připojení k zařízení.
6. V dolní části stránky klikněte na **Uložit**.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Povolení vzdálené správy přes protokol HTTPS pomocí konzoly sériového portu
Proveďte následující kroky na konzole sériového portu zařízení povolení vzdálené správy.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Chcete-li povolit vzdálenou správu prostřednictvím konzole sériového portu zařízení
1. V nabídce konzoly sériového portu vyberte možnost 1. Další informace o používání konzoly sériového portu v zařízení, přejděte na [připojit k Windows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Do příkazového řádku zadejte: 
   
     `Enable-HcsRemoteManagement`
   
    To by měl povolit protokol HTTPS na vašem zařízení.
3. Ověřte, že je povoleno HTTPS zadáním: 
   
     `Get-HcsSystem`
   
    Ujistěte se, že **RemoteManagementMode** pole ukazuje **HttpsEnabled**. Následující obrázek znázorňuje tato nastavení v PuTTY.
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Z výstupu `Get-HcsSystem`, zkopírujte sériové číslo zařízení a uložit pro pozdější použití.
   
   > [!NOTE]
   > Sériové číslo se mapuje na název CN certifikátu.
   > 
   > 
5. Získejte certifikát pro vzdálenou správu zadáním: 
   
     `Get-HcsRemoteManagementCert`
   
    Zobrazí se certifikát podobný následujícímu.
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Zkopírujte informace v certifikátu z **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer. (Bude tento soubor je zkopírovat do vzdáleného hostitele při přípravě hostitele.)
   
   > [!NOTE]
   > Pokud chcete vygenerovat nový certifikát, použijte `Set-HcsRemoteManagementCert` rutiny.
   > 
   > 

### <a name="prepare-the-host-for-remote-management"></a>Příprava pro vzdálenou správu hostitele
Pro přípravu na hostitelském počítači vzdáleného připojení, který používá relaci protokolu HTTPS, proveďte následující postupy:

* [Importovat soubor .cer do kořenového úložiště klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).
* [Sériová čísla zařízení přidat do souboru hostitelů na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).

Každá z těchto postupů je popsána níže.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Chcete-li importovat certifikát u vzdáleného hostitele
1. Klikněte pravým tlačítkem na soubor .cer a vyberte **instalace certifikátu**. Tím se spustí Průvodce importem certifikátu.
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.
3. Vyberte **všechny certifikáty umístit v následujícím úložišti**a potom klikněte na **Procházet**. Přejděte do kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Klikněte na **Dokončit**. Zobrazí se zpráva, která vám ukáže, že import proběhl úspěšně.
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Chcete-li přidat sériová čísla zařízení se vzdáleným hostitelem
1. Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts umístěný ve \Windows\System32\Drivers\etc.
2. Přidejte následující tři položky do souboru hostitele: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.
3. Zadejte sériové číslo zařízení, který jste předtím uložili. Mapování těchto na IP adresu, jak je znázorněno na následujícím obrázku. Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci sériové číslo (CN název).
   
    ![Název CN přidání do souboru hostitelů](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Uložte soubor hostitelů.

### <a name="connect-to-the-device-from-the-remote-host"></a>Připojení k zařízení od vzdáleného hostitele
Zadejte relaci SSAdmin na zařízení ze vzdáleného hostitele nebo klienta pomocí prostředí Windows PowerShell a SSL. Relace SSAdmin se mapuje na možnost 1 v [konzoly sériového portu](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.

Následující postup proveďte v počítači, ze kterého chcete pro vzdálené připojení prostředí Windows PowerShell.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>K zadání relaci SSAdmin na zařízení pomocí prostředí Windows PowerShell a SSL
1. Spusťte relaci prostředí Windows PowerShell jako správce.
2. Přidejte IP adresu zařízení do důvěryhodných hostitelů klienta zadáním:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Kde <*device_ip*> je IP adresa vašeho zařízení, třeba: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Vytvoření nového pověření zadáním: 
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Kde <*IP cílové zařízení*> je IP adresa DATA 0 pro vaše zařízení; například **10.126.173.90** jak je vidět na předchozím obrázku souboru hostitelů. Zadejte také heslo správce pro vaše zařízení.
4. Vytvořte relaci zadáním:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Pro parametr - ComputerName do rutiny, zadejte <*sériové číslo cílového zařízení*>. Toto sériové číslo bylo mapováno na IP adresu DATA 0 v souboru hostitelů na vzdáleném hostiteli; například **SHX0991003G44MT** jak je znázorněno na následujícím obrázku.
5. Zadejte: 
   
     `Enter-PSSession $session`
6. Budete muset několik minut počkat a potom budete připojeni do zařízení pomocí protokolu HTTPS přes protokol SSL. Zobrazí se zpráva, která určuje, že jste připojeni k zařízení.
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [pomocí prostředí Windows PowerShell ke správě zařízení StorSimple](storsimple-windows-powershell-administration.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

