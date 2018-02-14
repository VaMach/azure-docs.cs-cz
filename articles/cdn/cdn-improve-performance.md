---
title: "Zvýšení výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Naučte se zlepšit rychlost přenosu souborů a zvýšit výkon načtení stránky komprimací souborů v Azure CDN."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: mazha
ms.openlocfilehash: 743d1db803cdb58ae8fa37430ccffa10ca003f93
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zvýšení výkonu komprimací souborů v Azure CDN
Kompresí souborů je jednoduchá ale účinná metoda a zvýšit rychlost přenosu souborů zvyšuje výkon načtení stránky snižuje velikost souboru před odesláním ze serveru. Kompresí souborů může snížit náklady na šířku pásma a poskytnout rychlejšího prostředí pro vaše uživatele.

Existují dva způsoby, jak povolit kompresi souboru:

- Povolte kompresi na původním serveru. V takovém případě CDN předá podél komprimovaných souborů a doručí aktualizace na klienty, kteří požadují je.
- Povolte kompresi přímo na serveru edge CDN. V takovém případě CDN komprimaci soubory a slouží pro koncové uživatele, i když nejsou komprimované serverem původu.

> [!IMPORTANT]
> Změny konfigurace CDN může trvat nějakou dobu šířit přes síť. Pro **Azure CDN společnosti Akamai** profily, šíření obvykle dokončení v části jednu minutu.  Pro **Azure CDN společnosti Verizon** profily, šíření obvykle dokončení během 90 minut. Pokud jste nastavení komprese pro koncový bod CDN poprvé, zvažte, zda nevyčkat 1 – 2 hodiny, než při řešení problému Ujistěte se, že rozšíření nastavení komprese do bodů POP.
> 
> 

## <a name="enabling-compression"></a>Povolení komprese
CDN úrovně standard a premium poskytují stejnou funkcionalitu kompresi, ale uživatelské rozhraní se liší. Další informace o rozdílech mezi CDN úrovně standard a premium najdete v tématu [přehled CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Úroveň Standard
> [!NOTE]
> Tato část se týká **Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.
> 
> 

1. Na stránce profil CDN vyberte koncového bodu CDN, které chcete spravovat.
   
    ![Koncové body profil CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Otevře se stránka koncového bodu CDN.
2. Vyberte **komprese**.

    ![Výběr komprese CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Otevře se stránka komprese.
3. Vyberte **na** zapnutí komprese.
   
    ![Možnosti komprese souboru CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Použít výchozí typ MIME, nebo upravte seznam přidáním nebo odebráním typy MIME.
   
   > [!TIP]
   > Přestože je možné, není doporučeno použít komprese komprimované formáty. Například ZIP, MP3, MP4 nebo JPG.
   > 
 
5. Po provedení změny, vyberte **Uložit**.

### <a name="premium-tier"></a>Úroveň Premium
> [!NOTE]
> V této části se vztahují pouze na **Azure CDN Premium od společnosti Verizon** profily.
> 

1. Na stránce profil CDN vyberte **spravovat**.
   
    ![Výběr spravovat CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem. Vyberte **komprese**.

    ![Výběr komprese CDN](./media/cdn-file-compression/cdn-compress-select.png)
   
    Zobrazí se možnosti komprese.
   
    ![Možnosti komprese souboru CDN](./media/cdn-file-compression/cdn-compress-files.png)
3. Povolit kompresi výběrem **povolená komprese**. Zadejte typy MIME, kterou chcete komprimovat jako seznam s položkami oddělenými čárkou (bez mezer) **typy souborů** pole.
   
   > [!TIP]
   > Přestože je možné, není doporučeno použít komprese komprimované formáty. Například ZIP, MP3, MP4 nebo JPG.
   > 
    
4. Po provedení změny, vyberte **aktualizace**.

## <a name="compression-rules"></a>Komprese pravidla

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium-tiers"></a>Azure CDN společnosti Verizon profilů (úrovně standard a premium)

Pro **Azure CDN společnosti Verizon** profilů, pouze vhodné soubory jsou komprimovány. Způsobilé k komprese, musíte do souboru:
- Být větší než 128 bajtů
- Být menší než 1 MB
 
Tyto profily podporují následující kódování komprese:
- gzip (GNU zip)
- DEFLATE
- bzip2
- brotli 
 
Pokud požadavek podporuje více než jeden typ komprese, tyto typy komprese mají přednost před brotli komprese.

Pokud požadavek na prostředek určuje brotli komprese (`Accept-Encoding: br` hlavičky protokolu HTTP) a výsledky žádosti v neúspěšnému přístupu do mezipaměti, Azure CDN provede kompresi brotli prostředku na původním serveru. Potom je komprimovaný soubor zpracovat přímo z mezipaměti.

### <a name="azure-cdn-from-akamai-profiles"></a>Azure CDN společnosti Akamai profily

Pro **Azure CDN společnosti Akamai** profily, všechny soubory jsou způsobilé pro kompresi. Ale musí být soubor typu MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).

Tyto profily podporovat kódování gzip komprese jenom. Když koncový bod profilu služby požádá o soubor s kódováním gzip, vždy na vyžádání z tohoto počátku, a to bez ohledu na žádost klienta. 

## <a name="compression-behavior-tables"></a>Komprese chování tabulky
Následující tabulky popisují chování Azure CDN komprese pro každý scénář:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Komprese je zakázána nebo jej nelze komprese souboru
| Klient požádal formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souboru do mezipaměti | CDN odpověď klientovi | Poznámky k&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované | |
| Komprimované |Nekomprimované |Nekomprimované | |
| Komprimované |Není v mezipaměti |Komprimované nebo nekomprimovaným |Odpověď na počátku Určuje, jestli CDN provádí komprese. |
| Nekomprimované |Komprimované |Nekomprimované | |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Je povolená komprese a soubor je vhodné pro kompresi
| Klient požádal formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souboru do mezipaměti | CDN odpověď klientovi | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované |CDN transcodes mezi podporované formáty. |
| Komprimované |Nekomprimované |Komprimované |CDN provede komprese. |
| Komprimované |Není v mezipaměti |Komprimované |CDN provede kompresi, pokud zdroj vrátí nekomprimovaného souboru. <br/>**Azure CDN společnosti Verizon** předá nekomprimovaného souboru na první požadavek a pak komprimuje a ukládá do mezipaměti souborů pro následné požadavky. <br/>Soubory s Cache-Control: nikdy jsou komprimované záhlaví bez mezipaměti. |
| Nekomprimované |Komprimované |Nekomprimované |CDN provede dekomprese. |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

## <a name="media-services-cdn-compression"></a>Komprese CDN služby Media Services
Pro koncové body povolené pro streamování Media Services CDN je ve výchozím nastavení pro následující typy MIME povolená komprese: 
- application/vnd.ms-sstr+xml 
- application/dash+xml
- application/vnd.apple.mpegurl
- application/f4m+xml 

## <a name="see-also"></a>Další informace najdete v tématech
* [Řešení potíží s kompresí souborů v síti CDN](cdn-troubleshoot-compression.md)    

