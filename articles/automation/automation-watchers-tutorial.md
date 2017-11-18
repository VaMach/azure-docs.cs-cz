---
title: "Vytvoření úlohy sledovacího procesu v účtu Azure Automation | Microsoft Docs"
description: "Postup vytvoření úlohy sledovacího procesu v Azure Automation. účet, který chcete sledovat pro nové soubory vytvořené ve složce."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/15/2017
ms.author: eamono
ms.openlocfilehash: 7cd6bebcaa1ed263b9854f7307cf22fba006748e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="azure-automation-watcher-tasks-enable-you-to-respond-to-events-happening-in-your-local-datacenter"></a>Úlohy sledovacího procesu služby Azure Automation umožňují reakce na události děje ve vašem místním datovém centru

V tomto kurzu zjistěte, jak vytvořit novou úlohu sledovacích procesů na:

> [!div class="checklist"]
> * Vytvoření sady runbook sledovacích procesů, které hledá nové soubory v adresáři.
> * Vytvořte proměnnou automatizace zachovat čas posledního zpracování souboru sledovacích procesů.
> * Vytvoří akci sady runbook, která je volána, když sada runbook sledovacích procesů vyhledá nový soubor.
> * Vytvoření úlohy sledovacího procesu, která vybere sadu runbook sledovacích procesů a runbook akce.
> * Aktivovat sledovacích procesů tak, že přidáte nový soubor do adresáře.
> * Zkontrolujte výstup z runbooku akce, které se zobrazují informace o nový soubor.  

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je potřeba splnit následující nástroje.
+ Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Účet Automation](automation-offering-get-started.md) pro uložení sledovacích procesů a akce sad runbook a úlohy sledovacího procesu.
+ A [hybridní pracovní proces runbooku](automation-hybrid-runbook-worker.md) kde spuštěna úloha sledovacích procesů.

## <a name="create-a-watcher-runbook-that-looks-for-new-files"></a>Vytvoření sady runbook sledovacích procesů, které hledá nové soubory
1.  Otevřete účet Automation a klikněte na stránce sady Runbook.
2.  Klikněte na tlačítko "Procházet galerii".
![Seznam Runbook z uživatelského rozhraní](media/automation-watchers-tutorial/WatcherTasksRunbookList.png)
3.  Vyhledejte "Sledovat-NewFile" a importovat sady runbook do účtu Automation.
![Publikovat runbook z uživatelského rozhraní](media/automation-watchers-tutorial/Watch-NewFileRunbook.png)
4.  Klikněte na "Upravit" zobrazení zdroje sady Runbook a klikněte na tlačítko "Publikovat".

## <a name="create-an-automation-variable-to-keep-the-last-time-a-file-was-processed-by-the-watcher"></a>Vytvořte proměnnou automatizace zachovat čas posledního zpracování souboru sledovacích procesů
1.  Otevřete stránku pro proměnné v rámci SDÍLENÉ prostředky a klikněte na "Přidat proměnnou" ![seznamu proměnných z uživatelského rozhraní](media/automation-watchers-tutorial/WatcherVariableList.png)
2.  Jako název zadejte "Sledovat-NewFileTimestamp"
3.  Vyberte typ jako datum a čas a potom klikněte na tlačítko "Vytvořit".
![Vytvořte proměnnou vodoznak z uživatelského rozhraní](media/automation-watchers-tutorial/WatcherWatermarkVariable.png)

## <a name="create-an-action-runbook-that-is-called-when-the-watcher-runbook-finds-a-new-file"></a>Vytvořit akci sady runbook, která je volána, když sada runbook sledovacích procesů vyhledá nový soubor
1.  Klikněte na stránce sady Runbook v kategorii "AUTOMATIZACE PROCESŮ".
2.  Klikněte na tlačítko "Procházet galerii".
3.  Vyhledejte "Proces-NewFile" a importovat sady runbook do účtu Automation.
4.  Klikněte na "Upravit" zobrazení zdroje sady Runbook a klikněte na tlačítko "Publikovat".
![Proces sledovacích procesů z uživatelského rozhraní](media/automation-watchers-tutorial/Watch-ProcessNewFile.png)


## <a name="create-a-watcher-task-that-selects-the-watcher-runbook-and-action-runbook"></a>Vytvoření úlohy sledovacího procesu, která vybere sadu runbook sledovacích procesů a runbook akce
1.  Otevřete stránku úlohy sledovacího procesu a klikněte na tlačítko "Přidat úloha sledovacích procesů".
![Seznam sledovacích procesů z uživatelského rozhraní](media/automation-watchers-tutorial/WatchersList.png)
2.  Zadejte "Sledovat nové soubory" jako název.
3.  Vyberte "Konfigurace sledovacích procesů" a vyberte sadu runbook "Sledovat-NewFile".
![Konfigurace sledovacích procesů z uživatelského rozhraní](media/automation-watchers-tutorial/ConfigureWatcher.png)
4.  Zadejte následující hodnoty pro parametry:
    *   FOLDERPATH. Do složky na hybridní pracovní proces, kde získat vytváří nové soubory
    *   ROZŠÍŘENÍ. Ponechte prázdné zpracovat všechny přípony souborů.
    *   RECURSE. Ponechte výchozí nastavení.
    *   PARAMETRY SPUŠTĚNÍ. Vyberte hybridní pracovní proces.
5.  Klikněte na tlačítko OK a pak vyberte návrat na stránku sledovacích procesů.
6.  Vyberte "Konfigurace action" a vyberte sadu runbook "Proces-NewFile".
![Konfigurace akce sledovacích procesů z uživatelského rozhraní](media/automation-watchers-tutorial/ConfigureAction.png)
7.  Zadejte následující hodnoty pro parametry:
    *   EVENTDATA. Ponechte prázdné. Data, je předaná ze sady runbook sledovacích procesů.
    *   Parametry spuštění. Nechte Azure Tato sada runbook běží ve službě Automation.
8.  Klikněte na tlačítko OK a pak vyberte návrat na stránku sledovacích procesů.
9.  Klikněte na tlačítko OK, abyste vytvořili úlohu sledovacích procesů.

## <a name="trigger-a-watcher-by-adding-a-new-file-to-a-directory"></a>Aktivační událost sledovacích procesů přidáním nového souboru do jiného adresáře
1.  Vzdálené do hybridní pracovní proces
2.  Přidáte nový textový soubor do složky, která je monitorována úlohou sledovacích procesů.

## <a name="inspect-the-output-from-the-action-runbook-that-shows-information-on-the-new-file"></a>Zkontrolujte výstup z runbooku akce, které se zobrazují informace o nový soubor
1.  Klikněte na úlohu sledovacích procesů "Sledovat nové soubory"
2.  Klikněte na "zobrazení sledovacího procesu datové proudy" zda sledovacích procesů nalezen nový soubor a spuštění runbooku akce.
3.  Klikněte na "Zobrazit sledovacích procesů akce úlohy" zobrazíte akce úlohy runbooku.
![Akce úlohy sledovacího procesu z uživatelského rozhraní](media/automation-watchers-tutorial/WatcherActionJobs.png)


## <a name="next-steps"></a>Další kroky:

Další informace najdete v tématu [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).








