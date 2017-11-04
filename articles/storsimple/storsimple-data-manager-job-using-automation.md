---
title: "Použít k aktivaci úlohy Azure Automation | Microsoft Docs"
description: "Naučte se používat Azure Automation pro spuštění úlohy StorSimple Manager dat (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a>Použití Azure Automation k aktivaci úlohy (soukromém náhledu).

Tento článek popisuje, jak používat Azure Automation k aktivaci úlohy StorSimple Data Manager.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, zda máte:

*   Nainstalovat Azure Powershell. [Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Nastavení konfigurace k chybě při inicializaci úlohu transformace dat (pokyny k získání těchto nastavení jsou zde uvedena).
*   Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.
*   Stáhněte si `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) soubor z úložiště githubu.
*   Stáhněte si `Get-ConfigurationParams.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) z úložiště githubu.
*   Stáhněte si `Trigger-DataTransformation-Job.ps1` [skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) z úložiště githubu.

## <a name="step-by-step"></a>Podrobný postup

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a>Získat oprávnění služby Azure Active Directory pro danou definici úlohy úlohu automatizace

1. Chcete-li načíst parametry konfigurace pro službu Active Directory, proveďte následující kroky:

    1. Otevřete prostředí Windows PowerShell v místním počítači. Ujistěte se, že [prostředí Azure PowerShell](https://azure.microsoft.com/downloads/) je nainstalovaná.
    1. Spustit `Get-ConfigurationParams.ps1` skriptu (ve složce stažené výše). V okně prostředí PowerShell zadejte následující příkaz:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        ActiveDirectoryKey je heslo, které použijete později. Zadejte heslo podle svého výběru. AppName může být libovolný řetězec.

2. Skript vypíše následující hodnoty, které se mají použít při spuštění sady automation runbook. Tyto hodnoty si poznamenejte.

    - ID klienta
    - ID tenanta
    - Klíčů služby Active Directory (stejný jako ten, který je zadaný výše)
    - ID předplatného

### <a name="set-up-the-automation-account"></a>Nastavit účet Automation

1. Přihlaste se k Azure a otevřete svůj účet Automation.
2. Klikněte na tlačítko **prostředky** dlaždici otevřete seznam prostředků.
3. Klikněte na tlačítko **moduly** dlaždici otevřete seznamu modulů.
4. Klikněte na tlačítko **+ přidat modul** spuštění tlačítko a okna Přidat modul.

    ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Po výběru `DataTransformationApp.zip` souboru z místního počítače, klikněte na tlačítko **OK** Import modulu.

   Jakmile Azure Automation importuje modul ke svému účtu, extrahuje metadata o modulu. Tato operace může trvat několik minut.

   ![Nastavení účtu Automation.](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Po dokončení procesu obdržíte oznámení, který je nasazován modul a jiné oznámení.  Stav můžete zkontrolovat **moduly** dlaždici.

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a>Chcete-li importovat sady runbook, která aktivuje definici úlohy

1. Na webu Azure Portal otevřete účet Automation.
2. Klikněte na tlačítko **Runbooky** dlaždici otevřete seznam runbooků.
3. Klikněte na tlačítko **+ přidat runbook** a potom **importovat stávající runbook**.

   ![Importovat stávající runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Klikněte na tlačítko **soubor sady Runbook** a vyberte soubor k importu `Trigger-DataTransformation-Job.ps1`.
5. Klikněte na tlačítko **vytvořit** k importu sady runbook. Nový runbook se zobrazí v seznamu sad runbook pro účet služby Automation.
7. Klikněte na tlačítko **aktivační událost-DataTransformation-Job** runbook a potom klikněte na **upravit**.
8. Klikněte na tlačítko **publikovat** a potom **Ano** po zobrazení výzvy k potvrzení.


### <a name="to-run-the-runbook"></a>Spuštění sady runbook:
1. Na webu Azure Portal otevřete účet Automation.
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Klikněte na tlačítko **aktivační událost DataTransformation-Job**.
4. Kliknutím na **Spustit** spustíte runbook.

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. V **spuštění sady runbook** okno, zadejte všechny parametry. Klikněte na tlačítko **OK** se odeslat úlohu transformace Data.

   ![Spuštění runbooku](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Další kroky

[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).