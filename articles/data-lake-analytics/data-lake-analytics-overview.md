---
title: "Přehled Microsoft Azure Data Lake Analytics | Dokumentace Microsoftu"
description: "Data Lake Analytics je služba Azure pro velké objemy dat, která umožňuje využívat data k podpoře podnikání díky informacím získaným z vašich dat v cloudu, bez ohledu na jejich velikost a umístění."
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 12d6fe834ed2b31a756123351288eec7ba2a72f7
ms.lasthandoff: 03/22/2017


---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Přehled Microsoft Azure Data Lake Analytics
## <a name="what-is-azure-data-lake-analytics"></a>Co je Azure Data Lake Analytics?
Azure Data Lake Analytics je služba analytických úloh na vyžádání pro zjednodušení analýz velkých objemů dat. Soustřeďte se na psaní, spouštění a správu úloh, a nikoli na provoz distribuované infrastruktury. Namísto nasazení, konfigurace a ladění hardwaru můžete psát dotazy, transformovat data a extrahovat cenné informace. Analytická služba dokáže okamžitě zpracovávat úlohy libovolného rozsahu – stačí nastavit, jaký výkon potřebujete. Za úlohu platíte jenom tehdy, když je spuštěná, což je nákladově efektivní. Analytická služba podporuje Azure Active Directory, takže můžete jednoduše spravovat přístup a role integrované do vašeho místního systému identit. Zahrnuje také jazyk U-SQL, který kombinuje výhody jazyka SQL a výrazovou sílu uživatelského kódu. Škálovatelný distribuovaný modul runtime jazyka U-SQL umožňuje efektivně analyzovat data v úložišti a na SQL Serverech v Azure, ve službě Azure SQL Database a Azure SQL Data Warehouse.

## <a name="key-capabilities"></a>Klíčové funkce
* **Dynamické škálování**
  
    Služba Data Lake Analytics je navržena pro cloudové škálování a výkon.  Dynamicky zřizuje prostředky a umožňuje provádět analýzy terabajtů nebo dokonce exabajtů dat. Po dokončení úlohy postupně automaticky ukončí prostředky, takže platíte jenom za využitý výpočetní výkon. Při zvětšování a zmenšování objemu uložených dat nebo množství využitých výpočtů nemusíte přepisovat kód. Můžete se soustředit pouze na obchodní logiku, a nikoli na způsob zpracování a ukládání rozsáhlých datových sad.
* **Rychlejší vývoj a inteligentnější ladění a optimalizace pomocí známých nástrojů**
  
    Služba Data Lake Analytics je těsně integrovaná se sadou Visual Studio, takže můžete spouštět, ladit a optimalizovat kód pomocí známých nástrojů. Vizualizace úloh U-SQL umožňuje sledovat běh kódu v reálných podmínkách a snadno identifikovat kritická místa výkonu a optimalizovat náklady.
* **Jazyk U-SQL: jednoduchý a známý, výkonný a rozšiřitelný**
  
    Data Lake Analytics obsahuje dotazovací jazyk U-SQL, který rozšiřuje dobře známý, jednoduchý a deklarativní charakter jazyka SQL o výrazovou sílu jazyka C#. Jazyk U-SQL je založený na stejném distribuovaném modulu runtime, který využívají systémy velkých objemů dat ve společnosti Microsoft. Miliony vývojářů SQL a .NET teď můžou zpracovávat a analyzovat svá data pomocí dovedností, které už mají.
* **Hladká integrace s vašimi investicemi do IT**
  
    Služba Data Lake Analytics může využívat vaše stávající investice do IT pro účely identity, správy, zabezpečení a datových skladů. To usnadňuje řízení dat a umožňuje snadné rozšíření stávajících datových aplikací. Služba Data Lake Analytics je integrovaná se službou Active Directory pro účely oprávnění a správy uživatelů a obsahuje integrované monitorování a auditování.
* **Cenová dostupnost a nákladová efektivita**
  
    Služba Data Lake Analytics je nákladově efektivní řešení pro spouštění úloh s velkým objemem dat. Platíte za jednotlivé úlohy, kdy se zpracovávají data. Není potřeba žádný hardware, licence ani smlouvy o podpoře pro konkrétní službu. Při spuštění a dokončení úlohy systém automaticky vertikálně navyšuje a snižuje kapacitu, což znamená, že nikdy platíte víc, než co potřebujete.
* **Možnost práce se všemi daty Azure**
  
    Služba Data Lake Analytics je optimalizovaná pro práci se službou Azure Data Lake – nabízí tak nejvyšší úroveň výkonu, propustnosti a paralelizace vašich úloh s velkými objemy dat.  Data Lake Analytics může spolupracovat také se službami Azure Blob Storage a Azure SQL Database.

## <a name="see-also"></a>Viz také
* Začínáme
  
  * [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
  * [Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-get-started-powershell.md)
  * [Začínáme s Azure Data Lake Analytics pomocí sady Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
  * [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
  * [Začínáme s jazykem U-SQL v Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
* Jazyk U-SQL a vývoj
  
  * [Použití funkcí okna U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-use-window-functions.md)
  * [Vývoj uživatelsky definovaných operátorů U-SQL pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
* Správa
  
  * [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md)
  * [Správa Azure Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-manage-use-powershell.md)
  * [Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
  * [Přístup k protokolům diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Ucelený kurz
  
  * [Použití interaktivních kurzů Azure Data Lake Analytics](data-lake-analytics-use-interactive-tutorials.md)
  * [Analýza webových protokolů pomocí služby Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* Dejte nám vědět, co si myslíte.
  
  <!-- Fixing broken links for Azure content migration from ACOM to DOCS. I can't find a suitable substitute for what appears to be a link that is no longer available. I am commenting out for now. The author can investigate in the future. Hyperlink text: Comment on our documentation backlog. Referenced file: data-lake-analytics-documentation-backlog.md -->
  * [Odeslat žádost o funkci](http://aka.ms/adlafeedback)
  * [Získejte pomoc ve fórech](http://aka.ms/adlaforums)


