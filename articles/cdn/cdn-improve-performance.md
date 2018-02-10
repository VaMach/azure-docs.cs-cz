---
title: "Zvýšení výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Naučte se zlepšit rychlost přenosu souborů a zvyšuje zatížení stránky komprimací souborů v Azure CDN."
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
ms.date: 02/08/2018
ms.author: mazha
ms.openlocfilehash: 77d889f5d56ed839665588cf359b73e0f9ad28b5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zvýšení výkonu komprimací souborů v Azure CDN
Komprese je jednoduchá ale účinná metoda a zvýšit rychlost přenosu souborů zvyšuje zatížení stránky omezení velikosti souboru před odesláním ze serveru. Umožňuje snížit náklady na šířku pásma a poskytuje rychlejší reakce prostředí pro uživatele.

Existují dva způsoby, jak povolit kompresi:

* Povolte kompresi na původním serveru. V takovém případě CDN projdou komprimovaných souborů a doručí aktualizace na klienty, kteří požadují je.
* Povolte kompresi přímo na serveru edge CDN. V takovém případě CDN komprimaci soubory a slouží pro koncové uživatele, i když nejsou komprimované serverem původu.

> [!IMPORTANT]
> Změny konfigurace CDN může trvat nějakou dobu šířit přes síť. Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení v části jednu minutu.  Pro <b>Azure CDN společnosti Verizon</b> profily, šíření obvykle dokončení během 90 minut. Pokud jste nastavení komprese pro koncový bod CDN poprvé, zvažte, zda nevyčkat 1 – 2 hodiny, než při řešení problému Ujistěte se, že rozšíření nastavení komprese do bodů POP.
> 
> 

## <a name="enabling-compression"></a>Povolení komprese
Úrovních Standard a Premium CDN nabízí stejnou funkčnost kompresi, ale uživatelské rozhraní se liší. Další informace o rozdílech mezi úrovních Standard a Premium CDN najdete v tématu [přehled CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Úroveň Standard
> [!NOTE]
> Tato část se týká **Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.
> 
> 

1. Na stránce profil CDN vyberte koncového bodu CDN, které chcete spravovat.
   
    ![Koncové body profil CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Otevře se stránka koncového bodu CDN.
2. Vyberte **komprese**.

    ![Koncové body profil CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Otevře se stránka komprese.
3. Vyberte **na** zapnutí komprese.
   
    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Použijte výchozí typy formátu nebo upravte seznam přidáním nebo odebráním typy formátu.
   
   > [!TIP]
   > Přestože je možné, není doporučeno použít komprese komprimované formáty. Například ZIP, MP3, MP4 nebo JPG.
   > 
 
5. Po provedení změny, vyberte **Uložit**.

### <a name="premium-tier"></a>Úroveň Premium
> [!NOTE]
> V této části se vztahují pouze na **Azure CDN Premium od společnosti Verizon** profily.
> 
> 

1. Na stránce profil CDN vyberte **spravovat**.
   
    ![Tlačítko Spravovat profil CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem. Vyberte **komprese**.

    ![Výběr komprese souboru](./media/cdn-file-compression/cdn-compress-select.png)
   
    Zobrazí se možnosti komprese.
   
    ![Možnosti komprese souboru](./media/cdn-file-compression/cdn-compress-files.png)
3. Povolit kompresi výběrem **povolená komprese**. Zadejte typy MIME, kterou chcete komprimovat jako seznam s položkami oddělenými čárkou (bez mezer) **typy souborů** pole.
   
   > [!TIP]
   > Přestože je možné, není doporučeno použít komprese komprimované formáty. Například ZIP, MP3, MP4 nebo JPG.
   > 
    
4. Po provedení změny, vyberte **aktualizace**.

## <a name="compression-rules"></a>Komprese pravidla

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium"></a>Azure CDN společnosti Verizon profilů (standard a premium)

Pro **Azure CDN společnosti Verizon** profilů, pouze vhodné soubory jsou komprimovány. Způsobilé k komprese, musíte do souboru:
- Být větší než 128 bajtů.
- Být menší než 1 MB.
 
Tyto profily podporovat **gzip** (GNU zip), **deflate**, **bzip2**, nebo **Brazílie** kódování (Brotli). Pokud požadavek podporuje více než jeden typ komprese, tyto typy komprese mají přednost před Brotli komprese.

Pokud požadavek na prostředek Určuje kódování Brotli (obsahuje hlavičku HTTP `Accept-Encoding: br`) a výsledky žádosti v neúspěšnému přístupu do mezipaměti, Azure CDN provede kompresi Brotli prostředku na původním serveru. Potom je komprimovaný soubor zpracovat přímo z mezipaměti.

### <a name="azure-cdn-from-akamai-profiles"></a>Azure CDN společnosti Akamai profily

Pro **Azure CDN společnosti Akamai** profily, všechny soubory jsou způsobilé pro kompresi. Ale musí být soubor typu MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).

Tyto profily podporují pouze **gzip** kódování. Když koncový bod profilu služby požadavky **gzip** kódování souborů, jsou vždy požadovány z tohoto počátku, a to bez ohledu na žádost klienta. 

## <a name="compression-behavior-tables"></a>Komprese chování tabulky
Následující tabulky popisují chování Azure CDN komprese pro každý scénář:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Komprese je zakázána nebo jej nelze komprese souboru
| Klient požádal formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souboru do mezipaměti | CDN odpověď klientovi | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované | |
| Komprimované |Nekomprimované |Nekomprimované | |
| Komprimované |Není v mezipaměti |Komprimované nebo nekomprimovaným |Závisí na počátku odpovědi |
| Nekomprimované |Komprimované |Nekomprimované | |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Je povolená komprese a soubor je vhodné pro kompresi
| Klient požádal formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souboru do mezipaměti | CDN odpověď klientovi | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované |CDN transcodes mezi podporované formáty |
| Komprimované |Nekomprimované |Komprimované |CDN provede kompresi |
| Komprimované |Není v mezipaměti |Komprimované |CDN provede kompresi, pokud zdroj vrátí nekomprimovaného souboru. **Azure CDN společnosti Verizon** předá nekomprimovaného souboru na první požadavek a pak komprimuje a ukládá do mezipaměti souborů pro následné požadavky. Soubory s Cache-Control: nikdy jsou komprimované záhlaví bez mezipaměti. |
| Nekomprimované |Komprimované |Nekomprimované |Při dekompresi provede CDN |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

## <a name="media-services-cdn-compression"></a>Komprese CDN služby Media Services
Pro koncové body povolené pro streamování Media Services CDN je ve výchozím nastavení pro následující typy MIME povolená komprese: 
- application/vnd.ms-sstr+xml 
- application/dash+xml
- application/vnd.apple.mpegurl
- application/f4m+xml. 

Nemůžete povolit nebo zakázat komprese pro tyto typy MIME pomocí portálu Azure.  

## <a name="see-also"></a>Další informace najdete v tématech
* [Řešení potíží s kompresí souborů v síti CDN](cdn-troubleshoot-compression.md)    

