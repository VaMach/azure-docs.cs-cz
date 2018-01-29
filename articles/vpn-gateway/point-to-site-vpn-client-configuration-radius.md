---
title: "Vytvoření a instalace souborů konfigurace klienta VPN pro připojení P2S RADIUS: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit soubor konfigurace klienta VPN pro připojení Point-to-Site, které používají ověřování pomocí protokolu RADIUS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: cherylmc
ms.openlocfilehash: 838065287279f1c17e7f294bc919c4a0421e2a58
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Vytvoření a instalace souborů konfigurace klienta VPN pro ověřování pomocí protokolu RADIUS P2S

Soubory konfigurace klienta VPN jsou obsaženy v souboru zip. Konfigurační soubory, zadejte požadované nastavení pro nativní klient systému Windows nebo Mac IKEv2 VPN pro připojení k virtuální síti přes Point-to-Site. RADIUS server poskytuje několik možností ověřování a jako takový se liší pro jednotlivé možnosti konfigurace klienta VPN.

### <a name="workflow"></a>Pracovní postup

  1. [Nastaví bránu Azure VPN pro připojení P2S](point-to-site-how-to-radius-ps.md).
  2. [Nastavit server RADIUS pro ověřování](point-to-site-how-to-radius-ps.md#radius). 
  3. (Tento článek) - získat konfiguraci sítě VPN klienta pro možnost ověřování podle svého výběru a použít ho k nastavení klienta VPN na zařízení se systémem Windows. To vám umožní připojit se k virtuálním sítím Azure prostřednictvím připojení P2S.
  4. [Dokončete konfiguraci P2S a připojte](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Je-li vygenerovat profil konfigurace klienta VPN, jako je například protokol typ sítě VPN nebo typ ověřování, se změny v konfiguraci Point-to-Site VPN musí vygenerovat a nainstalujte nové konfigurace klienta VPN v zařízeních uživatele.
>
>

## <a name="adeap"></a>O ověřování uživatelského jména a hesla

* **Ověřování AD:** oblíbených scénář je ověřování v doméně AD. V tomto scénáři uživatelé používat přihlašovací údaje domény pro připojení k virtuálním sítím Azure. Můžete vytvořit soubory konfigurace klienta VPN pro ověřování pomocí protokolu RADIUS AD.

* **Ověřování bez AD:** můžete taky nakonfigurovat scénář ověřování RADIUS uživatelského jména a hesla bez AD.

Ujistěte se, že všichni uživatelé připojující mít přihlašovací údaje uživatelského jména a hesla, které lze ověřit pomocí protokolu RADIUS. Můžete vytvořit pouze konfiguraci pro ověřovací protokol EAP-MSCHAPv2 uživatelského jména a hesla. ' – AuthenticationMethod' je zadán jako 'EapMSChapv2'.

## <a name="usernamefiles"></a> 1. Generování souborů konfigurace klienta VPN

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

## <a name="setupusername"></a> 2. Konfigurace klientů Windows a Mac VPN
 
### <a name="adwincli"></a>Instalace klienta VPN ve Windows

Na každém klientském počítači s Windows, můžete použít stejný balíček konfigurace klienta VPN, tak dlouho, dokud verze odpovídá architektuře pro klienta. Seznam klientské operační systémy, které jsou podporované, najdete v části Point-to-Site [– nejčastější dotazy](vpn-gateway-vpn-faq.md#P2S).

Pomocí následujícího postupu můžete nakonfigurovat Nativní klient VPN ve Windows pro ověření certifikátu:

1. Vyberte konfigurační soubory klienta VPN, které odpovídá architektuře počítače Windows. Pro 64bitový procesor architekturu vyberte balíček Instalační služby 'VpnClientSetupAmd64'. Pro architekturu procesoru 32-bit vyberte balíček Instalační služby 'VpnClientSetupX86'. 
2. Dvakrát klikněte na balíček k její instalaci. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje. 

### <a name="admaccli"></a>Instalace klienta VPN MAC (OSX)

1. Vyberte **VpnClientSetup mobileconfig** souboru a jeho odeslání jednotlivých uživatelů. K tomu můžete použít e-mailu nebo jiným způsobem.

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

## <a name="certeap"></a>O ověření certifikátu
 
Klient VPN můžete vytvořit konfigurační soubory pro ověřování protokolu RADIUS certifikát, který používá protokol EAP-TLS. Certifikát vydaný Enterprise se obvykle používá k ověření uživatele pro síť VPN. Ujistěte se, zda všichni uživatelé připojující certifikát nainstalovaný na zařízení uživatelů, a že může být ověřen certifikát serveru RADIUS.
 
* ' – AuthenticationMethod' je 'EapTls'.
* Při ověřování certifikátu klienta ověří RADIUS server ověřením svůj certifikát. -RadiusRootCert je soubor .cer obsahující kořenový certifikát, který se používá k ověření serveru RADIUS.  
* V některých případech zařízení se systémem Windows má více klientských certifikátů. Při ověřování to může způsobit v automaticky otevřeném okně. dialog výpis všech certifikátů. Uživatel pak musí vybrat certifikát, který chcete použít. Správný certifikát můžete odfiltrovat zadáním kořenový certifikát, ke které by měly být zřetězené klientského certifikátu. '-ClientRootCert, je soubor .cer, který obsahuje kořenový certifikát. Je volitelný parametr. Pokud zařízení, ze kterého se chcete připojit má jen jeden klientský certifikát, pak tento parametr není nutné zadávat.

## <a name="certfiles"></a>1. Generování souborů konfigurace klienta VPN

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
 
## <a name="setupusername"></a> 2. Konfigurace klientů Windows a Mac VPN

### <a name="certwincli"></a>Instalace klienta VPN ve Windows

1. Vyberte konfigurační balíček a nainstalujte ho v klientském zařízení. Pro 64bitový procesor architekturu vyberte balíček Instalační služby 'VpnClientSetupAmd64'. Pro architekturu procesoru 32-bit vyberte balíček Instalační služby 'VpnClientSetupX86'. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**. Můžete také balíček uložit k instalaci na další klientské počítače.
2. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje.

### <a name="certmaccli"></a>Instalace klienta VPN MAC (OSX)

Pro každé zařízení Mac, která se připojuje k službě Azure VNet musí vytvořit samostatný profil. Je to proto, že tato zařízení vyžadují uživatelský certifikát pro ověřování, aby se zadaná v profilu. **Obecné** složka obsahuje všechny informace potřebné k vytvoření profilu.

  * **VpnSettings.xml** obsahuje důležitá nastavení, jako je například typ serveru adresu a tunelové propojení.
  * **VpnServerRoot.cer** obsahuje kořenový certifikát, který je nutné k ověření služby VPN gateway během instalace připojení P2S.
  * **RadiusServerRoot.cer** obsahuje kořenový certifikát, který je nutné k ověření serveru RADIUS během ověřování.

Nativní klient VPN nakonfigurovat v systému Mac pro ověření certifikátu použijte následující kroky:

1. Import **VpnServerRoot** a **RadiusServerRoot** kořenové certifikáty pro vaše Mac. To lze provést tak, že kopírování souboru přes do počítače Mac a dvakrát klikněte na jeho.  
Klikněte na tlačítko **přidat** k importu.

  **Add VpnServerRoot**

  ![Přidání certifikátu](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

  **Add RadiusServerRoot**

  ![Přidání certifikátu](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Otevřete **sítě** dialogové okno pod **sítě Předvolby** a klikněte na tlačítko **'+'** k vytvoření nového profilu připojení VPN klienta pro připojení P2S k virtuální síti Azure.

  **Rozhraní** hodnota je "VPN" a **typ sítě VPN** hodnota je 'IKEv2'. Zadejte název pro profil v **název služby** pole a pak klikněte na **vytvořit** k vytvoření profilu připojení klienta VPN.

  ![Sítě](./media/point-to-site-vpn-client-configuration-radius/network.png)
3. V **Obecné** složky, z **VpnSettings.xml** souboru, zkopírujte **VpnServer** hodnota značky. Vložte tuto hodnotu v **adresu serveru** a **vzdáleného ID** pole profilu. Ponechte **místní ID** prázdné pole.

  ![informace o serveru](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
4. Klikněte na tlačítko **nastavení ověřování** a vyberte **certifikát**. 

  ![nastavení ověřování](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
5. Klikněte na tlačítko **vyberte...** a vyberte certifikát, který chcete použít pro ověřování.

  ![certifikát](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
6. **Vyberte Identity** zobrazí seznam certifikátů můžete vybírat. Vybrat vhodný certifikát a potom klikněte na **pokračovat**.

  ![identity](./media/point-to-site-vpn-client-configuration-radius/identity.png)
7. V **místní ID** pole, zadejte název certifikátu (z kroku 5). V tomto příkladu je "ikev2Client.com". Potom klikněte na **použít** tlačítko a uložte změny.

  ![použít](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
8. Na **sítě** dialogové okno, klikněte na tlačítko **použít** uložit všechny změny. Potom klikněte na **Connect** spustit připojení P2S do virtuální sítě Azure.

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
