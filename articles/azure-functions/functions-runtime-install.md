---
title: Azure Functions instalace modulu Runtime | Microsoft Docs
description: Postup instalace modulu Runtime Azure Functions
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: b6078ba7b553773294ebbf11949f7d3b53f46f0c
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a>Nainstalujte Preview Runtime Azure Functions

Pokud chcete nainstalovat ve verzi preview Azure Functions Runtime, postupujte podle těchto kroků:

1. Zkontrolujte, že váš počítač úspěšně projde minimální požadavky.
1. Stažení [Azure Functions instalačního programu Preview Runtime](https://aka.ms/azafr).
1. Instalace modulu Runtime funkce Azure preview.
1. Dokončete konfiguraci Runtime funkce Azure Preview.

## <a name="prerequisites"></a>Požadavky

Než nainstalujete Azure Functions Runtime preview, musíte mít následující:

1. Počítač se systémem Microsoft Windows Server 2016 nebo Microsoft Windows 10 Creators Update (Professional nebo Enterprise Edition).
1. Instance SQL serveru spuštěna v rámci vaší sítě.  Minimální verze vyžaduje je SQL Server Express.

## <a name="install-the-azure-functions-runtime-preview"></a>Nainstalujte Preview Runtime Azure Functions

Instalační program Azure funkce Runtime Preview vás provede instalaci preview Azure Functions Runtime správy a rolí pracovního procesu.  Je možné nainstalovat roli správy a pracovního procesu na stejném počítači.  Jako přidáte další funkce, ale musíte nasadit více rolí pracovního procesu na další počítače. abyste mohli škálovat funkcí do více pracovníků.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Instalace správy a roli pracovního procesu na stejném počítači

1. Spusťte instalační program Preview Runtime Azure Functions.

    ![Instalační program Preview Runtime Azure Functions][1]

1. Klikněte na **Další**.
1. Jakmile budete mít přečtěte si podmínky **smlouvy EULA**, **zaškrtněte políčko** přijmout podmínky a klikněte na tlačítko **Další** posunut.
1. Nyní vyberte role, kterou chcete nainstalovat na tomto počítači **Role správy funkce** nebo **Role pracovního procesu funkce** a klikněte na tlačítko **Další**.

    ![Instalační program Preview Runtime Azure Functions - výběr Role][3]

    > [!NOTE]
    > Můžete nainstalovat **Role pracovního procesu funkce** na mnoho dalších počítačů. Uděláte to tak, postupujte podle těchto pokynů a vybrat pouze **Role pracovního procesu funkce** v instalačním programu.

1. Klikněte na tlačítko **Další** tak, aby měl **Průvodce instalací modulu Runtime Azure funkce** spustit proces instalace v počítači.
1. Po dokončení, spustí se Průvodce instalací **Azure Functions Runtime** nástroje Konfigurace.

    ![Dokončení instalační Preview Runtime Azure Functions][5]

    > [!NOTE]
    > Pokud instalujete na **Windows 10** a **kontejneru** funkce není povolená dříve, **instalace modulu Runtime funkce Azure** vás vyzve, abyste po restartování počítače pro dokončení instalace.

## <a name="configure-the-azure-functions-runtime"></a>Konfigurace modulu Runtime Azure Functions

K dokončení instalace Azure Functions Runtime je třeba provést konfiguraci.

1. **Azure Functions Runtime** konfigurační nástroj ukazuje, jaké role jsou nainstalovány v počítači.

    ![Nástroj konfigurace Preview Runtime Azure Functions][6]

1. Klikněte **databáze** kartě, zadejte podrobnosti připojení k vaší Instance systému SQL Server a klikněte na tlačítko **použít**.  To se vyžaduje, aby Runtime Azure Functions k vytvoření databáze pro podporu modulu Runtime.
    
    ![Konfigurace databáze Preview modulu Runtime Azure Functions][7]

1. Klikněte **pověření** kartě.  Na této obrazovce je nutné vytvořit dva nové přihlašovací údaje pro použití s sdílení souborů pro hostování všech Azure Functions.  Zadejte **uživatelské jméno** a **heslo** kombinací pro **vlastníka sdílené složky souboru** a **uživatele sdílené složky souboru**, pak klikněte na tlačítko **Použít**.

    ![Přihlašovací údaje Preview Runtime Azure Functions][8]

1. Klikněte **sdílené složky** kartě.  V této obrazovce je nutné zadat podrobnosti o **sdílené složky** umístění.  To je možné vytvořit za vás, nebo můžete použít existující sdílené složky a klikněte na tlačítko **použít**.  Pokud vyberete nové umístění sdílené složky, musíte zadat adresář pro použití Azure Functions Runtime.
    
    ![Azure Functions Runtime Preview sdílené složky][9]

1. Klikněte **IIS** kartě.  Tato karta zobrazuje podrobnosti o webů ve službě IIS, který nástroj Konfigurace Azure Functions Runtime vytvořit.  Klikněte na tlačítko **použít** k dokončení.

    ![Azure Functions Preview Runtime služby IIS][10]

1. Klikněte **služby** kartě.  Tato karta zobrazuje stav služeb v vaše konfigurace nástroje Azure Functions Runtime.  Pokud po počáteční konfiguraci **služba Aktivace hostitele funkce Azure** neběží klikněte na tlačítko **spustit službu**.

    ![Dokončení Configruation Preview Runtime Azure Functions][11]

1. Nakonec vyhledejte **modulu Runtime Azure Functions na portálu** jako `https://<machinename>/`.

    ![Modul Runtime Azure Functions na portálu Preview][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
