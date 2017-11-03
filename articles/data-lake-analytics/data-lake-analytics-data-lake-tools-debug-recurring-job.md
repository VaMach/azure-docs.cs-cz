---
title: "Řešení potíží s neobvyklé opakování úlohy | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio k ladění neobvyklé opakování úlohy."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: a358f94b117c12511028a875e56b5c9dba8d3382
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-an-abnormal-recurring-job"></a>Řešení potíží s neobvyklé opakování úlohy

V tomto dokumentu jsme vás seznámí použití [nástrojů Azure Data Lake pro Visual Studio](http://aka.ms/adltoolsvs) k řešení problémů opakované úlohy. Další informace o kanálu a opakované úlohy z [zde](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).
Opakované úlohy obvykle sdílet stejný dotaz logiku a podobně jako vstupní data. Například máte opakování úlohy spuštěné v každé pondělí ráno v 8: 00 počítat minulého týdne týdenní aktivního uživatele, skriptů pro tyto úlohy sdílet jedna šablona skript, který obsahuje logiku dotazu a vstupních hodnot pro tyto úlohy se dat o využití za poslední týden. Sdílení stejný dotaz logiku a podobně jako vstupní obvykle znamená výkon tyto úlohy je podobný a stability, pokud jeden z opakované úlohy najednou provádí neobvyklé, se nezdařilo nebo zpomalit mnoho, můžete chtít:

1.  Zobrazení statistiky hlášení pro verze Preview spustí opakované úlohy, které chcete zobrazit, co se stalo.
2.  Porovnejte neobvyklé úlohy s normální jeden zjistěte, co se změnilo.

**Zobrazení úloh souvisejících s** v nástroje Azure Data Lake pro Visual Studio pomůže urychlit proces řešení potíží s oba případy.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Krok 1: Najít opakované úlohy a otevřete zobrazení souvisejících úloh

K použití související úlohy zobrazení řešení problému opakované úlohy, musíte nejprve najít opakované úlohy v sadě Visual Studio a pak otevřete zobrazení související úlohy.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Případ 1: Máte adresu URL pro opakované úlohy

Prostřednictvím **nástroje > Data Lake > zobrazení úloh**, můžete vložit adresu URL úlohy otevřete zobrazení úloh v sadě Visual Studio a zobrazení souvisejících úloh otevřete související s zobrazení úloh.

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Případ 2: Máte kanálu pro opakované úlohy, avšak nikoliv adresu URL

V sadě Visual Studio, můžete otevřít kanál prohlížeče prostřednictvím **Průzkumníka serveru > váš účet Data Lake Analytics > kanály** (Pokud tento uzel nemůže najít v Průzkumníku serveru, získejte prosím nástroj lasted [sem](http://aka.ms/adltoolsvs)). V prohlížeči kanálu, jsou uvedeny všechny kanály pro účet ADLA na levé straně, můžete rozšířit kanály, které chcete najít všechny opakované úlohy, klikněte na ten má potíže, související zobrazení úloh otevře vpravo.

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-statistics-report"></a>Krok 2: Analýza sestavy statistik

Souhrn a sestavy statistik se zobrazí v horní části související zobrazení úloh, pomocí kterého můžete získat potenciální hlavní příčinu neobvyklé. 

1.  Nejprve budete muset najít neobvyklé úlohu v sestavě. Ukazuje osy X čas odeslání úlohy, pomocí kterého můžete vyhledat úlohu neobvyklé.
2.  Postupujte podle níže procesu ke kontrole statistiky a získejte přehled o neobvyklé a možná řešení.

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-recurring-job-to-a-normal-job"></a>Krok 3: Porovnání na normální úlohy neobvyklé opakované úlohy

Můžete najít všechny předat opakované úlohy prostřednictvím seznam úloh v dolní části zobrazení související úlohy. Prostřednictvím klikněte pravým tlačítkem, můžete porovnat neobvyklé úlohy s předchozí normální jeden další statistiky a možná řešení najdete v zobrazení úlohy rozdílové.

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Obvykle je potřeba věnovat pozornost velké rozdíly mezi tyto 2 úlohy, jako jsou pravděpodobně důvodů, proč způsobují problémy s výkonem a můžete také provést následující kroky, jak provést další kontroly.

![Zobrazit související úlohy nástrojů data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Další kroky

* [Ladění a vyřešte problémy zkosení dat](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Postup ladění selhání úlohy U-SQL pro uživatelem definovaný kód chyby](data-lake-analytics-debug-u-sql-jobs.md)