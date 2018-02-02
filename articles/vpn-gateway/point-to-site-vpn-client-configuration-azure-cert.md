---
title: "Vytvoření a instalace souborů konfigurace klienta P2S VPN pro ověření certifikátu Azure: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit a instalovat soubory konfigurace klienta VPN pro připojení Point-to-Site, která používá ověřování certifikátem."
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
ms.date: 01/29/2018
ms.author: cherylmc
ms.openlocfilehash: efe5d3db16db83568bb844894198b59a6b39f626
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Vytvoření a instalace souborů konfigurace klienta VPN pro nativní Azure certifikát ověřování P2S konfigurace

Soubory konfigurace klienta VPN jsou obsaženy v souboru zip. Konfigurační soubory, zadejte požadované nastavení pro nativní klient systému Windows nebo Mac IKEv2 VPN pro připojení k virtuální síti přes připojení Point-to-Site, která používá ověřování certifikátem. nativní Azure.

### <a name="workflow"></a>Pracovní postup P2S

  1. Nastaví bránu Azure VPN pro připojení P2S.
  2. Generování kořenového certifikátu a klientské certifikáty. Veřejný klíč certifikátu kořenové nahrát do Azure a nainstalujte klientské certifikáty na klientských zařízeních. Certifikáty se používají pro ověřování připojování uživatelů.
  3. Získání konfigurace klienta VPN pro možnost ověřování podle svého výběru a použít ho k nastavení klienta VPN na zařízení s Windows a Mac. To vám umožní připojit se k virtuálním sítím Azure prostřednictvím připojení Point-to-Site.

>[!NOTE]
>Konfigurační soubory klienta jsou specifické pro konfiguraci sítě VPN pro virtuální sítě. Je-li vygenerovat soubory konfigurace klienta VPN, třeba protokol typ sítě VPN nebo typ ověřování, se změny v konfiguraci Point-to-Site VPN je nutné vygenerovat nové soubory konfigurace klienta VPN pro uživatele zařízení.
>
>

## <a name="generate"></a>Generování souborů konfigurace klienta VPN

Než začnete, ujistěte se, že všechny připojování uživatelé mají platný certifikát nainstalovaný na zařízení uživatele. Další informace o instalaci certifikátu klienta najdete v tématu [nainstalovat certifikát klienta](point-to-site-how-to-vpn-client-install-azure-cert.md).

Můžete vygenerovat konfigurační soubory klienta pomocí prostředí PowerShell, nebo pomocí portálu Azure. Buď metoda vrátí stejné souboru zip. Rozbalte soubor zobrazíte následující složky:

  * **WindowsAmd64** a **WindowsX86**, které obsahují balíčky Instalační služby systému Windows 32-bit a 64-bit, v uvedeném pořadí. **WindowsAmd64** je balíček Instalační služby pro všechny podporované klienty Windows 64-bit, nikoli pouze Amd.
  * **Obecné**, který obsahuje obecné informace použít k vytvoření vlastní konfigurace klienta VPN. Ignorujte tuto složku. Obecné složce je zadán, pokud byla nakonfigurována IKEv2 nebo SSTP + IKEv2 v bráně. Pokud je nakonfigurována SSTP, pouze není obecné složce přítomen.

### <a name="zipportal"></a>Generování souborů pomocí portálu Azure

1. Na portálu Azure přejděte do bránu virtuální sítě pro virtuální síť, která chcete připojit.
2. Na stránce brány virtuální sítě klikněte na **KonfiguracePoint-to-site**.
3. V horní části stránky konfigurace Point-to-site, klikněte na tlačítko **klienta VPN Stáhnout**. Trvá několik minut pro klienta konfigurační balíček ke generování.
4. Váš prohlížeč označuje, že konfigurační soubor zip klienta je k dispozici. Je název stejný název jako bránu. Rozbalte soubor k zobrazení složky.

### <a name="zipps"></a>Generování souborů pomocí prostředí PowerShell

1. Při generování konfigurace klienta VPN pomocí souborů, hodnota pro '-AuthenticationMethod' je 'EapTls'. Generovat soubory konfigurace klienta VPN pomocí následujícího příkazu:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Zkopírujte adresu URL do prohlížeče ke stažení souboru zip a potom rozbalte soubor k zobrazení složky.

## <a name="installwin"></a>Instalovat balíček konfigurace klienta VPN ve Windows

Na každém klientském počítači s Windows, můžete použít stejný balíček konfigurace klienta VPN, tak dlouho, dokud verze odpovídá architektuře pro klienta. Seznam klientské operační systémy, které jsou podporované, najdete v části Point-to-Site [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#P2S).

Pomocí následujícího postupu můžete nakonfigurovat Nativní klient VPN ve Windows pro ověření certifikátu:

1. Vyberte konfigurační soubory klienta VPN, které odpovídá architektuře počítače Windows. Pro 64bitový procesor architekturu vyberte balíček Instalační služby 'VpnClientSetupAmd64'. Pro architekturu procesoru 32-bit vyberte balíček Instalační služby 'VpnClientSetupX86'. 
2. Dvakrát klikněte na balíček k její instalaci. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**.
3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje. 

## <a name="installmac"></a>Konfigurace klienta VPN na počítače Mac (OSX)

Azure neposkytuje mobileconfig soubor pro ověření certifikátu nativní Azure. Budete muset ručně nakonfigurovat Nativní klient IKEv2 VPN v každé Mac, který se připojí k Azure. **Obecné** složka obsahuje všechny informace, které je třeba ji nakonfigurovat. Pokud nevidíte obecné složce v stahování, je pravděpodobné, že IKEv2 nebyl vybrán jako typ tunelového propojení. Pokud je vybraná IKEv2, generujte souboru zip a načtěte obecné složce. Obecné složce obsahuje následující soubory:

* **VpnSettings.xml**, která obsahuje důležité nastavení, jako je typ serveru adresu a tunelové propojení. 
* **VpnServerRoot.cer**, který obsahuje kořenový certifikát, který je nutné k ověření služby Azure VPN Gateway během instalace připojení P2S.

Použijte následující postup ke konfiguraci Nativní klient VPN v systému Mac pro ověřování pomocí certifikátu. Je nutné provést tento postup na každý Mac, který se připojí k Azure:

1. Import **VpnServerRoot** kořenového certifikátu k vaší Mac. To lze provést tak, že kopírování souboru přes do počítače Mac a dvakrát kliknete na soubor na něm.  
Klikněte na tlačítko **přidat** k importu.

  ![Přidání certifikátu](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Dvojitým kliknutím na certifikátu nemusí být zobrazeny **přidat** dialogové okno, ale certifikát je nainstalovaný do správného úložiště. Můžete zkontrolovat pro certifikát v řetězci klíčů přihlášení v kategorii certifikáty.
  
2. Otevřete **sítě** dialogové okno pod **sítě Předvolby** a klikněte na tlačítko **'+'** k vytvoření nového profilu připojení VPN klienta pro připojení P2S k virtuální síti Azure.

  **Rozhraní** hodnota je "VPN" a **typ sítě VPN** hodnota je 'IKEv2'. Zadejte název pro profil v **název služby** pole a pak klikněte na **vytvořit** k vytvoření profilu připojení klienta VPN.

  ![Sítě](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
3. V **Obecné** složky, z **VpnSettings.xml** souboru, zkopírujte **VpnServer** hodnota značky. Vložte tuto hodnotu v **adresu serveru** a **vzdáleného ID** pole profilu.

  ![informace o serveru](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
4. Klikněte na tlačítko **nastavení ověřování** a vyberte **certifikát**. 

  ![nastavení ověřování](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
5. Klikněte na tlačítko **vyberte...** a vyberte certifikát klienta, který chcete použít pro ověřování. Klientský certifikát by měl být již nainstalován na počítači (viz krok #2 **P2S pracovního postupu** část výše).

  ![certifikát](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
6. **Vyberte Identity** zobrazí seznam certifikátů můžete vybírat. Vybrat vhodný certifikát a potom klikněte na **pokračovat**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
7. V **místní ID** pole, zadejte název certifikátu (z kroku 6). V tomto příkladu je "ikev2Client.com". Potom klikněte na **použít** tlačítko a uložte změny.

  ![použít](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
8. Na **sítě** dialogové okno, klikněte na tlačítko **použít** uložit všechny změny. Potom klikněte na **Connect** spustit připojení P2S do virtuální sítě Azure.

## <a name="next-steps"></a>Další kroky

Vrátit do článku do [dokončit konfiguraci P2S](vpn-gateway-howto-point-to-site-rm-ps.md).
