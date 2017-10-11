---
title: "Nástrojů Azure Data Lake: U-SQL místní spuštění a místní ladění s Visual Studio Code | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio Code místní spuštění a místní ladění."
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, odešlete do cestu k úložišti"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 367e4ba792f83d6ee246208306e4c09b69cb49ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a>U-SQL místní spuštění a místní ladění s kódem jazyka Visual Studio

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že máte následující požadavky na místě před zahájením těchto postupů:
- Nástroj Azure Data Lake pro Visual Studio Code. Pokyny najdete v tématu [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- C# pro Visual Studio Code (Pokud chcete provést místní ladicího U-SQL).

   ![Instalace jazyka C# v nástrojů Data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > Místní spuštění U-SQL a ladění funkcí aktuálně podporují pouze uživatelé s Windows. 


## <a name="set-up-the-u-sql-local-run-environment"></a>Nastavení prostředí pro místní spuštění U-SQL

1. Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: stažení závislostí LocalRun** stáhnout balíčky.  

   ![Stáhnout balíčky ADL LocalRun závislostí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Vyhledejte závislosti balíčků z cesty uvedené v **výstup** podokně a pak nainstalujte BuildTools a Win10SDK 10240. Tady je příklad cesty:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Vyhledejte závislosti balíčků](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   a. Chcete-li nainstalovat BuildTools, postupujte podle pokynů průvodce.   

  ![Nainstalujte BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   b. Chcete-li nainstalovat Win10SDK 10240, postupujte podle pokynů průvodce.  

  ![Nainstalujte Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Nastavte proměnnou prostředí. Nastavte **SCOPE_CPP_SDK** proměnnou prostředí:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Restartujte operačního systému a ujistěte se, že nastavení proměnné prostředí se projeví.  

   ![Zkontrolujte, jestli že je nainstalované SCOPE_CPP_SDK proměnné prostředí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Spusťte službu místní spuštění a odeslání úlohy U-SQL pro místní účet 
Pro uživatele poprvé, budete vyzváni ke stažení ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.
1. Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: spustit místní spustit službu**.
2. Vyberte **přijmout** poprvé přijmout licenční podmínky pro Software společnosti Microsoft. 

   ![Přijmout licenční podmínky pro Software společnosti Microsoft](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Otevře se konzola cmd. Pro uživatele, musíte zadat **3**a potom vyhledejte cestu místní složky pro vaše data vstup a výstup. Pro další možnosti můžete použít výchozí hodnoty. 

   ![Nástroje data Lake pro Visual Studio Code místní spuštění cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Vyberte Ctrl + Shift + P otevřít palety příkazu, zadejte **ADL: odeslat úlohu**a potom vyberte **místní** se odeslat úlohu místní účet.

   ![Vyberte místní nástrojů data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Po odeslání úlohy můžete zobrazit podrobnosti o odeslání. K zobrazení podrobností vyberte odesílání **jobUrl** v **výstup** okno. Můžete také zobrazit stav úlohy odeslání z konzoly cmd. Zadejte **7** v konzole cmd, pokud chcete zjistit podrobnosti úlohy.

   ![Nástroje data Lake pro Visual Studio Code místní spuštění výstupu](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![nástroje Data Lake pro Visual Studio Code místní spuštění cmd stav](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Spustit místní ladění pro projekt U-SQL  
Pro uživatele poprvé, budete vyzváni ke stažení ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.
  
1. Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: spustit místní spustit službu**. Otevře se konzola cmd. Ujistěte se, že **DataRoot** nastavena.
3. Nastavte zarážky v vaší C# kódu.
4. Zpět v editoru skriptu vyberte Ctrl + Shift + P pro otevření konzoly příkazu a potom zadejte **místní ladění** na místní ladění službu spustit.

![Nástroje data Lake pro Visual Studio Code výsledek místní ladění](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Další kroky
- Pomocí nástrojů Azure Data Lake pro Visual Studio Code, najdete v části [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Získávání Začínáme informací o Data Lake Analytics, najdete v části [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).
