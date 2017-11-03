---
title: "Nastavení webového proxy serveru pro zařízení StorSimple | Microsoft Docs"
description: "Další informace o použití prostředí Windows PowerShell pro StorSimple ke konfiguraci nastavení webového proxy serveru pro zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6c2ca351-a7c6-4da6-ab5e-c081e6d08261
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 66ee6ce15e51b14366eac0512c899d1c425c6092
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfigurace webového proxy serveru pro zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz popisuje, jak pomocí prostředí Windows PowerShell pro StorSimple ke konfiguraci a zobrazení nastavení webového proxy serveru pro zařízení StorSimple. Nastavení webového proxy serveru jsou používány zařízení StorSimple při komunikaci s cloudem. Webový proxy server se používá k přidání další úroveň zabezpečení, filtrování obsahu mezipaměti snadné nároky na šířku pásma nebo i pomoc s analytics.

Webový proxy server je volitelná konfigurace pro zařízení StorSimple. Můžete nakonfigurovat webový proxy server jenom prostřednictvím Windows Powershellu pro StorSimple. Konfigurace je dvoustupňový proces takto:

1. Je nejprve nakonfigurovat nastavení webového proxy serveru pomocí Průvodce instalací nebo prostředí Windows PowerShell pro StorSimple rutiny.
2. Je pak povolit nastavení nakonfigurované webového proxy serveru pomocí prostředí Windows PowerShell pro StorSimple rutiny.

Po dokončení konfigurace webového proxy serveru můžete zobrazit nastavení nakonfigurované webového proxy serveru služby Microsoft Azure StorSimple Manager a prostředí Windows PowerShell pro StorSimple. 

Po přečtení tohoto kurzu, budete moci:

* Nakonfigurovat webový proxy server pomocí Průvodce instalací a rutin
* Povolit webový proxy server pomocí rutin
* Nastavení proxy serveru webové zobrazení na portálu Azure classic
* Řešení chyb během konfigurace webového proxy serveru

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Nakonfigurovat webový proxy server pomocí prostředí Windows PowerShell pro StorSimple
Pomocí následujících konfigurovat nastavení webového proxy serveru:

* Průvodce instalací vás provedou kroky konfigurace.
* Rutiny Windows Powershellu pro StorSimple.

Každá z těchto metod je popsané v následujících částech.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Nakonfigurovat webový proxy server pomocí Průvodce instalací
Průvodce instalací vám pomůže vás provede kroky pro konfigurace webového proxy serveru. Proveďte následující kroky nakonfigurovat webový proxy server na vašem zařízení.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Chcete-li nakonfigurovat webový proxy server pomocí Průvodce instalací
1. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup** a poskytují **hesla správce zařízení**. Zadejte následující příkaz pro spuštění relace Průvodce instalací:
   
    `Invoke-HcsSetupWizard`
2. Pokud je to poprvé, co jste použili Průvodce instalací pro registraci zařízení, musíte nakonfigurovat všechna nastavení požadovaným síťovým, dokud se nedostanete konfigurace webového proxy serveru. Pokud vaše zařízení je již zaregistrován, může přijmout všechny nakonfigurovaná síťová nastavení, dokud se nedostanete konfigurace webového proxy serveru. V Průvodci instalací po zobrazení výzvy ke konfiguraci nastavení webového proxy serveru, zadejte **Ano**.
3. Pro **adresa URL proxy serveru webové**, zadejte IP adresu nebo název plně kvalifikované domény (FQDN) webového proxy serveru a číslo portu TCP, který chcete zařízení pro použití při komunikaci s cloudem. Použijte následující formát:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Ve výchozím nastavení je zadané číslo portu TCP 8080.
4. Vyberte typ ověřování jako **NTLM**, **základní**, nebo **žádné**. Základní je nejmíň bezpečná ověřování pro konfiguraci proxy serveru. NT LAN Manager (NTLM) je vysoce zabezpečené a komplexní ověřovací protokol, který používá třícestné systému zasílání zpráv (někdy čtyři Pokud je potřeba další integrity) k ověření uživatele. Výchozí ověřování je NTLM. Další informace najdete v tématu [základní](http://hc.apache.org/httpclient-3.x/authentication.html) a [ověřování protokolem NTLM](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **Ve službě StorSimple Manager grafy monitorování zařízení nefungují, pokud základní nebo je v konfiguraci proxy serveru pro zařízení povolené ověřování protokolem NTLM. Pro monitorování grafy pracovat musíte zajistit, že, ověření je nastaven na hodnotu NONE.**
   > 
   > 
5. Pokud používáte ověřování, zadejte **uživatelské jméno Proxy webové** a **heslo pro proxy server webové**. Musíte také potvrdit heslo.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Pokud registrujete zařízení poprvé, pokračujte s registrací. Pokud vaše zařízení už je registrovaná, dojde k ukončení průvodce. Nakonfigurované nastavení se uloží.

Webový proxy server bude nyní být rovněž povolena. Můžete přeskočit [Povolit proxy server webové](#enable-web-proxy) krok a přejít přímo na [zobrazit nastavení webového proxy serveru na portálu Azure classic](#view-web-proxy-settings-in-the-azure-classic-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurace webového proxy serveru pomocí Windows Powershellu pro StorSimple rutiny
Alternativní způsob konfigurace nastavení webového proxy serveru je prostřednictvím Windows Powershellu pro StorSimple rutiny. Proveďte následující postup pro konfiguraci webového proxy serveru.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Chcete-li nakonfigurovat webový proxy server pomocí rutiny
1. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**. Pokud budete vyzváni, zadejte **hesla správce zařízení**. Výchozí heslo je `Password1`.
2. Na příkazovém řádku zadejte:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Zadejte a potvrďte heslo po zobrazení výzvy, jak je uvedeno níže.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Webový proxy server je nyní nakonfigurována a musí být povolena.

## <a name="enable-web-proxy"></a>Povolit webový proxy server
Webový proxy server je ve výchozím nastavení zakázán. Po nakonfigurování nastavení webového proxy serveru na zařízení StorSimple, musíte povolit nastavení webového proxy serveru pomocí prostředí Windows PowerShell pro StorSimple.

> [!NOTE]
> **Tento krok nebude nutné, pokud jste použili Průvodce instalací nakonfigurovat webový proxy server. Webový proxy server je automaticky povolené ve výchozím nastavení po relaci Průvodce instalací.**
> 
> 

Ve Windows Powershellu pro StorSimple povolit webový proxy server na vašem zařízení postupujte takto:

#### <a name="to-enable-web-proxy"></a>Chcete-li povolit webový proxy server
1. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**. Pokud budete vyzváni, zadejte **hesla správce zařízení**. Výchozí heslo je `Password1`.
2. Na příkazovém řádku zadejte:
   
    `Enable-HcsWebProxy`
   
    Nyní jste povolili konfigurace webového proxy serveru v zařízení StorSimple.
   
    ![Nakonfigurovat webový proxy server na StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Nastavení proxy serveru webové zobrazení na portálu Azure classic
Nastavení webového proxy serveru jsou nakonfigurovány pomocí rozhraní Windows PowerShell a nemůže být změněn z v rámci portálu classic. Tyto nakonfigurované nastavení můžete, ale zobrazit na portálu classic. Proveďte následující kroky, chcete-li zobrazit webový proxy server.

#### <a name="to-view-web-proxy-settings"></a>Chcete-li zobrazit nastavení webového proxy serveru
1. Přejděte na **služby StorSimple Manager > zařízení**. Vyberte a klikněte na zařízení a potom přejděte na **konfigurace**.
2. Přejděte dolů na **konfigurace** stránky k **webové nastavení proxy serveru** části. Nastavení nakonfigurované webového proxy serveru můžete zobrazit na zařízení StorSimple, jak je uvedeno níže.
   
    ![Proxy webové zobrazení na portálu pro správu](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)

## <a name="errors-during-web-proxy-configuration"></a>Chyby během konfigurace webového proxy serveru
Pokud nastavení webového proxy serveru správně nakonfigurována, se zobrazí chybové zprávy pro uživatele v prostředí Windows PowerShell pro StorSimple. Následující tabulka popisuje některé z těchto chybových zpráv, jejich možných příčin a doporučené akce.

| Sériové č. | Chyba HRESULT kódu | Možné příčiny | Doporučená akce |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Příkaz spuštěn z pasivní řadiče a není schopen komunikovat s řadičem aktivní. |Spusťte příkaz v aktivním řadiči. Ke spuštění příkazu z pasivní řadiče, musíte opravit připojení ze pasivní do aktivního řadiče. Musíte používat Microsoft Support, pokud toto připojení je přerušené. |
| 2. |0x800710dd - identifikátor operace není platná |Nastavení proxy serveru nejsou podporovány na virtuální zařízení StorSimple. |Nastavení proxy serveru nejsou podporovány na virtuální zařízení StorSimple. To se dá nakonfigurovat jenom na fyzického zařízení StorSimple. |
| 3. |0x80070057 – neplatný parametr |Jeden z parametrů zadaných pro nastavení proxy serveru je neplatný. |Identifikátor URI není k dispozici ve správném formátu. Použijte následující formát:`http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - server RPC není k dispozici |Hlavní příčinou je jedním z těchto:</br></br>Cluster není nahoru.</br></br>DataPath služba není spuštěna.</br></br>Spuštění příkazu z pasivní řadiče a není schopen komunikovat s řadičem aktivní. |Microsoft Support zajistit, že je cluster v provozu a je spuštěna služba datapath prosím zapojení.</br></br>Spusťte příkaz z aktivní řadiče. Pokud chcete spustit příkaz z pasivní řadiče, musíte zajistit, že je pasivní řadič může komunikovat s řadičem active. Musíte používat Microsoft Support, pokud toto připojení je přerušené. |
| 5. |0X800706be - RPC volání se nezdařilo. |Clusteru je mimo provoz. |Prosím zaujmout Microsoft Support zajistit, že je cluster v provozu. |
| 6. |0x8007138f – prostředek clusteru nebyl nalezen |Prostředek clusteru služby platformy nebyla nalezena. To může nastat při instalaci nebyla správná. |Potřebujete obnovit v zařízení výrobní nastavení. Musíte vytvořit prostředek platformy. Microsoft Support Požádejte prosím o dalších krocích. |
| 7. |0x8007138c – prostředek clusteru není online |Prostředky clusteru platformy nebo datapath nejsou online. |Obraťte se na Microsoft Support k zajištění, že prostředek služby datapath a platforma jsou online. |

> [!NOTE]
> * Výše uvedený seznam chybových zpráv není vyčerpávající. 
> * Chyby související s nastavení webového proxy serveru se nezobrazí na portálu Azure classic ve službě StorSimple Manager. Pokud nastane problém s webový proxy server po dokončení konfigurace, zařízení stav se změní na **Offline** na klasickém portálu. |
> 
> 

## <a name="next-steps"></a>Další kroky
* Pokud máte problémy při nasazení zařízení nebo konfigurace nastavení webového proxy serveru, podívejte se na [řešení potíží s nasazením zařízení StorSimple](storsimple-troubleshoot-deployment.md).
* Další informace o použití služby StorSimple Manager, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

