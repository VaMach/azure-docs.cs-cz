---
title: "Optimalizace doručování obsahu Azure pro váš scénář"
description: "K optimalizaci doručování obsahu pro konkrétní scénáře"
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
ms.openlocfilehash: 98941c49b057380b3ef9164515bcc2a63ccb56ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optimalizace doručování obsahu Azure pro váš scénář

Při doručování obsahu do velkou globální cílovou skupinu, je důležité zajistit optimalizované doručování obsahu. Sítě pro doručování obsahu Azure můžete optimalizovat prostředí doručení založené na typ obsahu, které máte. Obsah může být web, živý datový proud, video nebo velký soubor ke stažení. Když vytvoříte koncový bod doručování obsahu (CDN) sítě, zadáte scénář v **optimalizované pro** možnost. Vaše volba určuje, které optimalizace se použije k obsahu doručit z koncového bodu CDN.

Možnosti optimalizace navržena pro použití chování osvědčené postupy pro zlepšení výkonu doručování obsahu a snižování zátěže lepší původu. Vaše volby scénář ovlivnit výkon změnou konfigurace pro částečné ukládání do mezipaměti, objekt rozdělování a zásady opakování selhání původu. 

Tento článek obsahuje přehled různých funkcí optimalizace a kdy byste měli použít. Další informace o funkcích a omezení naleznete v příslušných článcích na každý typ jednotlivých optimalizace.

> [!NOTE]
> Vaše **optimalizované pro** možnosti může lišit v závislosti na zprostředkovateli vyberete. Vylepšení zprostředkovatele CDN použít různými způsoby v závislosti na scénáři. 

## <a name="provider-options"></a>Možnosti zprostředkovatele

Podporuje Azure Content Delivery Network společnosti Akamai:

* Obecné webové doručení 

* Obecné streamování médií

* Streamování videa na přání médií

* Stažení velkých souborů

* Akcelerace dynamických webů 

Azure Content Delivery Network od společnosti Verizon podporuje pouze obecné webové doručení. Slouží pro video na vyžádání a stahování velkých souborů. Nemáte chcete vybrat typ optimalizace.

Důrazně doporučujeme, abyste otestovali výkonu rozdíly mezi různé zprostředkovatele můžete vybrat optimální zprostředkovatele pro vaše doručení.

## <a name="select-and-configure-optimization-types"></a>Vyberte a nakonfigurujte optimalizace typy

Pokud chcete vytvořit nový koncový bod, vyberte typ optimalizace, který nejlépe odpovídá scénář a typ obsahu, který chcete, aby koncový bod pro doručování. **Obecné webové doručení** je výchozí výběr. Můžete aktualizovat možnosti optimalizace pro jakékoli existující koncový bod Akamai kdykoli. Tato změna nemá přerušit doručování z CDN. 

1. Vyberte koncový bod v rámci profilu Standard Akamai.

    ![Výběr koncového bodu ](./media/cdn-optimization-overview/01_Akamai.png)

2. V části **nastavení**, vyberte **optimalizace**. Pak vyberte typ z **optimalizované pro** rozevíracího seznamu.

    ![Optimalizace a typ výběru](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalizace pro konkrétní scénáře

Koncový bod CDN pro jednu z následujících scénářů, můžete optimalizovat. 

### <a name="general-web-delivery"></a>Obecné webové doručení

Obecné webové doručení je nejběžnější možnosti optimalizace. Je určený pro optimalizaci obecné webového obsahu, například webových stránek a webových aplikací. Tato optimalizace můžete použít také pro soubor, a stáhne video.

Typické webu obsahuje obsah statické a dynamické. Statický obsah zahrnuje obrázků, knihovny jazyka JavaScript a stylů, které lze do mezipaměti a doručit různým uživatelům. Dynamický obsah je přizpůsobené pro jednotlivé uživatele, například položky zpráv, které jsou přizpůsobené profilu uživatele. Dynamický obsah neukládá do mezipaměti, protože je jedinečný pro každého uživatele, třeba nákupní košík obsah. Obecné webové doručení můžete optimalizovat celý web. 

> [!NOTE]
> Pokud používáte Azure Content Delivery Network společnosti Akamai, můžete chtít použít optimalizace, pokud vaše Průměrná velikost je menší než 10 MB. Pokud vaše Průměrná velikost souboru je větší než 10 MB, vyberte **stahování velkých souborů** z **optimalizované pro** rozevíracího seznamu.

### <a name="general-media-streaming"></a>Obecné streamování médií

Pokud budete muset použít koncový bod pro živé streamování a streamování videa na přání, doporučujeme streamování optimalizace obecné médií.

Datové proudy Media je čas citlivé, protože paketů, které přicházejí pozdní na straně klienta může způsobit sníženou zobrazení prostředí, jako je například časté ukládání do vyrovnávací paměti obsahu videa. Optimalizace streamování médií snižuje latenci doručování obsahu pro média a poskytuje technologie smooth streaming prostředí pro uživatele. 

Tento scénář je běžný u Zákazníci využívající službu Azure media. Když používáte službu Azure media services, zobrazí jeden streamování koncový bod, který lze použít pro datové proudy živě i na vyžádání. V tomto scénáři není nutné přepnout na jiný koncový bod, když se změní ze za provozu se streamování na vyžádání zákazníků. Obecné média streamování optimalizace podporuje tento typ scénáře.

Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.

Další informace o optimalizaci streamování médií najdete v tématu [optimalizace streamování médií](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Streamování videa na přání médií

Optimalizace streamování na vyžádání Video-on-Demand média zlepšuje streamování obsahu na vyžádání video-on-demand. Pokud používáte koncový bod pro vyžádání video-on-demand streamování, můžete chtít použít tuto možnost.

Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.

Další informace o optimalizaci streamování médií najdete v tématu [optimalizace streamování médií](cdn-media-streaming-optimization.md).

> [!NOTE]
> Pokud koncový bod slouží především na vyžádání video-on-demand obsahu, použijte tento typ optimalizace. Hlavní rozdíl mezi optimalizace a streamování optimalizace obecné médií je časový limit opakování připojení. Časový limit je mnohem kratší pro práci s živé streamování scénáře.

### <a name="large-file-download"></a>Stažení velkých souborů

Pokud používáte Azure Content Delivery Network společnosti Akamai, musíte použít velkých souborů stahování dodávat soubory větší než 1,8 GB. Azure Content Delivery Network od společnosti Verizon nemá omezení na soubor stáhnout velikost v jeho doručení optimalizaci obecné webů.

Pokud používáte Azure Network doručování obsahu společnosti Akamai, stahování velkých souborů jsou optimalizované pro obsah větší než 10 MB. Pokud vaše Průměrná velikost souboru je menší než 10 MB, můžete chtít použít obecné webové doručení. Pokud vaše soubory průměrné velikosti jsou konzistentně větší než 10 MB, může to být efektivnější vytvořit samostatné koncový bod pro velkých souborů. Aktualizace firmwaru a softwaru jsou například obvykle velkých souborů.

Síť doručování obsahu Azure od společnosti Verizon používá daný typ doručení optimalizace obecné webové při doručování obsahu streamování médií.

Další informace o optimalizaci velkých souborů najdete v tématu [velkých souborů optimalizace](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Akcelerace dynamických webů

 Akcelerace dynamických webů je k dispozici z Akamai a Verizon Content Delivery Network profily. Tato optimalizace zahrnuje další poplatek používat. Další informace najdete na stránce s cenami.

Akcelerace dynamických webů zahrnuje různé techniky využívající latenci a výkonu dynamického obsahu. Mezi dostupné techniky patří trasy a síťové optimalizace, optimalizace TCP a další. 

Tato optimalizace vám pomůže urychlit webovou aplikaci, která zahrnuje mnoho odpovědí, které nejsou lze uložit do mezipaměti. Příklady jsou výsledky hledání, najdete v článku věnovaném transakce nebo dat v reálném čase. Můžete nadále používat možnosti ukládání do mezipaměti jádra CDN statických dat. 



