---
title: "Analýza využití dat v Log Analytics | Dokumentace Microsoftu"
description: "Pokud chcete zobrazit, kolik dat je odesíláno do služby OMS, můžete k tomu použít řídicí panel Využití v Log Analytics."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/12/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 7e3d4b83fefdc70f292cf85b682cf8ed756bf4c5
ms.openlocfilehash: e7f04df679604f274c8ad9bf4daddc63c8b5418a
ms.contentlocale: cs-cz
ms.lasthandoff: 02/22/2017


---
# <a name="analyze-data-usage-in-log-analytics"></a>Analýza využití dat v Log Analytics
Log Analytics shromažďuje data a pravidelně je odesílá do služby OMS.  Pokud chcete zobrazit, kolik dat je odesíláno do služby OMS, můžete k tomu použít řídicí panel **Využití Log Analytics**. Řídicí panel také ukazuje, kolik dat je odesíláno řešeními a jak často vaše servery data odesílají.

> [!NOTE]
> Pokud máte bezplatný účet, je odesílání dat do služby OMS omezeno na 500 MB denně. Pokud denní limit překročíte, analýza dat se zastaví a bude pokračovat na začátku dalšího dne. V takovém případě bude nutné znovu odeslat veškerá data, která nebyla přijata nebo zpracována v OMS.

Pokud jste denní limit využití překročili nebo se mu blížíte, můžete volitelně odebrat některé řešení a snížit tak množství dat, která se odesílají do služby OMS. Další informace o odebírání řešení najdete v tématu [Přidání řešení Log Analytics z Galerie řešení](log-analytics-add-solutions.md).

![řídicí panel využití](./media/log-analytics-usage/usage-dashboard01.png)

Řídicí panel **Využití Log Analytics** obsahuje tyto informace:

- Objem dat
    - Objem dat v průběhu času (v závislosti na vašem aktuálním časovém rozsahu)
    - Objem dat podle řešení
    - Data, která nejsou spojena s počítačem
- Počítače
    - Počítače odesílající data
    - Počítače, které během posledních 24 hodin neodeslaly žádná data
- Nabídky
    - Uzly Insight and Analytics
    - Uzly Automation and Control
    - Uzly zabezpečení
- Výkon
    - Čas potřebný k shromáždění a indexaci dat
- Seznam dotazů

## <a name="understanding-nodes-for-oms-offers"></a>Vysvětlení uzlů pro nabídky OMS

Pokud jste na cenové úrovni *za uzel (OMS)*, pak se vám bude účtovat v závislosti na počtu uzlů a řešení, které jste povolili. V části *nabídek* řídicího panelu využití můžete zobrazit, kolik uzlů každé nabídky se používá.

![řídicí panel využití](./media/log-analytics-usage/log-analytics-usage-offerings.png)

## <a name="to-work-with-usage-data"></a>Práce s daty o využití
1. Pokud jste to ještě neudělali, přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého předplatného Azure.
2. V nabídce **Centra** klikněte na **Další služby** a v seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Klikněte na**Log Analytics**.  
    ![Centrum Azure](./media/log-analytics-usage/hub.png)
3. Na řídicím panelu **Log Analytics** se zobrazí seznam vašich pracovních prostorů. Vyberte pracovní prostor.
4. Na řídicím panelu *pracovního prostoru* klikněte na **Využití Log Analytics**.
5. Na řídicím panelu **Využití Log Analytics** klikněte na **Čas: Posledních 24 hodin** a změňte časový interval.  
    ![časový interval](./media/log-analytics-usage/time.png)
6. Zobrazte okna kategorií využití s oblastmi, které vás zajímají. Vyberte okno a pak v něm klikněte na některou položku, abyste zobrazili další podrobnosti v [Prohledávání protokolů](log-analytics-log-searches.md).  
    ![příklad okna využití dat](./media/log-analytics-usage/blade.png)
7. Na řídicím panelu Prohledávání protokolů zkontrolujte vrácené výsledky hledání.  
    ![příklad prohledávání protokolů využití](./media/log-analytics-usage/usage-log-search.png)


## <a name="next-steps"></a>Další kroky
* Pokud chcete zobrazit podrobné informace shromážděné a odeslané do OMS funkcemi a řešeními, přečtěte si téma [Prohledávání protokolů v Log Analytics](log-analytics-log-searches.md).

