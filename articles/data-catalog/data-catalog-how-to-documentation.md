---
title: "Postup dokumentování zdrojů dat | Microsoft Docs"
description: "Postupy: článek zvýraznění postup dokumentování datových prostředcích v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 1901ac534c46e4de0446543de4a40de040b8d51e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="document-data-sources"></a>Dokumentování zdrojů dat
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy **Azure Data Catalog** se točí kolem pomoci zjistit, osoby *pochopit*, použijte zdroje dat a pomáhá organizacím získat větší hodnotu z jejich stávající data.

Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou, ale není článek existuje ukončení. **Azure Data Catalog** také umožňuje uživatelům poskytnout vlastní kompletní dokumentaci popisující běžné scénáře pro zdroj dat a využití.

V [postup přidání poznámek ke zdrojům dat](data-catalog-how-to-annotate.md), zjistíte, že odborníky, kteří znají zdroj dat může opatřit poznámkami ho pomocí značek a popis. **Azure Data Catalog** portál obsahuje bohatou textového editoru, aby uživatelé můžete plně dokumentů datových prostředků a kontejnery. Editor zahrnuje formátování odstavce, jako je například záhlaví, formátování textu, seznamy s odrážkami, číslované seznamy a tabulky.

Značky a popisy jsou ideální pro jednoduché poznámky. Chcete-li spotřebitelé dat se lépe pochopit zdroje dat a obchodní scénáře použití pro zdroj dat, ale odborník může poskytovat dokončení, podrobnou dokumentaci. Je snadné zdroj dat dokumentu. Vyberte datový prostředek nebo kontejner a vyberte **dokumentaci**.

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a>Dokumentace datových prostředků
Výhodou **Azure Data Catalog** dokumentaci vám umožňuje používat Data Catalog jako obsahu úložiště k vytvoření komentáře dokončení datových prostředků. Můžete si prostudovat podrobné obsah, který popisuje kontejnery a tabulek. Pokud již máte obsah v úložišti jiného obsahu, například SharePoint nebo sdílenou složku, můžete přidat do odkazy dokumentace asset odkazovat na tuto existující obsah. Díky této funkci existující dokumenty zjistitelná.

> [!NOTE]
> Dokumentace není součástí indexu vyhledávání.
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

Úroveň dokumentaci rozsahu popisující vlastnosti a hodnotu kontejner dat asset k podrobný popis schématu tabulky v rámci kontejneru. Úroveň dokumenty poskytnuté by měl být doprovází vašim obchodním potřebám. Ale obecně uvádíme několik výhody a nevýhody dokumentace datových prostředků:

* Zdokumentujte právě kontejner: veškerý obsah na jednom místě, ale může nedostatku potřebné detaily uživatelům provést informované rozhodnutí.
* Zdokumentujte právě tabulky: obsah je specifické pro tento objekt, ale mají vaši uživatelé více míst pro dokumenty.
* Zdokumentujte kontejnery a tabulky: nejvíce komplexní přístup, ale může způsobit další údržby dokumentů.

## <a name="summary"></a>Souhrn
Dokumentace zdroje dat s **Azure Data Catalog** můžete vytvořit komentáře o datových prostředcích v množství podrobností, kolik potřebujete.  Pomocí odkazů můžete propojit na obsah uložený v existující úložišti obsahu, který spojuje vaše stávající dokumenty a data prostředků. Jakmile vaši uživatelé zjistit prostředky příslušná data, mohou mít úplnou sadu dokumentace.
