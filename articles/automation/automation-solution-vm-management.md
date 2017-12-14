---
title: "Spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení | Microsoft Docs"
description: "Řešení pro správu virtuálních počítačů spouštějí a zastavují vaše virtuální počítače s Azure Resource Managerem podle časového plánu a proaktivně provádějí monitorování ze služby Log Analytics."
services: automation
documentationCenter: 
authors: georgewallace
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: magoedte
ms.openlocfilehash: e6f1189b9729c57718a5cd6d6f6a583b94f6f142
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení v Azure Automation.

Spuštění a zastavení virtuálních počítačů špičku řešení spustí a zastaví virtuální počítače Azure na uživatelem definované plány, poskytuje přehledy prostřednictvím analýzy protokolů a odešle volitelné e-mailů s využitím [sendgrid vám umožňuje](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Podporuje Azure Resource Manager i klasické virtuální počítače pro většinu scénářů. 

Toto řešení poskytuje funkci decentralizované automatizace pro zákazníky, kteří chtějí snížit náklady na jejich využití prostředků nízké náklady, bez serveru. K funkcím patří:

* Virtuální počítače plán spuštění a zastavení
* Plánování virtuální počítače na spuštění a zastavení ve vzestupném pořadí pomocí značek Azure (není podporováno pro klasické virtuální počítače)
* Automatické ukončení, virtuální počítače založené na nízké využití procesoru

## <a name="prerequisites"></a>Požadavky

- Runbooky pracují s [účtem Spustit jako Azure](automation-offering-get-started.md#authentication-methods).  Účet Spustit jako je upřednostňovanou metodou ověřování, protože namísto hesla, jehož platnost může vypršet nebo které se může často měnit, používá ověřování certifikátu.  

- Toto řešení můžete spravovat pouze virtuální počítače, které jsou ve stejném předplatném jako kde je umístěn účet Automation.  

- Toto řešení nasadí jenom pro následující oblasti - Austrálie – jihovýchod, Střední Kanada, střed, východní USA, Japonsko – východ, jihovýchodní Asie, Spojené království – jih a západní Evropa.  
    
    > [!NOTE]
    > Sady runbook Správa virtuálních počítačů plánu, můžete vybrat virtuální počítače v libovolné oblasti.  

- Chcete-li odeslat e-mailová oznámení po dokončení spouštění a zastavování sad runbook virtuálních počítačů při připojování z Azure Marketplace, je nutné vybrat vyberte **Ano** sendgrid vám umožňuje nasadit. 

    > [!IMPORTANT]
    > Sendgrid vám umožňuje služby třetích stran, pro podporu služby sendgrid požádejte [sendgrid vám umožňuje](https://sendgrid.com/contact/).  
    >
   
    Omezení sendgridu jsou následující:

    * Maximálně jeden účet sendgrid vám umožňuje na uživatele na předplatné
    * Maximálně dva účty sendgrid vám umožňuje na předplatné

Pokud jste nasadili předchozí verze tohoto řešení, musíte nejprve odstranit z vašeho účtu před nasazením této verze.  

## <a name="solution-components"></a>Součásti řešení

Toto řešení zahrnuje předkonfigurovaná sady runbook, plány a integrace s analýzy protokolů, která umožňuje přizpůsobit spuštění a vypnutí virtuálních počítačů k suite, pomocí vaší firmě potřebuje. 

### <a name="runbooks"></a>Runbooky

V následující tabulce je seznam sady runbook nasazené do vašeho účtu Automation.  Není vhodné měnit kód sady runbook, ale místo zápisu vlastní sadu runbook pro nové funkce.

> [!NOTE]
> Nespouštět přímo všechny sady runbook s názvem "Podřízený" připojen na konci jeho název.
>

Zahrnuje všechny nadřazené sady runbook *WhatIf* parametr, který v případě nastavena na **True**, podporuje s podrobnostmi o přesné chování sady runbook provede, když spustit, aniž by *WhatIf* parametr a ověří správnou virtuální počítače jsou cílové.  Sady Runbook pouze provede jeho definované akce při *WhatIf* parametr je nastaven na **False**. 

|**Sady Runbook** | **Parametry** | **Popis**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Volá se nezávisle na nadřízeném runbooku. Vytvoří výstrahy na základě prostředků za AutoStop scénáři.| 
|AutoStop_CreateAlert_Parent | WhatIf: True nebo False <br> VMList | Vytvoří nebo aktualizuje Azure pravidla výstrah na virtuálních počítačích v cílových skupinách, předplatné nebo prostředek. <br> VMList: Čárkami oddělený seznam virtuálních počítačů.  Například *vm1, virtuálního počítače 2, vm3*| 
|AutoStop_Disable | None | Zakažte AutoStop výstrahy a výchozí plán.| 
|AutoStop_StopVM_Child | WebHookData | Volat z jenom nadřízený runbook. Pravidla výstrah volání této sady runbook a dělá práci při zastavení virtuálního počítače.|  
|Bootstrap_Main | None | Použít jednou k zavedení konfigurace nastavení, třeba webhookURI, která nejsou obvykle přístupné ze Správce prostředků Azure. Tato sada runbook bude automaticky odebrána, pokud nasazení úspěšně přešel.|  
|ScheduledStartStop_Child | VMName <br> Akce: Zastavení nebo spuštění <br> Název skupiny prostředků | Volat z jenom nadřízený runbook. Neodpovídá skutečné provádění zastavení nebo spuštění pro naplánované zastavit.|  
|ScheduledStartStop_Parent | Akce: Zastavení nebo spuštění <br> WhatIf: True nebo False | To se projeví na všech virtuálních počítačích v rámci předplatného Pokud neupravíte **External_Start_ResourceGroupNames** a **External_Stop_ResourceGroupNames** kterého bude ji provést pouze na tyto omezit cílové skupiny prostředků. Můžete také vyloučit konkrétní virtuálních počítačů tím, že aktualizuje **External_ExcludeVMNames** proměnné. WhatIf se chová stejně jako jiné runbooky.|  
|SequencedStartStop_Parent | Akce: Zastavení nebo spuštění <br> WhatIf: True nebo False | Vytvoření značky názvem **SequenceStart** a jiné značky názvem **SequenceStop** na každý virtuální počítač, který chcete spustit pořadí\\zastavení aktivity pro. Hodnota značky musí být celé kladné číslo (1,2,3), který odpovídá pořadí, které chcete spustit\\zastavení ve vzestupném pořadí. WhatIf se chová stejně jako jiné runbooky. <br> **Poznámka: Virtuální počítače musí být v rámci skupiny prostředků určené External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames a External_ExcludeVMNames v Azure Automation. proměnné a mít odpovídající značky pro akce se projeví.**|

### <a name="variables"></a>Proměnné

V následující tabulce je seznam proměnných vytvořené v účtu Automation.  Je doporučeno upravit pouze proměnné předponu **externí**, úprava proměnné předponu **interní** může způsobit nežádoucí účinky.  

|**Proměnná** | **Popis**|
---------|------------|
|External_AutoStop_Condition | Toto je podmíněný operátor vyžaduje pro konfiguraci stavu před spuštěním výstrahu. Přípustné hodnoty jsou **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, **LessThanOrEqual**.|  
|External_AutoStop_Description | Výstrahy zastavte virtuální počítač, pokud % využití procesoru překročí prahovou hodnotu.|  
|External_AutoStop_MetricName | Název metriky výkonu pravidlo výstrahy Azure má být nakonfigurován pro.| 
|External_AutoStop_Threshold | Prahová hodnota pro pravidlo výstrahy Azure určeném v proměnné *External_AutoStop_MetricName*. Procentuální hodnoty musí být v rozsahu 1 až 100.|  
|External_AutoStop_TimeAggregationOperator | Operátor agregace času, které se použijí na vybrané okno pro velikost vyhodnotit podmínku. Přípustné hodnoty jsou **průměrná**, **minimální**, **maximální**, **celkový**, **poslední**.|  
|External_AutoStop_TimeWindow | Velikost okna, za které bude analyzovat Azure vybrané metriky pro aktivaci výstrahy. Tento parametr přijme vstup ve formátu timespan. Možné hodnoty jsou pět minut až šest hodin.|  
|External_EmailFromAddress | Určuje odesílatele e-mailu.|  
|External_EmailSubject | Určuje text pro řádek předmětu e-mailu.|  
|External_EmailToAddress | Určuje příjemce e-mailu. Jednotlivé názvy čárkou.|  
|External_ExcludeVMNames | Zadejte názvy virtuálních počítačů, které se mají vyloučit, oddělené čárkou bez mezer.|  
|External_IsSendEmail | Určuje možnost odeslat e-mailové oznámení po dokončení.  Zadejte **Ano** nebo **ne** není odeslat e-mailu.  Tato možnost by měla být **ne** Pokud jste nevytvořili sendgrid vám umožňuje během počátečního nasazení.|  
|External_Start_ResourceGroupNames | Určuje jeden nebo více skupin prostředků oddělení hodnoty pomocí čárky, určený pro spuštění akce.|  
|External_Stop_ResourceGroupNames | Určuje jednu nebo více skupin prostředků, oddělte hodnoty čárkami, určený pro zastavení akce.|  
|Internal_AutomationAccountName | Určuje název účtu služby Automation.|  
|Internal_AutoSnooze_WebhookUri | Určuje že identifikátor URI Webhooku pro scénář AutoStop volat.|  
|Internal_AzureSubscriptionId | Určuje ID předplatného Azure.|  
|Internal_ResourceGroupName | Určuje název skupiny prostředků účtu Azure Automation.|  
|Internal_SendGridAccountName | Určuje název účtu sendgrid vám umožňuje.|  
|Internal_SendGridPassword | Určuje heslo účtu sendgrid vám umožňuje.|  

<br>

Ve všech scénářích **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, a **External_ExcludeVMNames** nutných proměnné pro cílení na virtuální počítače s výjimkou poskytování čárkami oddělený seznam virtuálních počítačů pro **AutoStop_CreateAlert_Parent** sady runbook. To znamená virtuální počítače se musí nacházet v cílové skupiny prostředků pro spuštění a zastavení akce proběhnout. Logiku funguje trochu jako Azure zásad v, že se cílí na skupiny předplatné nebo prostředek a máte akce zdědí nově vytvořený virtuální počítače. Tento přístup zabraňuje museli udržovat samostatné plán pro každý virtuální počítač a spravovat spuštění a zastavení rozsahu.

### <a name="schedules"></a>Plány

Následující tabulka uvádí každý z výchozích plánů vytvoří ve vašem účtu Automation.  Můžete je upravit nebo vytvořit vlastní vlastní plány.  Ve výchozím nastavení všechny tyto plány jsou zakázány s výjimkou **Scheduled_StartVM** a **naplánovaná StopVM**.

Povolit všechny plány ho se nedoporučuje, protože to může vytvořit překrývají jako na plán, který provádí akce.  Místo toho by bylo vhodné určit optimalizace, které chcete provádět a odpovídajícím způsobem upravit.  Najdete ukázkové scénáře v oddílu přehled další vysvětlení.   

|**ScheduleName** | **Frekvence** | **Popis**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Spouští každých 8 hodin | Ke spuštění sady runbook AutoStop_CreateAlert_Parent každých 8 hodin, které pak bude zastavení Virtuálního počítače na základě hodnot v "External_Start_ResourceGroupNames", "External_Stop_ResourceGroupNames" a "External_ExcludeVMNames" v proměnné automatizace Azure.  Alternativně můžete čárkami oddělený seznam virtuálních počítačů pomocí parametru "VMList".|  
|Scheduled_StopVM | Uživatelem definované, každý den | Spuštění sady runbook Scheduled_Parent s parametrem "Stop" každý den v daném čase.  Automaticky zastaví všechny virtuální počítače splňující z pravidel definovaných prostřednictvím proměnné Asset.  Doporučujeme povolení sesterské naplánovat, naplánovaná-StartVM.|  
|Scheduled_StartVM | Uživatelem definované, každý den | Ke spuštění sady runbook Scheduled_Parent s parametrem *spustit* každý den v daném čase.  Program automaticky spustí všechny virtuální počítače, které splňují z pravidel definovaných a součástí příslušné proměnné.  Doporučujeme povolit sesterské plánu, **naplánovaná StopVM**.|
|Sekvencování StopVM | 1:00 AM (UTC), každý pátek | Ke spuštění sady runbook Sequenced_Parent s parametrem *Zastavit* každý pátek v daném čase.  Budou postupně (vzestupným) zastavte všechny virtuální počítače s značkou z **SequenceStop** určené a součástí příslušné proměnné.  Informace naleznete v sekci sady Runbook další podrobnosti o značky hodnoty a proměnné asset.  Je vhodné povolit sesterské plánu, **Sequenced-StartVM**.|
|Sekvencování StartVM | 1:00 PM (UTC), každé pondělí | Ke spuštění sady runbook Sequenced_Parent s parametrem *spustit* každé pondělí v určeném čase.  Budou postupně (sestupně) spustit všechny virtuální počítače s značkou z **SequenceStart** určené a součástí příslušné proměnné.  Informace naleznete v sekci sady Runbook další podrobnosti o značky hodnoty a proměnné asset.  Doporučujeme povolit sesterské plánu, **Sequenced StopVM**.|

<br>

## <a name="configuration"></a>Konfigurace

Provedením následujících kroků přidejte řešení pro spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview] do svého účtu služby Automation a poté řešení upravte prostřednictvím konfigurace proměnných.

1. Na webu Azure Portal klikněte na **Nový**.<br> ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. V podokně Marketplace zadejte **spustit virtuální počítač**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **spuštění a zastavení virtuálních počítačů, která [Preview]** ve výsledcích hledání.  
3. V **spuštění a zastavení virtuálních počítačů, která [Preview]** vybraného řešení. v podokně zkontrolujte souhrnné informace a potom klikněte na **vytvořit**.  
4. **Přidat řešení** podokně se zobrazí, kde jste vyzváni ke konfiguraci řešení, než můžete ho importovat do vašeho předplatného automatizace.<br><br> ![Okno Přidat řešení správy virtuálních počítačů](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Na **přidat řešení** vyberte **prostoru** a zde vyberte pracovní prostor OMS, který je propojený s stejné předplatné Azure, účet Automation je v nebo vytvořit nový pracovní prostor.  Pokud nemáte pracovní prostor, můžete si vybrat **vytvořit nový pracovní prostor** a na **pracovním prostorem OMS** podokně, proveďte následující kroky: 
   - Zadejte název pro nový **pracovní prostor OMS**.
   - Vyberte **předplatné**, které má být cílem propojení, výběrem z rozevíracího seznamu, pokud výchozí vybrané předplatné není vhodné.
   - Pro možnost **Skupina prostředků** můžete vytvořit novou skupinu prostředků nebo vybrat existující skupinu prostředků.  
   - Vyberte **Umístění**.  Aktuálně jsou pouze umístění zadané pro výběr **Austrálie – jihovýchod**, **Kanada centrální**, **střed**, **východní USA**, **Japonsko – východ**, **jihovýchodní Asie**, **Spojené království – jih**, a **západní Evropa**.
   - Vyberte možnost u položky **Cenová úroveň**.  Řešení je k dispozici ve dvou úrovních: úroveň Free a placená úroveň OMS.  V úrovni Free je omezen objem dat shromážděných za den, doba uchovávání a počet minut běhu úloh runbooku.  V placené úrovni OMS není objem dat shromážděných za den omezen.  

        > [!NOTE]
        > Když jako možnost zobrazí za GB (samostatně) placené vrstvy, není použitelný.  Pokud ji vyberete a budete pokračovat ve vytváření tohoto řešení v rámci předplatného, dojde k selhání.  Problém bude odstraněn po oficiálním vydání tohoto řešení.<br>Pokud použijete toto řešení, bude využívat pouze příjem protokolů a minuty úloh služby Automation.  Řešení nepřidá do vašeho prostředí další uzly OMS.  

6. Po zadání požadovaných informací v okně **Pracovní prostor OMS** klikněte na **Vytvořit**.  Během ověřování informací a vytváření pracovního prostoru můžete průběh zpracování sledovat prostřednictvím položky nabídky **Oznámení**.  Budete přesměrováni zpět do okna **Přidat řešení**.  
7. V okně **Přidat řešení** vyberte možnost **Účet služby Automation**.  Pokud vytváříte nový pracovní prostor OMS, bude třeba, abyste vytvořili i nový účet služby Automation, který bude přidružen k novému pracovnímu prostoru OMS určenému dříve, a to včetně vašeho předplatného Azure, skupiny prostředků a oblasti.  Můžete vybrat možnost **Vytvořit účet Automation** a v okně **Přidat účet Automation** zadat následující údaje: 
  - Do pole **Název** zadejte název účtu služby Automation.

    Všechny ostatní možnosti se vyplní automaticky na základě vybraného pracovního prostoru OMS. Tyto možnosti nelze upravovat.  Účet Spustit v Azure jako představuje výchozí metodu ověřování pro runbooky obsažené v tomto řešení.  Po kliknutí na **OK** se ověří možnosti konfigurace a vytvoří se účet služby Automation.  Průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

    Můžete také vybrat existující účet služby Automation Spustit jako.  Všimněte si, že účet, který vyberete, již nelze propojit s jiným pracovním prostorem OMS. V opačném případě se v okně zobrazí zpráva s informací.  Pokud je propojení již vytvořeno, je třeba vybrat jiný účet služby Automation Spustit jako nebo vytvořit nový.<br><br> ![Účet služby Automation, který je již propojen s pracovním prostorem OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Nakonec v okně **Přidat řešení** vyberte možnost **Konfigurace**. Zobrazí se okno **Parametry**.<br><br> ![Podokno parametrů pro řešení](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  V okně **Parametry** se zobrazí výzva k provedení následujících akcí:  
   - Zadejte hodnotu **Názvy cílové skupiny prostředků**. Jedná se o název skupiny prostředků obsahující virtuální počítače, které mají být spravovány tímto řešením.  Můžete zadat více než jeden název a oddělit každou čárkou (hodnoty nejsou malá a velká písmena).  Je podporováno použití zástupného znaku, pokud jsou cílem virtuální počítače ve všech skupinách prostředků v rámci předplatného.
   - Specity **seznamu vyloučení virtuálních počítačů (string)**, což je název na nebo víc virtuálních počítačů z cílové skupiny prostředků.  Můžete zadat více než jeden název a oddělit každou čárkou (hodnoty nejsou malá a velká písmena).  Pomocí zástupného znaku je podporována.
   - Vyberte hodnotu pro položku **Plán**. Jedná se o opakované datum a čas spouštění a zastavování virtuálních počítačů v cílových skupinách prostředků.  Ve výchozím plánu je nakonfigurován na časové pásmo UTC a vybrat jinou oblast, není k dispozici.  Pokud chcete nakonfigurovat plán pro vaše konkrétní časové pásmo po dokončení konfigurace řešení, najdete v části [úprava plán spuštění a vypnutí](#modifying-the-startup-and-shutdown-schedule) níže.
   - Pro příjem **e-mailová oznámení** z Sendgridu, přijměte výchozí hodnotu **Ano** a zadejte platnou e-mailovou adresu.  Pokud vyberete **ne** a později se rozhodnete chcete dostávat e-mailová oznámení, budete muset znovu nasadit řešení znovu z Azure marketplace.  

10. Po dokončení konfigurace počátečních nastavení vyžadovaných pro příslušné řešení vyberte možnost **Vytvořit**.  Všechna nastavení budou ověřena a poté se provede pokus o nasazení řešení v rámci vašeho předplatného.  Dokončení tohoto procesu může trvat několik sekund a průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

## <a name="collection-frequency"></a>Četnost shromažďování dat

Automatizace úloh protokolu a úlohy datový proud dat je do úložiště analýzy protokolů konzumována každých pět minut.  

## <a name="using-the-solution"></a>Použití řešení

Když přidáte do řešení pro správu virtuálních počítačů, v pracovním prostoru analýzy protokolů z portálu Azure, **StartStopVM zobrazení** dlaždice se přidá na řídicí panel.  Tuto dlaždici zobrazí počet a grafické znázornění úlohy sady runbook pro řešení, které mají spuštěno a dokončeno úspěšně.<br><br> ![Dlaždice Zobrazení StartStopVM správy virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

V účtu Automation, můžete používat a spravovat řešení výběrem **prostoru** možnost a na stránce analýzy protokolů vyberte **řešení** v levém podokně.  Na **řešení** vyberte řešení **Start-Stop-VM [prostoru]** ze seznamu.<br><br> ![Seznam řešení v analýzy protokolů](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Výběr řešení se zobrazí **Start-Stop-VM [prostoru]** řešení okno, kde můžete zkontrolovat důležité podrobnosti, jako **StartStopVM** dlaždici, jako je třeba v pracovním prostoru analýzy protokolů, které Zobrazí počet a grafické znázornění úlohy sady runbook pro řešení, které byly zahájeny a byly úspěšně dokončeny.<br><br> ![Stránka řešení automatizace správy aktualizací](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Odsud můžete také provádět další analýzu záznamů úlohy kliknutím na dlaždici prstenec a na řídicím panelu řešení zobrazuje historie úlohy, předem definované protokolu vyhledávací dotazy, a přepínač tak, aby portálu pokročilé analýzy protokolů pro vyhledávání podle vyhledávací dotazy.  

Zahrnuje všechny nadřazené sady runbook *WhatIf* parametr, který v případě nastavena na **True**, podporuje s podrobnostmi o přesné chování sady runbook provede, když spustit, aniž by *WhatIf* parametr a ověří správnou virtuální počítače jsou cílové.  Sady Runbook pouze provede jeho definované akce při *WhatIf* parametr je nastaven na **False**.  


### <a name="scenario-1-daily-stopstart-vms-across-a-subscription-or-target-resource-groups"></a>Scénář 1: Denně zastavit a spustit virtuální počítače napříč předplatné nebo cílové skupiny prostředků 

Toto je výchozí konfigurace, při prvním nasazení řešení.  Například můžete nakonfigurovat ukončení všech virtuálních počítačů mezi předplatné večer při nechte pracovní a spustit v ráno, když jste zpátky v kanceláři. Když konfigurujete plány ** naplánovaná-StartVM "a **naplánovaná StopVM** během nasazení, spuštění a zastavení cílových virtuálních počítačů.
>[!NOTE]
>Časové pásmo je aktuální časové pásmo, když konfigurujete parametru času plán; ale je uložený ve formátu UTC ve službě Azure Automation.  Nemusíte dělat žádné převodu časového pásma, jako to je zpracování během nasazení.

Můžete určit, které virtuální počítače, které se nacházejí v oboru nakonfigurováním dvou proměnných - **External_Start_ResourceGroupNames**, ** External_Stop_ResourceGroupNames, a **External_ExcludeVMNames**.  

>[!NOTE]
> Hodnotu pro proměnnou **cílové názvy ResourceGroup**"je uloženo jako hodnota pro obě **External_Start_ResourceGroupNames** a **External_Stop_ResourceGroupNames** proměnné. Pro další členitosti můžete upravit, každá z těchto proměnných pro různé skupiny prostředků.  Pro spuštění akce použití **External_Start_ResourceGroupNames** a pro použití akce zastavení **External_Stop_ResourceGroupNames** místo. Nové virtuální počítače se automaticky přidají do spuštění a zastavení plány.

Chcete-li otestovat a ověřit konfiguraci, spusťte ručně **ScheduledStartStop_Parent** runbook a nastavte *akce* parametru **spustit** nebo **zastavit**  a *WhatIf* parametru **true**.<br><br> ![Konfigurovat parametry runbooku](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> To umožňuje zobrazit náhled akci, která by proběhnout a proveďte požadované změny v případě potřeby před implementací proti produkční virtuálních počítačů.  Po umíte, můžete provést ručně sady runbook s parametrem nastavena na **false** , nebo nechte plán Automation **plán-StartVM** a **plán-StopVM**spustit automaticky po předepsaný plán.

### <a name="scenario-2-sequence-the-stopstart-vms-across-a-subscription-using-tags"></a>Scénář 2: Pořadí zastavit a spustit virtuální počítače napříč předplatné pomocí značek
V prostředí, které obsahuje dvě nebo více součástí na podpora distribuovaných mezi několika virtuálními počítači podpora pořadí, které jsou součástí spustit nebo zastavit v pořadí je důležité.  Toho lze dosáhnout pomocí následujících kroků.

1. Přidání **SequenceStart** a **SequenceStop** značku s hodnotou kladné celé číslo na virtuální počítače napříč vaše předplatné, které jsou cílem v **External_Start_ResourceGroupNames**a  **External_Stop*ResourceGroupNames** proměnné.  Spuštění a ukončení akce se provede ve vzestupném pořadí.  Další postupy k označení virtuálního počítače najdete v tématu [značku virtuálního počítače s Windows v Azure](../virtual-machines/windows/tag.md) a [značku virtuální počítač s Linuxem v Azure](../virtual-machines/linux/tag.md)
2. Změnit plány **Sequenced-StartVM** a **Sequenced StopVM** datum a čas k plnění požadavků a povolit plán.  

Chcete-li otestovat a ověřit konfiguraci, spusťte ručně **SequencedStartStop_Parent** runbook a nastavte *akce* parametru **spustit** nebo **zastavit**  a *WhatIf* parametru **True**.<br><br> ![Konfigurovat parametry runbooku](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> To umožňuje zobrazit náhled akci, která by proběhnout a proveďte požadované změny v případě potřeby před implementací proti produkční virtuálních počítačů.  Po umíte, můžete provést ručně sady runbook s parametrem nastavena na **false** , nebo nechte plán Automation **Sequenced-StartVM** a **Sequenced-StopVM**spustit automaticky po předepsaný plán.  

### <a name="scenario-3-auto-stopstart-vms-across-a-subscription-based-on-cpu-utilization"></a>Scénář 3: Automaticky zastavit a spustit virtuální počítače napříč odběru na základě využití procesoru
Ke správě náklady na provozování virtuálních počítačů v rámci vašeho předplatného, toto řešení může pomoct vyhodnocením virtuálních počítačích Azure, který se používá během období mimo špičku, například po hodinách a automaticky zastaví je-li využití procesoru je menší než x %.  

Ve výchozím nastavení řešení je předem nakonfigurovaný k vyhodnocení metrika procento procesoru a průměrné využití je 5 % nebo méně.  Tím je řízeno následující proměnné a lze upravit, pokud jejich výchozí hodnoty nevyhovují vašim požadavkům:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Můžete povolit jenom cílení akci vůči předplatného a skupiny prostředků nebo konkrétní seznam virtuálních počítačů, ale ne obojí.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Cíl akce zastavení pro skupinu předplatného a prostředků

1. Konfigurace **External_Stop_ResourceGroupNames** a **External_ExcludeVMNames** proměnné, které chcete zadat cílový virtuální počítače.  
2. Povolení a aktualizovat **Schedule_AutoStop_CreateAlert_Parent** plán.
3. Spustit **AutoStop_CreateAlert_Parent** runbook s *akce* parametr nastaven na **spustit** a *WhatIf* parametr nastaven na  **Hodnota TRUE,** náhled změny.

#### <a name="target-stop-action-by-vm-list"></a>Akce zastavení cílového seznamu virtuálních počítačů

1. Spustit **AutoStop_CreateAlert_Parent** runbook s *akce* parametr nastaven na **spustit**, přidejte čárkami oddělený seznam virtuálních počítačů v *VMList* parametr a *WhatIf* parametr nastaven na **True** náhled změny.  
2. Konfigurace **External_ExcludeVMNames** parametr s čárkami oddělený seznam virtuálních počítačů (VM1, virtuálního počítače 2, VM3).
3. Tento scénář nectí **External_Start_ResourceGroupNames** a **External_Stop_ResourceGroupnames** varabies.  V tomto scénáři musíte vytvořit vlastní plán Automation. Podrobnosti najdete v tématu [plánování runbooku ve službě Azure Automation](../automation/automation-schedules.md).

Teď, když máte plán pro zastavení virtuálních počítačů podle využití procesoru, budete muset povolit jedno z níže plány, které je spustit.

* Cíl zahájení předplatného a skupiny prostředků.  Podívejte se na postup v [scénář č. 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) pro účely testování a povolení **naplánovaná-StartVM** plán.
* Cíl zahájení předplatné, skupinu prostředků, a značky.  Podívejte se na postup v [scénář č. 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) pro účely testování a povolení "Sequenced-StartVM" naplánovat.


### <a name="configuring-e-mail-notifications"></a>Konfigurace e-mailových oznámení

Chcete-li konfigurovat e-mailová oznámení po nasazení řešení, můžete upravit následující tři proměnné:

* External_EmailFromAddress – zadejte adresu odesílatele e-mailu
* External_EmailToAddress - čárkami oddělený seznam e-mailové adresy (user@hotmail.com, user@outlook.com) pro příjem oznámení e-mailů
* External_IsSendEmail - li nastavena na **Ano**, bude dostávat e-maily a chcete zakázat e-mailová oznámení, nastavte hodnotu na **ne**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Úprava plány spuštění a vypnutí

Správa spuštění a vypnutí plány v tomto řešení následuje stejný postup, jak je uvedeno v [plánování runbooku ve službě Azure Automation](automation-schedules.md).     

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics

Služba Automation vytváří v úložišti OMS dva typy záznamů.

### <a name="job-logs"></a>Protokoly úloh

Vlastnost | Popis|
----------|----------|
Volající |  Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.|
Kategorie | Klasifikace typu dat.  Službě Automation odpovídá hodnota JobLogs.|
CorrelationId | Identifikátor GUID, který představuje ID korelace úlohy runbooku.|
JobId | Identifikátor GUID, který představuje ID úlohy runbooku.|
operationName | Určuje typ operace prováděné v Azure.  Službě Automation odpovídá hodnota Job.|
resourceId | Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku.|
ResourceGroup | Určuje název skupiny prostředků příslušné úlohy runbooku.|
ResourceProvider | Určuje službu Azure poskytující prostředky, které můžete nasadit a spravovat.  Službě Automation odpovídá hodnota Azure Automation.|
ResourceType | Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku.|
resultType | Stav úlohy runbooku.  Možné hodnoty:<br>- Spuštěno<br>- Zastaveno<br>- Pozastaveno<br>- Neúspěch<br>- Úspěch|
resultDescription | Popisuje výsledný stav úlohy runbooku.  Možné hodnoty:<br>- Úloha se spustila<br>- Zpracování úlohy se nezdařilo<br>- Úloha je dokončená|
RunbookName | Určuje název runbooku.|
SourceSystem | Určuje zdrojový systém pro odeslaná data.  Službě Automation odpovídá hodnota :OpsManager.|
StreamType | Určuje typ události. Možné hodnoty:<br>- Podrobné<br>- Výstup<br>- Chyba<br>- Varování|
SubscriptionId | Určuje ID předplatného úlohy.
Time | Datum a čas provedení úlohy runbooku.|


### <a name="job-streams"></a>Datové proudy úlohy

Vlastnost | Popis|
----------|----------|
Volající |  Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.|
Kategorie | Klasifikace typu dat.  Službě Automation odpovídá hodnota JobStreams.|
JobId | Identifikátor GUID, který představuje ID úlohy runbooku.|
operationName | Určuje typ operace prováděné v Azure.  Službě Automation odpovídá hodnota Job.|
ResourceGroup | Určuje název skupiny prostředků příslušné úlohy runbooku.|
resourceId | Určuje ID prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku.|
ResourceProvider | Určuje službu Azure poskytující prostředky, které můžete nasadit a spravovat.  Službě Automation odpovídá hodnota Azure Automation.|
ResourceType | Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku.|
resultType | Výsledek úlohy runbooku v době, kdy byla událost vygenerována.  Možné hodnoty:<br>- Probíhá zpracování|
resultDescription | Zahrnuje výstupní datový proud z runbooku.|
RunbookName | Název runbooku.|
SourceSystem | Určuje zdrojový systém pro odeslaná data.  Službě Automation odpovídá hodnota OpsManager.|
StreamType | Typ datového proudu úlohy. Možné hodnoty:<br>- Průběh<br>- Výstup<br>- Varování<br>- Chyba<br>- Ladění<br>- Podrobné|
Time | Datum a čas provedení úlohy runbooku.|

Při provádění jakékoli hledání v protokolech, které vrací záznamy kategorie **JobLogs** nebo **JobStreams**, můžete vybrat zobrazení **JobLogs** nebo **JobStreams**, které obsahuje sadu dlaždic se souhrnem aktualizací vrácených hledáním.

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech

V následující tabulce jsou uvedeny ukázky hledání v protokolech pro záznamy úloh shromážděné tímto řešením. 

Dotaz | Popis|
----------|----------|
Najít úlohy pro runbook ScheduledStartStop_Parent, které byly úspěšně dokončeny | hledání kategorie == "JobLogs" &#124; kde (RunbookName_s == "ScheduledStartStop_Parent") &#124; kde (ResultType == "Dokončeno") &#124; shrnout AggregatedValue = count() podle ResultType, bin (TimeGenerated, 1 hod) &#124; Řadit podle TimeGenerated desc|
Najít úlohy pro runbook SequencedStartStop_Parent, které byly úspěšně dokončeny | hledání kategorie == "JobLogs" &#124; kde (RunbookName_s == "SequencedStartStop_Parent") &#124; kde (ResultType == "Dokončeno") &#124; shrnout AggregatedValue = count() podle ResultType, bin (TimeGenerated, 1 hod) &#124; Řadit podle TimeGenerated desc

## <a name="removing-the-solution"></a>Odebrání řešení

Pokud se rozhodnete, že už budete muset použít řešení všechny další, odstraňte jej z účtu služby Automation.  Odstraněním tohoto řešení se jenom odeberou sady runbook, neodstraní plány nebo proměnné, které byly vytvořeny při přidání řešení.  Tyto prostředky, které budete muset ručně odstranit, pokud se nepoužívá s jiné runbooky.  

Pokud chcete odstranit řešení, proveďte následující kroky:

1.  Z vašeho účtu Automation vyberte **prostoru** v levém podokně.  
2.  Na **řešení** vyberte řešení **Start-Stop-VM [prostoru]**.  Na **VMManagementSolution [prostoru]** stránky, klikněte na nabídku **odstranit**.<br><br> ![Odstranit řešení pro správu virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  V **odstranit řešení** okně potvrďte, kterou chcete odstranit řešení.
4.  Při ověření informace a řešení, se odstraní, můžete sledovat průběh v části **oznámení** z nabídky.  Se vrátí **řešení** stránky po spuštění procesu odebrání řešení.  

Účet Automation a pracovní prostor analýzy protokolů nejsou odstraněna jako součást tohoto procesu.  Pokud nechcete zachovat pracovní prostor analýzy protokolů, musíte ručně odstranit.  To můžete provést taky z portálu Azure.   Z domovské obrazovky portálu Azure, vyberte **analýzy protokolů** a pak na **analýzy protokolů** okně vyberte pracovní prostor a klikněte na tlačítko **odstranit** na v nabídce okno nastavení pracovního prostoru.  
      
## <a name="next-steps"></a>Další kroky

- Další informace o tom, jak vytvářet různé vyhledávací dotazy a kontrolovat protokoly úloh služby Automation s použitím služby Log Analytics, najdete v článku [Vyhledávání protokolů v Log Analytics](../log-analytics/log-analytics-log-searches.md)
- Další informace o spouštění runbooků, postupy při monitorování úloh runbooků a další technické podrobnosti najdete v článku [Sledování úlohy runbooku](automation-runbook-execution.md).
- Další informace o analýzy protokolů a kolekci zdrojů dat naleznete v tématu [shromažďování Azure úložiště dat v přehledu analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)






   

