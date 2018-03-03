---
title: "Optimalizace stahování velkých souborů pomocí Azure CDN"
description: "Optimalizace vysvětlené podrobněji stahování velkých souborů"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 6e6266fdaaac6a1a1a5d3a5595c10f79fd9f01a7
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="large-file-download-optimization-via-azure-cdn"></a>Optimalizace stahování velkých souborů pomocí Azure CDN

Velikosti souborů obsahu doručit přes Internet pořád roste s tím kvůli rozšířené funkce, vylepšené grafika a bohatý mediální obsah. Tento nárůst vycházejí z hlavních faktorů: širokopásmové průnikům, větší zařízení nenákladné úložiště, zvýšení rozšířeným vysokým rozlišením video a připojené k Internetu zařízení (IoT). Doručení rychlé a efektivní mechanismus pro velkých souborů je velmi důležité, aby bylo prostředí hladký a zábavná příjemce.

Doručování velkých souborů má několik problémy. Průměrná doba stahování velký soubor nejdřív může být důležité, protože všechna data aplikace nemusí stahovat postupně. V některých případech může aplikace stahovat poslední část souboru před první část. Pokud se požaduje jenom malé množství soubor nebo nastavení stahování, stahování může selhat. Stahování také může dojít ke zpoždění až po síti pro doručování obsahu (CDN) načte celý soubor ze zdrojového serveru. 

Druhý latenci mezi počítačem uživatele a soubor určuje rychlost, kdy se může zobrazit obsah. Kromě toho problémy zahlcení a kapacitu sítě také ovlivnit propustnost. Větší vzdálenosti mezi servery a uživatelé vytvoření dalších možností pro ke ztrátě paketů odesílaných dojde k, což snižuje kvality. Snížení kvality způsobené omezená propustnost a ztráta paketů vyšší může se prodloužit doba čekání stahování souborů k dokončení. 

Třetí mnoho velkých souborů nejsou doručeny jako celek. Uživatelé mohou zrušit uprostřed stahování nebo si pusťte pouze první několik minut dlouhé video MP4. Proto software a média doručení společnosti chcete poskytovat jenom ta část souboru, který je požadovaný. Efektivní distribuci požadované části snižuje výchozí přenos ze zdrojového serveru. Efektivní distribuční zmenšuje paměti a vstupu a výstupu tlak na původním serveru. 

Azure Content Delivery Network společnosti Akamai teď nabízí funkce, která poskytuje velkých souborů efektivně uživatelům po celém světě ve velkém měřítku. Funkci snižuje latenci, protože snižuje zatížení serverů původu. Tato funkce je k dispozici s Akamai Standard cenová úroveň.

## <a name="configure-a-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Konfigurace koncového bodu CDN k optimalizaci doručování velkých souborů

Můžete nakonfigurovat koncový bod CDN k optimalizaci doručování velkých souborů prostřednictvím portálu Azure. K tomu můžete také použít rozhraní API REST nebo některého z klienta sady SDK. Následující kroky ukazují proces prostřednictvím portálu Azure.

1. Chcete-li přidat nový koncový bod na **profil CDN** vyberte **koncový bod**.

    ![Nový koncový bod](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. V **optimalizované pro** rozevíracího seznamu vyberte **stahování velkých souborů**.

    ![Vybrané optimalizace velkých souborů](./media/cdn-large-file-optimization/02_Creating.png)


Po vytvoření koncového bodu CDN, bude se vztahovat optimalizace velkých souborů pro všechny soubory, které splňují určitá kritéria. Následující část popisuje tento proces.

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-akamai"></a>Optimalizace pro doručování velkých souborů s Azure Content Delivery Network společnosti Akamai

Typ funkce optimalizace velkých souborů zapne optimalizace sítě a konfigurace k poskytování velkých souborů rychlejší a více responsively. Obecné webové přenosy s Akamai pouze následující 1,8 GB soubory ukládá do mezipaměti, a můžete soubory tunelového propojení (ne mezipaměť) až 150 GB. Velkých souborů optimalizace mezipaměti souborů až do 150 GB.

Optimalizace velkých souborů je účinný, pokud jsou splněny určité podmínky. Podmínky zahrnují, jak funguje na zdrojový server a velikosti a typy souborů, které jsou požadovány. Předtím, než se nám získat do podrobnosti o těchto tématech, byste měli porozumět, jak funguje optimalizace. 

### <a name="object-chunking"></a>Objekt rozdělování 

Azure Content Delivery Network společnosti Akamai využívá techniku, názvem rozdělování objektu. Pokud se požaduje velký soubor, CDN načte menší části souboru z tohoto počátku. Jakmile serveru edge POP CDN obdrží žádost o úplné nebo rozsah bajtů souboru, zkontroluje, zda pro tato optimalizace je podporován typ souborů. Také zkontroluje, zda typ souboru splňuje požadavky na velikost souboru. Pokud velikost souboru je větší než 10 MB, požaduje CDN edge server soubor z tohoto počátku v bloky 2 MB. 

Po bloku dat dorazí na hranici CDN, nemá v mezipaměti a okamžitě poskytovaného pro uživatele. CDN pak prefetches další blok paralelně. Toto předběžné načtení zajistí, že obsah zůstane jeden blok před uživatele, což snižuje latence. Tento proces pokračuje, dokud celý stažení souboru (je-li požadována), všechny rozsahů bajtů jsou k dispozici (je-li požadována), nebo klienta, ukončí připojení. 

Další informace o žádosti o rozsah bajtů, najdete v části [RFC 7233](https://tools.ietf.org/html/rfc7233).

CDN ukládá do mezipaměti všechny bloky dat po přijetí. Celý soubor nemá ukládat do mezipaměti v mezipaměti CDN. Odeslání dalších žádostí o souboru nebo bajtů rozsahy se zpracovávají z mezipaměti CDN. Není-li všechny bloky dat jsou do mezipaměti na CDN, předběžné načtení slouží k vyžádání bloků dat z tohoto počátku. Tato optimalizace spoléhá na možnost na zdrojový server pro podporu požadavků rozsah bajtů. _Pokud je zdrojový server nepodporuje požadavky rozsah bajtů, optimalizace není platná._ 

### <a name="caching"></a>Mezipaměť
Optimalizace velkých souborů používá jiný výchozí doba ukládání do mezipaměti vypršení platnosti z obecné webové doručení. Rozlišuje ukládání do mezipaměti kladné a záporné ukládání do mezipaměti na základě kódů odpovědi HTTP. Pokud je zdrojový server určuje čas vypršení platnosti prostřednictvím ovládacího prvku mezipaměti nebo vyprší platnost hlavičky v odpovědi, CDN ctí tuto hodnotu. Když soubor odpovídá podmínkám typ a velikost pro tento typ optimalizace počátek neuvádí, CDN použije výchozí hodnoty pro optimalizaci velkých souborů. CDN, jinak používá výchozí hodnoty pro obecné webové doručení.


|    | Obecné web | Optimalizace velkých souborů 
--- | --- | --- 
Ukládání do mezipaměti: kladné <br> HTTP 200, 203, 300, <br> 301, 302 a 410 | 7 dní |1 den  
Ukládání do mezipaměti: záporná <br> HTTP 204, 305, 404, <br> a 405 | Žádná | 1 sekunda 

### <a name="deal-with-origin-failure"></a>Řešení s chybou počátek

Délka časového limitu pro čtení počátek zvyšuje ze dvou sekund k doručení obecné webové do dvou minut pro typ optimalizace velkých souborů. Toto zvýšení účty pro velkých souborů, aby se zabránilo předčasné vypršení časového limitu připojení.

Pokud vyprší časový limit připojení, CDN opakuje několikrát, než odešle "504 – časový limit brány" Chyba do klienta. 

### <a name="conditions-for-large-file-optimization"></a>Podmínky pro optimalizaci velkých souborů

Následující tabulka uvádí sadu kritérií, které je třeba splnit pro optimalizaci velkých souborů:

Podmínka | Hodnoty 
--- | --- 
Podporované typy souborů | 3g, 2, 3gp, amp, avi, bz2, dmg, exe, f4v, flv, <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, serveru mts, pkg, RT, rm, swf, vkládání, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minimální velikost souboru | 10 MB. 
Maximální velikost souboru | 150 GB 
Vlastnosti serveru počátek | Žádosti o rozsah bajtů musí podporovat. 

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-verizon"></a>Optimalizace pro doručování velkých souborů s Azure Content Delivery Network od společnosti Verizon

Azure Content Delivery Network od společnosti Verizon nabízí velké soubory bez cap na velikost souboru. Další funkce jsou zapnuté ve výchozím nastavení rychleji doručování velkých souborů.

### <a name="complete-cache-fill"></a>Dokončení mezipaměti výplně

Funkce Výchozí dokončení mezipaměti výplně umožňuje od CDN k vyžádání obsahu souboru do mezipaměti, při opuštění nebo ke ztrátě počáteční požadavku. 

Výplně dokončení mezipaměti je nejvhodnější pro velké prostředky. Většinou uživatelé je nemusíte stahovat od začátku do konce. Používají progresivní stahování. Výchozí chování vynutí hraničního serveru k zahájení načítání na pozadí prostředku ze zdrojového serveru. Potom prostředku je v hraniční server místní mezipaměti. Po úplné objekt je v mezipaměti, edge server plnit požadavky rozsah bajtů k CDN pro objekt uložená v mezipaměti.

Výchozí chování lze zakázat pomocí stroj pravidel ve vrstvě Verizon Premium.

### <a name="peer-cache-fill-hot-filing"></a>Sdílená mezipaměť vyplnění horkou podání

Funkce výchozích sdílené mezipaměti výplně horkou podání používá sofistikované vlastního algoritmu. Používá další hraniční ukládání do mezipaměti servery založené na šířku pásma a agregace požadavků metriky, aby vyplnila požadavky klienta pro velkých, vysoce oblíbených objekty. Tato funkce zabraňuje situaci, v níž jsou odesílány velký počet navíc požadavků uživatele zdrojový server. 

### <a name="conditions-for-large-file-optimization"></a>Podmínky pro optimalizaci velkých souborů

Ve výchozím nastavení jsou zapnuté funkce optimalizace pro Verizon. Neexistují žádná omezení na maximální velikost souboru. 

## <a name="additional-considerations"></a>Další aspekty

Vezměte v úvahu následující další aspekty pro tento typ optimalizace.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Síť pro doručování obsahu Azure společnosti Akamai

- Proces bloku dat vygeneruje další požadavky na zdrojový server. Celkový objem dat doručit od počátku je však mnohem menší. Bloku dat za následek lepší ukládání do mezipaměti charakteristiky v CDN.

- Paměť a vstupně-výstupních operací přetížení jsou omezeny na počátku vzhledem k tomu, že jsou doručovány na menší části souboru.

- U bloky dat uložené v mezipaměti v CDN neexistují žádné další požadavky na počátek až vyprší platnost obsahu nebo je vyřazena z mezipaměti.

- Mohou uživatelé provádět požadavky na rozsah k CDN a jste zacházet jako jakýkoli normální soubor. Optimalizace platí jenom v případě, že je platný typ souboru a rozsah bajtů je mezi 10 MB a 150 GB. Pokud je požadovaná velikost průměrná soubor je menší než 10 MB, můžete místo toho použijte obecné webové doručení.

### <a name="azure-content-delivery-network-from-verizon"></a>Doručování obsahu Azure sítě od společnosti Verizon

Daný typ doručení optimalizace obecné webové doručovat velkých souborů.
