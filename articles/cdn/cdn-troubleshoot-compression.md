---
title: "Řešení potíží s kompresí souborů v Azure CDN | Microsoft Docs"
description: "Vyřešte problémy s kompresí souborů Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>Poradce při potížích s kompresí souborů CDN
Tento článek vám pomůže vyřešit problémy s [komprese souboru CDN](cdn-improve-performance.md).

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a fóra Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu**.

## <a name="symptom"></a>Příznaky
Je povolená komprese pro svůj koncový bod, ale soubory se vrací nekomprimované.

> [!TIP]
> Pokud chcete zkontrolovat, zda jsou vaše soubory nevrátila komprimovaný, budete muset použít nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) nebo prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Hlavičky odpovědi zkontrolujte HTTP vrátil s vaší mezipaměti CDN obsahu.  Pokud je záhlaví s názvem `Content-Encoding` s hodnotou **gzip**, **bzip2**, nebo **deflate**, obsah je komprimován.
> 
> ![Kódování obsahu hlaviček](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Příčina
Existuje několik možných příčin, včetně:

* Požadovaný obsah nejsou vhodné pro kompresi.
* Typ požadovaný soubor není povolená komprese.
* Požadavek HTTP neobsahuje hlavičku požaduje typ platný komprese.

## <a name="troubleshooting-steps"></a>Řešení potíží
> [!TIP]
> Stejně jako u nasazení nové koncové body, provést změny konfigurace CDN některé čas potřebný k šíření přes síť.  Obvykle změny se použijí během 90 minut.  Pokud je poprvé, které jste nastavili komprese pro koncový bod CDN, měli byste zvážit čekání 1 – 2 hodiny jistotu komprese, které se mají nastavení rozšíří do bodů POP. 
> 
> 

### <a name="verify-the-request"></a>Ověřit požadavek
Nejdřív by měl provedeme kontrolu rychlé správností v požadavku.  Můžete použít v prohlížeči na [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) zobrazíte požadavky prováděné.

* Ověření požadavku je odesílána vaše adresa URL koncového bodu `<endpointname>.azureedge.net`a ne do zdrojového umístění.
* Ověřit požadavek obsahuje **Accept-Encoding** obsahuje hlavičku a hodnotu této hlavičky **gzip**, **deflate**, nebo **bzip2**.

> [!NOTE]
> **Azure CDN společnosti Akamai** profily podporuje se jen **gzip** kódování.
> 
> 

![Hlavičky žádosti CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Ověřte nastavení komprese (profil CDN úrovně Standard)
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Standard od společnosti Verizon** nebo **Azure CDN Standard od společnosti Akamai** profilu. 
> 
> 

Přejděte na váš koncový bod v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **konfigurace** tlačítko.

* Ověřte, že je povolená komprese.
* Ověřte, že typ MIME pro obsah, aby se komprimoval je obsažena v seznamu komprimované formátů.

![Nastavení komprese CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Ověřte nastavení komprese (profil Premium CDN)
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN Premium od společnosti Verizon** profilu.
> 
> 

Přejděte na váš koncový bod v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **spravovat** tlačítko.  Otevře se na doplňkovém portálu.  Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.  Klikněte na tlačítko **komprese**. 

* Ověřte, že je povolená komprese.
* Ověřte **typy souborů** seznam obsahuje seznam oddělený čárkami (bez mezer) typů standardu MIME.
* Ověřte, že typ MIME pro obsah, aby se komprimoval je obsažena v seznamu komprimované formátů.

![Nastavení komprese CDN premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Ověřte, že obsah se uloží do mezipaměti
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).
> 
> 

Používání nástrojů pro vývojáře v prohlížeči, zkontrolujte hlavičky odpovědi zajistit, že soubor je uložené v mezipaměti v oblasti, kde jsou požadovány.

* Zkontrolujte **Server** hlavičky odpovědi.  Záhlaví musí mít formát **platformy (ID POP nebo serveru)**, jak je vidět v následujícím příkladu.
* Zkontrolujte **X mezipaměti** hlavičky odpovědi.  Záhlaví měli přečíst **DOSÁHL**.  

![Hlavičky odpovědi CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Ověřte, že soubor splňuje požadavky na velikost
> [!NOTE]
> Tento krok platí jenom v případě, že je váš profil CDN **Azure CDN společnosti Verizon** profil (Standard nebo Premium).
> 
> 

Přijatelné pro kompresi soubor musí splňovat následující požadavky na velikost:

* Větší než 128 bajtů.
* Menší než 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Zkontrolujte požadavek na původním serveru **prostřednictvím** záhlaví
**Prostřednictvím** hlavičky protokolu HTTP na webový server označuje, že požadavek je předávána pomocí serveru proxy.  Webové servery Microsoft IIS ve výchozím nastavení není kompresi odpovědí, pokud požadavek obsahuje **prostřednictvím** záhlaví.  Chcete-li toto chování potlačit, postupujte takto:

* **Služby IIS 6**: [nastavit HcNoCompressionForProxies = "FALSE" ve vlastnostech metabáze služby IIS](https://msdn.microsoft.com/library/ms525390.aspx)
* **Službu IIS 7 a vyšší**: [nastavit obě **noCompressionForHttp10** a **noCompressionForProxies** na hodnotu False v konfiguraci serveru](http://www.iis.net/configreference/system.webserver/httpcompression)

