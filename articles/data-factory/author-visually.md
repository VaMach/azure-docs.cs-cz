---
title: "Vizuální vytvářet objekty pro vytváření dat Azure | Microsoft Docs"
description: "Naučte se vytvářet vizuálně Azure Data Factory"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Vizuální Autor datové továrny
S prostředím Azure Data Factory UX uživatelé vizuálně vytvářet a nasazujte prostředky v jejich objekt pro vytváření dat bez nutnosti napsat jediný řádek kódu. Toto rozhraní bez kódu umožňuje přetáhněte a odpojit aktivity v kanálu plátno, provést testovací spouští, interaktivně, ladění a nasazení a monitorování vašeho kanálu spustí. Můžete použít nástroj ADF UX dvěma způsoby:

1. Pracovat přímo s služba Data Factory
2. Konfigurace služby VSTS integrace Gitu pro spolupráci, Správa zdrojového kódu a správa verzí

## <a name="authoring-with-data-factory"></a>Vytváření pomocí služby Data Factory
První možností je vytváření přímo s režimem Data Factory. Tento postup se liší od vytváření prostřednictvím služby VSTS úložiště kódu v tomto neexistuje žádné úložiště ukládání JSON entity změny, ani je optimalizovaná pro spolupráci nebo Správa verzí.

![Konfigurace objektu pro vytváření dat](media/author-visually/configure-data-factory.png)

V režimu pro vytváření dat je pouze režim "Publikovat". Všechny změny, které jsou publikovány přímo ke službě Data Factory.

![Publikování služby Data Factory](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>Vytváření s integrací služby VSTS Git
Vytváření s integrací služby VSTS Git umožňuje zdrojového kódu a spolupráce při vytváření objektu pro vytváření datových kanálů. Uživatelé mají možnost přidružit objekt pro vytváření dat účet služby VSTS Git úložiště správy zdrojového kódu, spolupráce a správa verzí atd. Jeden účet služby VSTS GIT může mít několik úložiště. Však úložiště Git služby VSTS lze přidružit pouze pomocí jednoho data factory. Pokud nemáte účet služby VSTS a úložiště už, vytvořit [zde](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student).

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>Konfigurace úložiště Git služby VSTS s Azure Data Factory
Uživatelé můžou konfigurovat úložiště GIT služby VSTS objekt pro vytváření dat pomocí dvou metod.

#### <a name="method-1-lets-get-started-page"></a>Metoda 1:, můžeme začít' stránky

Přejít na stránku, můžeme začít a klikněte na tlačítko Konfigurovat úložiště kódu

![Konfigurace úložiště kódu](media/author-visually/configure-repo.png)

Z ní se zobrazí vedle panel pro konfiguraci nastavení úložiště.

![Konfigurovat nastavení úložiště](media/author-visually/repo-settings.png)
* **Typ úložiště**: Visual Studio Team Services Git (v současné době Githubu není podporována.)
* **Visual Studio Team Services účet**: název účtu naleznete z názvu https://{account}. visualstudio.com. Přihlaste se k účtu služby VSTS [zde](https://www.visualstudio.com/team-services/git/) a přístup k sadě Visual Studio profilu můžete najdete v části úložiště a projekty
* **Název projektu:** název projektu nelze nalézt název name}.visualstudio.com/{project https://{account}
* **RepositoryName:** název úložiště. Služby VSTS projekty obsahují úložiště Git ke správě zdrojového kódu s růstem projektu. Vytvořit nové úložiště nebo použití existující úložiště už v projektu.
* **Import existujících prostředků pro vytváření dat do úložiště**: Zaškrtnutím tohoto políčka můžete importovat vaše aktuální prostředky objekt pro vytváření dat vytvořené na plátno UX přidružené úložiště GIT služby VSTS ve formátu JSON. Tato akce exportuje každého prostředku jednotlivě (to znamená, propojené služby a datové sady exportují do samostatné JSONs).    Pokud zaškrtnutí tohoto políčka zrušte, existující prostředky nejsou naimportovány do úložiště Git.

#### <a name="method-2-from-authoring-canvas"></a>Metoda 2: Z vytváření plátno

V vytváření plátno klikněte v části název objektu pro vytváření dat rozevírací nabídky služby Data Factory. Potom klikněte na "Konfigurace úložiště kódu." Podobně jako **metoda 1**, straně panelu se zobrazí pro konfiguraci nastavení úložiště. Naleznete v předchozích částech informace o nastavení.

![Konfigurace úložiště kódu 2](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Správa verzí
Správa verzí, nazývaná také jen na jako správa zdrojového kódu systémů umožňují vývojářům spolupracovat na kód a sledovat změny provedené kód základní. Správa zdrojového kódu je základní nástroj pro projekty více vývojáři.

Každý úložiště Git služby VSTS jednou přidružený objekt pro vytváření dat má hlavní větve. Odtud, každý uživatel, který má přístup do úložiště Git služby VSTS má dvě možnosti při provádění změn: synchronizace a publikovat.

![Publikování synchronizace](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Sync

Po kliknutí na tlačítko 'synchronizace', můžete změny z hlavní větve místní větev pro vyžádání obsahu, nebo doručte změny z vaší místní větve do hlavní větve.

![Synchronizuje změny](media/author-visually/sync-change.png)

#### <a name="publish"></a>Publikování
 Publikujte změny v hlavní větvi do služby Data Factory.

> [!NOTE]
> **Hlavní větve se nemusí shodovat s co je nasazen ve službě Data Factory.** Hlavní větve *musí* ke službě Data Factory publikovat ručně.




## <a name="expression-language"></a>Jazyk výrazů

Uživatelé mohou určit výrazy k definování hodnoty vlastností pomocí výrazu jazyka podporovaných službou Azure Data Factory. V tématu [výrazy a funkce v Azure Data Factory](control-flow-expression-language-functions.md) pro více o tom, které jsou podporovány výrazy.

Zadejte výrazy v hodnotách vlastnosti v uživatelského takto.

![Jazyk výrazů](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parametry
Uživatelé mohou určit parametry pro kanálů a datové sady, na kartě Parametry.". Kromě toho parametry ve vlastnostech snadno využívat stisknutím kombinace kláves "Přidat dynamický obsah."

![Dynamický obsah](media/author-visually/dynamic-content.png)

Odtud můžete využívat existující parametr nebo zadejte nový parametr ve vašem hodnotu vlastnosti.

![Parametry](media/author-visually/parameters.png)

## <a name="feedback"></a>Váš názor
Klikněte na ikonu 'zpětnou vazbu, sdělte nám svůj názor (Microsoft) na různých funkcí nebo potíže, které se setkávají.

![Váš názor](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Další postup

Další informace o monitorování a Správa kanálů najdete v tématu [monitorování a Správa kanálů prostřednictvím kódu programu](monitor-programmatically.md) článku
