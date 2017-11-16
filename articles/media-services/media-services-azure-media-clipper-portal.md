---
title: "Pomocí Azure Media výřez na portálu | Microsoft Docs"
description: "Vytvořit klipy pomocí Azure Media výřez z portálu Azure"
services: media-services
keywords: "klip; subclip; kódování; média"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: faaae7edbc2fb62ae219dd963f405e7246c982d4
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Vytvoření klipů s Azure Media výřez v portálu.
Azure Media výřez portálu slouží k vytvoření klipů z prostředků ve svých účtů media services. Chcete-li začít, přejděte na svůj účet mediálních služeb v portálu. Potom vyberte **Subclip** kartě.

Na **Subclip** karty, budete moci začít skládání klipů. Na portálu výřez načte soubory MP4 s jednou přenosovou rychlostí rychlostmi, soubory MP4 s více přenosovými rychlostmi a archivy za provozu, které jsou publikovány s platný Lokátor streamování. Publikování prostředků nebyly načteny.

## <a name="producing-clips"></a>Vytváření klipů
Vytvořit klip, přetažením prostředek na rozhraní klip. Pokud jsou známé označit časy, můžete je zadat ručně do rozhraní. Jinak hodnota přehrávání asset nebo přetáhněte počátek přehrávání najít požadované značky v a čas. Pokud není k dispozici čas začátku nebo konce, klip začne od začátku nebo pokračuje na konec vstupu prostředku, v uvedeném pořadí.

Přejděte s rámečkem přesnost nebo GOP-přesnost, použijte tlačítko Předat dál/GOP-převíjení nebo rámce zpětné/GOP-zpátky. Pro výstřižek proti více prostředků, přetažení více prostředků do rozhraní klip z panelu Výběr asset. Můžete vybrat a změnit pořadí prostředky v rozhraní požadované pořadí. Panel výběru asset poskytuje asset trvání, typ a překlad metadat pro každý prostředek. Pokud společně zřetězení více prostředků, zvažte rozlišení zdroje každého vstupního souboru. Pokud zdroj řešení liší, je nižší rozlišení vstup upscaled ke splnění řešení nejvyšší prostředku řešení. Chcete-li zobrazit náhled výstup úlohy výstřižek, vyberte tlačítko Náhled a klip přehrává z vybrané označit časy.

## <a name="producing-dynamic-manifest-filters"></a>Vytváření dynamické filtry manifestu
[Dynamické filtry manifestu](https://azure.microsoft.com/blog/dynamic-manifest/) popisují sadu pravidel na základě manifestu atributy a časová osa asset. Tato pravidla určují, jak koncový bod streamování zpracovává seznam stop výstup (manifestu). Filtr lze změnit, které segmenty streamovaných pro přehrávání. Filtry vyprodukované výřez jsou místní filtry a jsou specifické pro daný prostředek zdroje. Na rozdíl od vykreslené klipů filtry nejsou nové prostředky a nevyžadují úlohu kódování k vytvoření. Může být rychle vytvořené prostřednictvím [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) nebo [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), je však pouze GOP přesné. Prostředky kódováním pro streamování obvykle mají velikost GOP dvou sekund.

K vytvoření dynamických manifestu filtru, vyberte jako výstřižek režim z nabídky Rozšířené nastavení dynamické manifestu filtru. Můžete provést stejný postup k vytvoření klip k vytvoření filtru rozhraní. Filtry je možné vytvořit jenom u jednoho datového zdroje.

## <a name="submitting-clipping-jobs"></a>Odesílání úloh výstřižek
Po skončení skládání klip, vyberte tlačítko pro spuštění úlohy odeslání odpovídající úloha výstřižek nebo dynamické manifestu volání.