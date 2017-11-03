---
title: "Ladění úloh U-SQL | Microsoft Docs"
description: "Zjistěte, jak k ladění selhání vrchol U-SQL pomocí sady Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Ladění uživatelem definované C# – kód pro selhání úloh U-SQL

U-SQL poskytuje model rozšiřitelnosti pomocí jazyka C#, takže můžete napsat kód přidat další funkce, jako jsou vlastní Extraktor nebo reduktorem. Další informace najdete v tématu [U-SQL programovatelnosti průvodce](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). V praxi může potřebovat žádný kód, ladění a systémy velkých objemů dat může poskytnout jenom omezené runtime ladění informace, jako jsou soubory protokolu.

Azure nástrojů Data Lake pro Visual Studio poskytuje funkci **se nezdařilo ladění vrchol**, která vám umožňuje klonovat neúspěšnou úlohu z cloudu do místního počítače pro ladění. Místní klonu zaznamená celý cloudové prostředí, včetně všech vstupních dat a uživatelského kódu.

Toto video ukazuje se nezdařilo vrchol ladění ve nástrojů Azure Data Lake pro Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Visual Studio vyžaduje následující dvě aktualizace, pokud již nejsou nainstalovány: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) a [Universal C Runtime pro systém Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-to-local-machine"></a>Vrchol stažení se nezdařilo místním počítači

Při otevření neúspěšnou úlohu v nástrojů Azure Data Lake pro Visual Studio zobrazí žlutý výstrahy pruh s podrobné chybové zprávy v kartě chyby.

1. Klikněte na tlačítko **Stáhnout** a stahovat požadované prostředky a vstupní datové proudy. Pokud stahování nedokončí, klikněte na tlačítko **opakujte**.

2. Klikněte na tlačítko **otevřete** po dokončení stahování pro generování prostředí místní ladění. Nová instance Visual Studio s ladění řešení je automaticky vytvořen a otevřít.

![Azure Data Lake Analytics U-SQL vrchol stažení sady visual studio pro ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Úlohy mohou zahrnovat soubory zdrojového kódu nebo registrovaný sestavení a tyto dva typy mají různé scénáře ladění.

- [Ladění úlohy se nezdařilo s kódem v pozadí](#debug-job-failed-with-code-behind)
- [Ladění neúspěšnou úlohu se sestavení](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Ladění úlohy se nezdařilo s kódem v pozadí

Pokud se nezdaří úlohy U-SQL a úlohy zahrnuje uživatelského kódu (obvykle s názvem `Script.usql.cs` v projektu U-SQL), že zdrojový kód je importovat do ladění řešení.  Odtud můžete použít sady Visual Studio ladicí nástroje (sledovat, proměnné atd.) k vyřešení tohoto problému.

> [!NOTE]
> Před ladění, nezapomeňte zkontrolovat **výjimky modulu CLR** v okně Nastavení výjimky (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL ladění sady visual studio nastavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Stiskněte klávesu **F5** spustit kód kódu. Bude fungovat, dokud nebude zastaven výjimkou.

2. Otevřete `ADLTool_Codebehind.usql.cs` souboru a nastavit zarážky, stiskněte **F5** k ladění kódu krok za krokem.

    ![Azure Data Lake Analytics U-SQL ladění výjimek](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Ladění úlohy se nezdařilo s sestavení

Pokud používáte registrované sestavení ve vašem skriptu U-SQL, systému nelze načíst zdrojový kód. Ručně přidejte v tomto případě soubory zdrojového kódu sestavení do řešení.

### <a name="configure-the-solution"></a>Konfigurace řešení

1. Klikněte pravým tlačítkem na **řešení 'VertexDebug' > Přidat > existující projekt...**  najít zdrojový kód sestavení a přidejte projekt k ladění řešení.

    ![Přidání projektu Azure Data Lake Analytics U-SQL ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Klikněte pravým tlačítkem na **LocalVertexHost > vlastnosti** v řešení a zkopírujte **pracovní adresář** cesta.

3. Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > vlastnosti**, vyberte **sestavení** kartě na levé straně a vložte zkopírovaný cestu jako **výstup > Výstupní cesta**.

    ![Azure Data Lake Analytics U-SQL ladění, nastavte cestu pdb](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Stiskněte klávesu **Ctrl + Alt + E**, zkontrolujte **výjimky modulu CLR** v okně Nastavení výjimky.

### <a name="start-debug"></a>Spuštění ladění

1. Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > sestavit** do výstupní soubory PDB do `LocalVertexHost` pracovní adresář.

2. Stiskněte klávesu **F5** a projekt se spustí, dokud nebude zastaven výjimkou. Může zobrazí následující zprávu upozornění, která můžete bezpečně ignorovat. Ho může trvat několik minut dostali na obrazovku pro ladění.

    ![Azure Data Lake Analytics U-SQL ladění sady visual studio upozornění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Otevřete vašeho zdrojového kódu a nastavit zarážky, stiskněte **F5** k ladění kódu krok za krokem.

Můžete také použít Visual Studio ladicí nástroje (sledovat, proměnné atd.) k vyřešení tohoto problému.

> [!NOTE]
> Znovu sestavte projekt sestavení zdrojového kódu pokaždé, když po úpravě kód pro vygenerování souborů aktualizované pdb.

Po ladění, po úspěšném dokončení projektu ve výstupním okně zobrazí následující zprávu:

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL ladění úspěšné](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>Odešlete znovu úlohu

Po dokončení ladění znovu odešlete informace o neúspěšné úloze.

1. Pro úlohy s kódem v pozadí řešení, zkopírujte kód C# do zdrojového souboru kódu na pozadí (obvykle `Script.usql.cs`).
2. Pro úlohy se sestaveními registraci sestavení aktualizované .dll do vaší databáze ADLA:
    1. Z Průzkumníka serveru nebo v Průzkumníku cloudu, rozbalte **ADLA účet > databáze** uzlu.
    2. Klikněte pravým tlačítkem na **sestavení** a registraci vaší nové sestavení .dll s databází ADLA: ![Azure Data Lake Analytics U-SQL ladění registrace sestavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Odešlete znovu úlohu.

## <a name="next-steps"></a>Další kroky

- [Průvodce programovatelnosti U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Vývoj U-SQL uživatelem definované operátory pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
