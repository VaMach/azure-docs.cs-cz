---
title: "Vytvoření a instalace souborů konfigurace klienta VPN pro připojení P2S RADIUS: prostředí PowerShell: Azure | Microsoft Docs"
description: "Vytvoření klienta Windows a Mac OS X, Linux VPN konfigurační soubory pro připojení, které používají ověřování pomocí protokolu RADIUS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: ce914d2fd0472855ad7a17bf64ae43a76ceb5743
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Vytvoření a instalace souborů konfigurace klienta VPN pro ověřování pomocí protokolu RADIUS P2S

Připojení k virtuální síti přes Point-to-Site, musíte nakonfigurovat klientské zařízení, ze kterého budete připojovat. Můžete vytvořit připojení k síti VPN P2S z Windows, Mac OSX a Linux klientských zařízení. Při použití ověřování pomocí protokolu RADIUS, existuje několik možností ověřování: ověřování uživatelského jména a hesla, ověřování pomocí certifikátu, jakož i jiné typy ověřování. Konfigurace klienta VPN se liší pro jednotlivé typy ověřování. Ke konfiguraci klienta VPN, je použít klienta konfigurační soubory, které obsahují požadovaná nastavení. Tento článek vám pomůže vytvořit a instalovat konfigurace klienta VPN pro typ ověřování RADIUS, který chcete použít.

Pracovní postup konfigurace ověřování P2S RADIUS vypadá takto:

1. [Nastaví bránu Azure VPN pro připojení P2S](point-to-site-how-to-radius-ps.md).
2. [Nastavit server RADIUS pro ověřování](point-to-site-how-to-radius-ps.md#radius). 
3. **Konfigurace klienta VPN pro možnost ověřování podle svého výběru získat a použít ho k nastavení klienta VPN**. (V tomto článku)
4. [Dokončete konfiguraci P2S a připojte](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Je-li vygenerovat profil konfigurace klienta VPN, jako je například protokol typ sítě VPN nebo typ ověřování, se změny v konfiguraci Point-to-Site VPN musí vygenerovat a nainstalujte nové konfigurace klienta VPN v zařízeních uživatele.
>
>

Pokud chcete použít v částech v tomto článku, nejprve rozhodnout, jaký typ ověřování, kterou chcete použít: uživatelské jméno a heslo, certifikát nebo další typy ověřování. V každém oddílu jsou kroky pro Windows, Mac OS X a Linux (nyní k dispozici omezená kroky).

## <a name="adeap"></a>Ověřování uživatelského jména a hesla

Existují dva způsoby konfigurace ověřování uživatelským jménem a heslem. Buď můžete nakonfigurovat ověřování používat AD, nebo ověřit bez použití AD. Buď scénář Ujistěte se, aby všichni uživatelé připojující mít přihlašovací údaje uživatelského jména a hesla, které lze ověřit pomocí protokolu RADIUS.

* Když nakonfigurujete ověřování uživatelského jména a hesla, můžete vytvořit pouze konfiguraci pro ověřovací protokol EAP-MSCHAPv2 uživatelského jména a hesla.
* ' – AuthenticationMethod' je 'EapMSChapv2'.

### <a name="usernamefiles"></a> 1. Generování souborů konfigurace klienta VPN

Generování souborů konfigurace klienta VPN pro použití s ověřováním uživatelského jména a hesla. Můžete vygenerovat soubory konfigurace klienta VPN pomocí následujícího příkazu:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Spuštěním příkazu vrátí odkaz. Zkopírujte a vložte odkaz do webového prohlížeče ke stažení, VpnClientConfiguration.zip'. Rozbalte soubor zobrazíte následující složky: 
 
* **WindowsAmd64** a **WindowsX86** -tyto složky obsahují balíčky Instalační služby systému Windows 64bitové a 32bitové v uvedeném pořadí. 
* **Obecné** – tato složka obsahuje obecné informace použít k vytvoření vlastní konfigurace klienta VPN. Tato složka není vyžadován pro ověření konfigurace uživatelského jména a hesla.
* **Mac** -IKEv2 Pokud byla nakonfigurována, když vytvoříte bránu virtuální sítě, najdete v části složku s názvem "Mac, který obsahuje **mobileconfig** souboru. Tento soubor se používá ke konfiguraci klienti se systémem Mac.

Pokud jste již vytvořili klienta konfigurační soubory, můžete je načíst pomocí rutiny 'Get-AzureRmVpnClientConfiguration'. Ale pokud provedete změny konfiguraci P2S VPN, jako je například protokol VPN typ nebo typ ověřování, konfigurace nebude aktualizovat automaticky. Musíte spustit rutinu, New-AzureRmVpnClientConfiguration' Chcete-li vytvořit nové konfigurace stahování.

Pokud chcete načíst dříve generovaného klienta konfigurační soubory, použijte následující příkaz:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. Konfigurace klientů VPN

Můžete nakonfigurovat následující klienti VPN:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Linux pomocí strongSwan](#adlinuxcli)
 
#### <a name="adwincli"></a>Instalace klienta VPN ve Windows

Na každém klientském počítači s Windows, můžete použít stejný balíček konfigurace klienta VPN, tak dlouho, dokud verze odpovídá architektuře pro klienta. Seznam klientské operační systémy, které jsou podporované, najdete v části Point-to-Site [– nejčastější dotazy](vpn-gateway-vpn-faq.md#P2S).

Pomocí následujícího postupu můžete nakonfigurovat Nativní klient VPN ve Windows pro ověření certifikátu:

1. Vyberte konfigurační soubory klienta VPN, které odpovídá architektuře počítače Windows. Pro 64bitový procesor architekturu vyberte balíček Instalační služby 'VpnClientSetupAmd64'. Pro architekturu procesoru 32-bit vyberte balíček Instalační služby 'VpnClientSetupX86'. 
2. Chcete-li nainstalovat balíček, dvakrát klikněte na. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje. 

#### <a name="admaccli"></a>Instalace klienta VPN MAC (OS X)

1. Vyberte **VpnClientSetup mobileconfig** souboru a jeho odeslání jednotlivých uživatelů. Můžete vytvořit e-mailu nebo jiným způsobem.

2. Vyhledejte **mobileconfig** souboru na Mac.

  ![Vyhledejte soubor mobilconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Dvakrát klikněte na profil, který se ji nainstalovat, klikněte na tlačítko **pokračovat**. Název profilu je stejný jako název sítě vnet.

  ![Instalace](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Klikněte na tlačítko **pokračovat** vytvoříte vztah důvěryhodnosti odesílatel profilu a pokračujte v instalaci.

  ![pokračovat](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Během instalace profilu máte možnost zadat uživatelské jméno a heslo použité pro ověřování sítě VPN. Není nutné zadat tyto informace. -Li zadána, je informace uložit a automaticky používá při inicializaci připojení. Klikněte na tlačítko **nainstalovat** pokračovat.

  ![settings](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Zadejte uživatelské jméno a heslo pro potřebná oprávnění potřebná k instalaci profilu ve vašem počítači. Klikněte na **OK**.

  ![uživatelské jméno a heslo](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Po instalaci profilu se zobrazí na **profily** dialogové okno. Tento dialog můžete také otevřít později z **předvolbách systému**.

  ![předvolbách systému](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Chcete-li získat přístup k připojení k síti VPN, otevřete **sítě** dialogové okno z **předvolbách systému**.

  ![Sítě](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. Připojení k síti VPN se zobrazuje jako **IkeV2 VPN**. Název může změnit aktualizace **mobileconfig** souboru.

  ![připojení](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Klikněte na tlačítko **nastavení ověřování**. Zvolte **uživatelské jméno** v rozevíracím seznamu a zadejte svá pověření. Pokud jste zadali přihlašovací údaje dříve, pak **uživatelské jméno** je automaticky vybrat v rozevírací nabídky a uživatelské jméno a heslo jsou předem vyplněné. Kliknutím na **OK** uložte nastavení. Tím přejdete zpět do dialogového okna sítě.

  ![ověření](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Klikněte na tlačítko **použít** a uložte změny. Chcete-li iniciovat připojení, klikněte na tlačítko **Connect**.

#### <a name="adlinuxcli"></a>Nastavení klienta Linux VPN pomocí strongSwan

Podle následujících pokynů byly vytvořeny pomocí strongSwan 5.5.1 na Ubuntu 17.0.4. Skutečné obrazovky se může lišit v závislosti na vaší verzí systémů Linux a strongSwan.

1. Otevřete **Terminálové** k instalaci **strongSwan** a jeho správce sítě spuštěním příkazu v příkladu. Pokud se zobrazí chyba týkající se "libcharon navíc-modulů plug-in", nahraďte ji metodou "strongswan-plugin-eap-mschapv2".

  ```Terminal
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Klikněte **správce sítě** ikona (šipka nebo nižší šipku nahoru) a vyberte **upravit připojení**.

  ![Upravit připojení](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Klikněte **přidat** tlačítko vytvořte nové připojení.

  ![Přidat připojení](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Vyberte **protokolu IPsec nebo IKEv2 (strongswan)** v rozevírací nabídce klikněte **vytvořit**. Můžete přejmenovat připojení v tomto kroku.

  ![Přidat ikev2](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Otevřete **VpnSettings.xml** souboru z **Obecné** složky stažené klienta konfigurační soubory. Nalézt značku názvem **VpnServer** a zkopírujte název, počínaje "azuregateway" a končící ". cloudapp.net".

  ![nastavení sítě VPN](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Vložte tento název do **adresu** pole nové připojení VPN v části **brány** části. Potom klikněte na ikonu složky na konci **certifikát** pole, přejděte do složky, obecné a vyberte **VpnServerRoot** soubor umístěný existuje.
7. V části **klienta** části připojení, zvolte **EAP** pro **ověřování**a zadejte uživatelské jméno a heslo. Možná budete muset vyberte ikonu zámku na pravé straně uložit tyto informace. Potom klikněte na **Uložit**.

  ![Upravit nastavení připojení](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Klikněte **správce sítě** ikona (šipka nebo nižší šipku nahoru) a hover přes **připojení k síti VPN**. Zobrazí se připojení k síti VPN, který jste vytvořili. Chcete-li iniciovat připojení, vyberte připojení.

  ![připojení radius](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Ověřování pomocí certifikátu
 
Klient VPN můžete vytvořit konfigurační soubory pro ověřování protokolu RADIUS certifikát, který používá protokol EAP-TLS. Certifikát vydaný Enterprise se obvykle používá k ověření uživatele pro síť VPN. Ujistěte se, zda všichni uživatelé připojující certifikát nainstalovaný na zařízení uživatelů, a že může být ověřen certifikát serveru RADIUS.
 
* ' – AuthenticationMethod' je 'EapTls'.
* Při ověřování certifikátu klienta ověří RADIUS server ověřením svůj certifikát. -RadiusRootCert je soubor .cer obsahující kořenový certifikát, který se používá k ověření serveru RADIUS.
* Každé zařízení klient VPN vyžaduje certifikát nainstalovaného klienta.
* V některých případech zařízení se systémem Windows má více klientských certifikátů. Při ověřování to může způsobit v automaticky otevřeném okně. dialog výpis všech certifikátů. Uživatel pak musí vybrat certifikát, který chcete použít. Správný certifikát můžete odfiltrovat zadáním kořenový certifikát, ke které by měly být zřetězené klientského certifikátu. '-ClientRootCert, je soubor .cer, který obsahuje kořenový certifikát. Je volitelný parametr. Pokud zařízení, ze kterého se chcete připojit má jen jeden klientský certifikát, pak tento parametr není nutné zadávat.

### <a name="certfiles"></a>1. Generování souborů konfigurace klienta VPN

Generování souborů konfigurace klienta VPN pro použití s ověřováním pomocí certifikátu. Můžete vygenerovat soubory konfigurace klienta VPN pomocí následujícího příkazu:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Spuštěním příkazu vrátí odkaz. Zkopírujte a vložte odkaz do webového prohlížeče ke stažení, VpnClientConfiguration.zip'. Rozbalte soubor zobrazíte následující složky:

* **WindowsAmd64** a **WindowsX86** -tyto složky obsahují balíčky Instalační služby systému Windows 64bitové a 32bitové v uvedeném pořadí. 
* **GenericDevice** – tato složka obsahuje obecné informace použít k vytvoření vlastní konfigurace klienta VPN.

Pokud jste již vytvořili klienta konfigurační soubory, můžete je načíst pomocí rutiny 'Get-AzureRmVpnClientConfiguration'. Ale pokud provedete změny konfiguraci P2S VPN, jako je například protokol VPN typ nebo typ ověřování, konfigurace nebude aktualizovat automaticky. Musíte spustit rutinu, New-AzureRmVpnClientConfiguration' Chcete-li vytvořit nové konfigurace stahování.

Pokud chcete načíst dříve generovaného klienta konfigurační soubory, použijte následující příkaz:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. Konfigurace klientů VPN

Můžete nakonfigurovat následující klienti VPN:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (nepodporuje žádné článku kroky ještě)

#### <a name="certwincli"></a>Instalace klienta VPN ve Windows

1. Vyberte konfigurační balíček a nainstalujte ho v klientském zařízení. Pro 64bitový procesor architekturu vyberte balíček Instalační služby 'VpnClientSetupAmd64'. Pro architekturu procesoru 32-bit vyberte balíček Instalační služby 'VpnClientSetupX86'. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**. Můžete také balíček uložit k instalaci na další klientské počítače.
2. Každý klient vyžaduje klientský certifikát pro ověřování. Nainstalujte certifikát klienta. Informace o klientských certifikátů najdete v tématu [klientské certifikáty pro Point-to-Site](vpn-gateway-certificates-point-to-site.md). Chcete-li nainstalovat certifikát, který byl vytvořen, přečtěte si téma [nainstalovat certifikát pro klienty Windows](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje.

#### <a name="certmaccli"></a>Instalace klienta VPN MAC (OS X)

Pro každé zařízení Mac, která se připojuje k službě Azure VNet musí vytvořit samostatný profil. Je to proto, že tato zařízení vyžadují uživatelský certifikát pro ověřování, aby se zadaná v profilu. **Obecné** složka obsahuje všechny informace potřebné k vytvoření profilu.

  * **VpnSettings.xml** obsahuje důležitá nastavení, jako je například typ serveru adresu a tunelové propojení.
  * **VpnServerRoot.cer** obsahuje kořenový certifikát, který je nutné k ověření služby VPN gateway během instalace připojení P2S.
  * **RadiusServerRoot.cer** obsahuje kořenový certifikát, který je nutné k ověření serveru RADIUS během ověřování.

Nativní klient VPN nakonfigurovat v systému Mac pro ověření certifikátu použijte následující kroky:

1. Import **VpnServerRoot** a **RadiusServerRoot** kořenové certifikáty pro vaše Mac. To lze provést tak, že kopírování souboru přes do počítače Mac a dvakrát klikněte na jeho.  
Klikněte na tlačítko **přidat** k importu.

  *Add VpnServerRoot*

  ![Přidání certifikátu](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  *Add RadiusServerRoot*

  ![Přidání certifikátu](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Každý klient vyžaduje klientský certifikát pro ověřování. Nainstalujte certifikát klienta v klientském zařízení.
3. Otevřete **sítě** dialogové okno pod **sítě Předvolby** a klikněte na tlačítko **'+'** k vytvoření nového profilu připojení VPN klienta pro připojení P2S k virtuální síti Azure.

  **Rozhraní** hodnota je "VPN" a **typ sítě VPN** hodnota je 'IKEv2'. Zadejte název pro profil v **název služby** pole a pak klikněte na **vytvořit** k vytvoření profilu připojení klienta VPN.

  ![Sítě](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. V **Obecné** složky, z **VpnSettings.xml** souboru, zkopírujte **VpnServer** hodnota značky. Vložte tuto hodnotu v **adresu serveru** a **vzdáleného ID** pole profilu. Ponechte **místní ID** prázdné pole.

  ![informace o serveru](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Klikněte na tlačítko **nastavení ověřování** a vyberte **certifikát**. 

  ![nastavení ověřování](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Klikněte na tlačítko **vyberte...** a vyberte certifikát, který chcete použít pro ověřování.

  ![certifikát](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Vyberte Identity** zobrazí seznam certifikátů můžete vybírat. Vybrat vhodný certifikát a potom klikněte na **pokračovat**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. V **místní ID** pole, zadejte název certifikátu (z kroku 6). V tomto příkladu je "ikev2Client.com". Potom klikněte na **použít** tlačítko a uložte změny.

  ![použít](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. Na **sítě** dialogové okno, klikněte na tlačítko **použít** uložit všechny změny. Potom klikněte na **Connect** spustit připojení P2S do virtuální sítě Azure.

## <a name="otherauth"></a>Práce s jinými typy ověřování nebo protokoly

Chcete používat různá ověřovací typu (například OTP) a ne uživatelského jména a hesla nebo certifikáty, nebo používají různá ověřovací protokol (například protokol PEAP-MSCHAPv2 místo EAP-MSCHAPv2), musíte vytvořit vlastní profil konfigurace klienta VPN. Chcete-li vytvořit profil, musíte informace, jako je IP adresa brány virtuální sítě, Typ tunelového propojení a děleného tunelového propojení směrování. Tyto informace můžete získat pomocí následujících kroků:

1. Použijte rutinu "Get-AzureRmVpnClientConfiguration" ke generování konfigurace klienta VPN pro EapMSChapv2. Pokyny najdete v tématu [v této části](#ccradius) článku.

2. Rozbalte soubor VpnClientConfiguration.zip a vyhledejte složku GenenericDevice. Ignorujte složky obsahující instalační služba systému Windows pro 64bitové a 32bitové architektury.
 
3. Složka GenenericDevice obsahuje soubor XML s názvem VpnSettings. Tento soubor obsahuje všechny požadované informace.

  * VpnServer - plně kvalifikovaný název domény služby Azure VPN Gateway. Toto je adresa, která se klient připojuje k.
  * VpnType - typu tunelu, který používáte pro připojení.
  * Trasy - tras, které jste nakonfigurovali v profilu tak, aby provoz vázaný jenom virtuální síť Azure je odesílán prostřednictvím tunelu P2S.
  * Složka GenenericDevice obsahuje také do souboru .cer s názvem 'VpnServerRoot'. Tento soubor obsahuje kořenový certifikát, který je nutné k ověření služby Azure VPN Gateway během instalace připojení P2S. Nainstalujte certifikát na všech zařízeních, která se budou připojovat k službě Azure VNet.

## <a name="next-steps"></a>Další postup

Vrátit do článku do [dokončit konfiguraci P2S](point-to-site-how-to-radius-ps.md).

Informace o odstraňování potíží P2S [připojení point-to-site řešení potíží s Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).