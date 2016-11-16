---
title: "Přehled Microsoft Azure Data Lake Analytics | Dokumentace Microsoftu"
description: "Data Lake Analytics je výpočetní služba Azure pro velké objemy dat, která umožňuje využívat data k podpoře podnikání díky informacím získaným z vašich dat v cloudu, bez ohledu na to, jak jsou tato data velká a kde se nacházejí. Data Lake Analytics toto umožňuje nejjednodušším, nejvíce škálovatelným a nejhospodárnějším možným způsobem. "
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
ms.date: 05/16/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a6d2d6365afaecab57a100867a72b9aa2def3745


---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Přehled Microsoft Azure Data Lake Analytics
## <a name="what-is-azure-data-lake-analytics"></a>Co je Azure Data Lake Analytics?
Azure Data Lake Analytics je nová služba, díky níž se dají snadno analyzovat velké objemy dat. Tato služba vám umožní soustředit se na psaní, spouštění a správu úloh, a nikoli na provoz distribuované infrastruktury. Namísto nasazení, konfigurace a ladění hardwaru můžete psát dotazy, transformovat data a extrahovat cenné informace. Analytická služba dokáže okamžitě zpracovávat úlohy libovolného rozsahu – stačí jednoduše nastavit, jaký výkon potřebujete. Za úlohu platíte jenom tehdy, když je spuštěná, což je nákladově efektivní. Analytická služba podporuje službu Azure Active Directory a umožňuje snadnou správu přístupu a rolí, které jsou integrovány s vaším místním systémem identit. Zahrnuje také jazyk U-SQL, který kombinuje výhody jazyka SQL a výrazovou sílu uživatelského kódu. Škálovatelný distribuovaný modul runtime jazyka U-SQL umožňuje efektivně analyzovat data v úložišti a na SQL Serverech v Azure, Azure SQL Database a Azure SQL Data Warehouse.

## <a name="key-capabilities"></a>Klíčové funkce
* **Dynamické škálování**
  
    Služba Data Lake Analytics je od samého počátku navržena pro cloudové škálování a výkon.  Dynamicky zřizuje prostředky a umožňuje provádět analýzy terabajtů nebo dokonce exabajtů dat. Po dokončení úlohy postupně automaticky ukončí prostředky, takže platíte jenom za využitý výpočetní výkon. Při zvětšování a zmenšování objemu uložených dat nebo množství využitých výpočtů nemusíte přepisovat kód. Díky tomu se můžete soustředit na obchodní logiku, a nikoli na způsob zpracování a ukládání rozsáhlých datových sad.
* **Rychlejší vývoj a inteligentnější ladění a optimalizace pomocí známých nástrojů**
  
    Služba Data Lake Analytics je těsně integrovaná se sadou Visual Studio, takže můžete spouštět, ladit a optimalizovat kód pomocí známých nástrojů. Vizualizace úloh U-SQL umožňuje sledovat běh kódu v reálných podmínkách a snadno identifikovat kritická místa výkonu a optimalizovat náklady.
* **Jazyk U-SQL: jednoduchý a známý, výkonný a rozšiřitelný**
  
    Data Lake Analytics obsahuje dotazovací jazyk U-SQL, který rozšiřuje dobře známý, jednoduchý a deklarativní charakter jazyka SQL o výrazovou sílu jazyka C#. Jazyk U-SQL je založený na stejném distribuovaném modulu runtime, který využívají systémy velkých objemů dat ve společnosti Microsoft. Miliony vývojářů SQL a .NET teď můžou zpracovávat a analyzovat všechna svoje data pomocí dovedností, které už mají.
* **Hladká integrace s vašimi investicemi do IT**
  
    Služba Data Lake Analytics může využívat vaše stávající investice do IT pro účely identity, správy, zabezpečení a datových skladů. To usnadňuje řízení dat a umožňuje snadné rozšíření stávajících datových aplikací. Služba Data Lake Analytics je integrovaná se službou Active Directory pro účely oprávnění a správy uživatelů a obsahuje integrované sledování a auditování.
* **Cenová dostupnost a nákladová efektivita**
  
    Služba Data Lake Analytics je nákladově efektivní řešení pro spouštění úloh s velkým objemem dat. Platíte za jednotlivé úlohy, kdy se zpracovávají data. Není potřeba žádný hardware, licence ani smlouvy o podpoře pro konkrétní službu. Při spuštění a dokončení úlohy systém automaticky vertikálně navyšuje a snižuje kapacitu, což znamená, že nikdy platíte víc, než co potřebujete.
* **Možnost práce se všemi daty Azure**
  
    Služba Data Lake Analytics může pracovat s řadou zdrojů dat Azure (úložiště objektů Azure Blob, databáze Azure SQL) a je speciálně optimalizovaná pro práci s Azure Data Lake Store – poskytuje nejvyšší úroveň výkonu, propustnosti a paralelizace pro úlohy s velkými objemy dat.

## <a name="see-also"></a>Viz také
* Začínáme
  
  * [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
  * [Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-get-started-powershell.md)
  * [Začínáme s Azure Data Lake Analytics pomocí sady Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
  * [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
  * [Začínáme s jazykem U-SQL v Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
* Jazyk U-SQL a vývoj
  
  * [Začínáme s jazykem U-SQL v Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
  * [Použití funkcí okna U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-use-window-functions.md)
  * [Vývoj uživatelem definovaných operátorů U-SQL pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
* Správa
  
  * [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md)
  * [Správa Azure Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-manage-use-powershell.md)
  * [Sledování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
  * [Přístup k protokolům diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Ucelený kurz
  
  * [Použití interaktivních kurzů Azure Data Lake Analytics](data-lake-analytics-use-interactive-tutorials.md)
  * [Analýza webových protokolů pomocí služby Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* Dejte nám vědět, co si myslíte.
  
  <!-- Fixing broken links for Azure content migration from ACOM to DOCS. I can't find a suitable substitute for what appears to be a link that is no longer available. I am commenting out for now. The author can investigate in the future. Hyperlink text: Comment on our documentation backlog. Referenced file: data-lake-analytics-documentation-backlog.md -->
  * [Odeslat žádost o funkci](http://aka.ms/adlafeedback)
  * [Získejte pomoc ve fórech](http://aka.ms/adlaforums)




<!--HONumber=Nov16_HO2-->


