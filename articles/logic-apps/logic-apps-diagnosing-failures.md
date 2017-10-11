---
title: "Diagnostikování selhání - Azure Logic Apps | Microsoft Docs"
description: "Běžné způsoby, abyste pochopili, kde dochází k selhání aplikace logiky"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a>Diagnostikování selhání aplikace logiky
Pokud máte selhání nebo problémy s logic apps, existuje několik přístupů vám může pomoct lépe pochopit, kde jsou chyby pocházejících z.  

## <a name="azure-portal-tools"></a>Nástroje portálu Azure
Portál Azure poskytuje celou řadu nástrojů pro diagnostiku každé logiku aplikace při každém kroku.

### <a name="trigger-history"></a>Aktivační událost historie

Každá aplikace logiky má nejméně jedna aktivační událost. Pokud si všimnete, že aplikace se ohlásí, nejprve vyhledává v historii aktivační událost pro další informace. Historie aktivační události v hlavním okně app'ss logiky můžete přistupovat.

![Vyhledání historie aktivační události][1]

Historie aktivační události obsahuje seznam všech pokusů o aktivační události, které aplikace logiky udělat. Můžete kliknout na jednotlivé pokusy o aktivační události přejít na podrobnosti, konkrétně žádný vstupů nebo výstupů, které generuje pokus o aktivační události. Pokud zjistíte, se nezdařila aktivace, vyberte pokus o aktivační události a zvolte **výstupy** odkaz zobrazíte všechny vygenerované chybové zprávy, například přihlašovací údaje serveru FTP, které nejsou platné.

Různé stavy, mohou být zobrazeny jsou:

* **Vynecháno**. Koncový bod byl dotazování vyhledávat data a obdržel odpověď, nebyla dostupná žádná data.
* **Úspěšné**. Aktivační událost obdržel odpověď, nebyla dostupná data. Tento stav může být výsledkem ruční aktivační událost, aktivační událost opakování nebo cyklického dotazování aktivační událost. Tento stav je obvykle přiložena **Fired** stav, ale možná, pokud máte v zobrazení kódu, která nebyla splněná podmínka nebo SplitOn příkaz.
* **Se nezdařilo**. Byla generována chyba.

#### <a name="start-a-trigger-manually"></a>Ruční spuštění aktivační události

Pokud chcete aplikaci logiky zkontrolujte aktivační procedury k dispozici okamžitě bez čekání na další opakování, klikněte na tlačítko **vyberte aktivační událost** v hlavním okně vynuťte kontrolu. Kliknutím na tento odkaz se aktivační událostí Dropbox například pracovní postup k okamžitému dotázání Dropbox pro nové soubory.

### <a name="run-history"></a>Historie spouštění

Všechny aktivní aktivační události výsledkem spustit. Informace o běhu můžete přistupovat z hlavní okno, který obsahuje mnoho podrobnosti, které vám může pomoct pochopit, co se stalo během pracovního postupu.

![Vyhledání historie spouštění][2]

Spustit zobrazí jedno z následujících stavů:

* **Úspěšné**. Všechny akce bylo úspěšné. Pokud došlo k selhání, že selhání zajišťoval akci, která došlo k chybě později v pracovním postupu. To znamená selhání zajišťoval akci, která byla nastavena pro spuštění po selhání akce.
* **Se nezdařilo**. Nejméně jedna akce měl selhání, který nebyl zpracován akcí později v pracovním postupu.
* **Zrušena**. Pracovní postup byl spuštěn, ale přijal žádost o zrušení.
* **Spuštění**. Pracovní postup je právě spuštěna. Tento stav může dojít k omezenému pracovních postupů, nebo z důvodu aktuálního cenový plán. Podrobnosti najdete v tématu [akce omezení na stránce s cenami](https://azure.microsoft.com/pricing/details/app-service/plans/). Konfigurace diagnostiky (grafů, které se zobrazují v části historie spouštění) také může poskytnout informace o všech omezení událostí, které dojít.

Když se díváte historie spouštění, můžete zobrazit další podrobnosti Další podrobnosti.  

#### <a name="trigger-outputs"></a>Výstupy aktivační události

Aktivační událost výstupy zobrazit data, která pochází aktivační událost. Tyto výstupů vám pomohou určit, zda všechny vlastnosti vrátila podle očekávání.

> [!NOTE]
> Pokud se zobrazí veškerý obsah, který neznáte, zjistěte, jak Azure Logic Apps [zpracovává jiné typy obsahu](../logic-apps/logic-apps-content-type.md).
> 

![Příklady výstup aktivační události][3]

#### <a name="action-inputs-and-outputs"></a>Akce vstupy a výstupy

Můžete rozbalit vstupy a výstupy, které přijaly akce. Tato data jsou užitečné pro pochopení velikost a tvar výstupy a také pro hledání chybové zprávy, které byly vytvořeny.

![Akce vstupy a výstupy][4]

## <a name="debug-workflow-runtime"></a>Ladění modulu runtime pracovního postupu

Společně s monitorování vstupy, výstupy a aktivuje spuštění, můžete přidat do pracovního postupu, které pomáhají s laděním některé kroky. 
[RequestBin](http://requestb.in) je výkonný nástroj, který můžete přidat jako krok v pracovním postupu. Pomocí RequestBin můžete nastavit kontrolor požadavku HTTP určit přesnou velikost, tvar a formát požadavku HTTP. Můžete vytvořit RequestBin a vložte adresu URL aplikace logiky akce HTTP POST s obsah textu, který chcete otestovat, například, výraz nebo jiné krok výstup. Po spuštění aplikace logiky můžete obnovit vaše RequestBin zobrazíte jak žádosti byl vytvořen při vygenerování z modulu Logic Apps.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
