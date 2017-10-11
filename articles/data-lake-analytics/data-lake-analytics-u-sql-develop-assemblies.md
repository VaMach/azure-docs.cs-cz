---
title: "Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics | Microsoft Docs"
description: "Další informace jak vyvíjet sestavení, které chcete použít a znovu použít v úloh Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics
Zjistěte, jak chcete-li kódu do sestavení, které chcete použít a znovu použít v úloh Data Lake Analytics. 

U-SQL umožňuje snadno přidat vlastní kód v jazyce .net, například C#, VB.Net nebo F #. Dokonce můžete nasadit vlastní modul runtime pro podporu dalších jazyků.

Nejjednodušší způsob, jak použít vlastní kód je použití nástroje Data Lake pro Visual Studio kódu možnosti. Další informace najdete v tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Existuje několik nevýhody použití kódu:

- Zdrojový kód získá nahrát pro každý skript odeslání.
- kódu nemohou být sdíleny s ostatními úlohami.

Chcete-li vyřešit tyto nevýhody, můžete zapnout kódu do sestavení a registraci sestavení do katalogu Data Lake Analytics.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012 s nainstalovaným Visual C++
* Microsoft Azure SDK pro .NET verze 2.5 nebo vyšší.  Nainstalujte ji pomocí instalačního programu webové platformy nebo instalační program Visual Studio
* Účet Data Lake Analytics.  Viz [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).
* Projděte [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kurzu.
* Připojte k Azure.
* Nahrát zdrojová data, najdete v části [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>Vývoj sestavení pro U-SQL

**K vytvoření a odeslání úlohy U-SQL**

1. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.
2. Rozbalte položku **nainstalovaná**, **šablony**, **Azure Data Lake**, **U-SQL(ADLA)**, vyberte **knihovny tříd (pro aplikace U-SQL)** šablony a pak klikněte na tlačítko **OK**.
3. Zápis kódu v Class1.cs.  Zde je ukázka kódu.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Klikněte na tlačítko **sestavení** nabídce a pak klikněte na tlačítko **sestavit řešení** vytvořit knihovnu dll.

## <a name="register-assemblies"></a>Registrace sestavení

V tématu [použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-the-assemblies"></a>Použití sestavení

V tématu [použití nástroje Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Viz také
* [Začínáme s Data Lake Analytics pomocí prostředí PowerShell](data-lake-analytics-get-started-powershell.md)
* [Začínáme s Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
* [Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
* [Použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md)
* [Použití nástrojů Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)