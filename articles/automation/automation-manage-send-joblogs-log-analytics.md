---
title: "Předávání dat úlohu Azure Automation k analýze protokolů OMS | Microsoft Docs"
description: "Tento článek ukazuje, jak odesílat stav úlohy a runbook proudy úlohy Microsoft Operations Management Suite Log Analytics k poskytování další aspekty a správu."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: b3b9457e6c8ce501a7295859923838460e7ab6cc
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Předávání zpráv o stavu úlohy a datové proudy úlohy z Automatizace analýzy protokolů (OMS)
Automatizace můžete odeslat runbook datové proudy úlohy stavu a úlohu do pracovního prostoru analýzy protokolů Microsoft Operations Management Suite (OMS).  Protokoly úlohy a datové proudy úlohy jsou viditelné na portálu Azure nebo v prostředí PowerShell pro jednotlivé úlohy a to umožňuje provádět jednoduché šetření. Pomocí analýzy protokolů můžete nyní:

* Pohled na vaše úlohy automatizace
* Aktivační událost e-mailem nebo výstrahy podle runbook stav úlohy (například chybných nebo pozastavených)
* Zápis pokročilými dotazy napříč vaše datové proudy úlohy
* Vazbu mezi úlohy v účtech Automation
* Vizualizace historii úlohy v čase     

## <a name="prerequisites-and-deployment-considerations"></a>Požadavky a důležité informace o nasazení
Chcete-li zahájit odesílání protokolů služby Automation k analýze protokolů, je třeba:

1. Listopadu 2016 nebo novější vydání [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Pracovní prostor analýzy protokolů. Další informace najdete v tématu [začít pracovat s analýzy protokolů](../log-analytics/log-analytics-get-started.md). 
3. ID prostředku pro váš účet Azure Automation.

Najít ResourceId pro váš účet Azure Automation a pracovní prostor analýzy protokolů, spusťte následující prostředí PowerShell:

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Pokud máte více účtů Automation, nebo pracovní prostory v výstup z předchozích příkazů *název* budete muset nakonfigurovat a zkopírujte hodnotu pro *ResourceId*.

Pokud potřebujete najít *název* účtu Automation na portálu Azure vyberte svůj účet Automation z **účet Automation** a vyberte **všechna nastavení**.  V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.  V okně **Vlastnosti** si můžete tyto hodnoty opsat.<br> ![Vlastnosti účtu Automation](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Nastavení integrace s analýzy protokolů
1. V počítači, spusťte **prostředí Windows PowerShell** z **spustit** obrazovky.  
2. Zkopírujte a vložte následující prostředí PowerShell a upravit její hodnotu `$workspaceId` a `$automationAccountId`.  Pro `-Environment` parametr platné hodnoty jsou *AzureCloud* nebo *AzureUSGovernment* v závislosti na práci v prostředí cloudu.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Po spuštění tohoto skriptu, zobrazí se záznamy v analýzy protokolů během deseti minut nové JobLogs nebo JobStreams zapisovaný.

Pokud chcete zobrazit protokoly, spusťte následující dotaz ve vyhledávání protokolu analýzy protokolů:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Ověření konfigurace
Potvrďte, že váš účet Automation odesílá protokoly do pracovního prostoru analýzy protokolů, zkontrolujte, jestli jsou správně nastavené diagnostiky na účtu Automation pomocí prostředí PowerShell následující:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Ve výstupu zajistěte, aby:
+ V části *protokoly*, hodnota *povoleno* je *True*
+ Hodnota *WorkspaceId* je nastaven na ResourceId pracovního prostoru analýzy protokolů


## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Diagnostika z Azure Automation vytvoří dva typy záznamů v analýzy protokolů a jsou označené jako **typ = AzureDiagnostics**.

### <a name="job-logs"></a>V protokolech úloh
| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas provedení úlohy runbooku. |
| RunbookName_s |Název runbooku. |
| Caller_s |Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| Tenant_g | Identifikátor GUID, který identifikuje klienta pro volajícího. |
| JobId_g |Identifikátor GUID, který představuje ID úlohy runbooku. |
| ResultType |Stav úlohy runbooku.  Možné hodnoty:<br>– Nový<br>- Spuštěno<br>- Zastaveno<br>- Pozastaveno<br>- Neúspěch<br>-Byla dokončena |
| Kategorie | Klasifikace typu dat.  Službě Automation odpovídá hodnota JobLogs. |
| OperationName | Určuje typ operace prováděné v Azure.  Hodnota pro automatizaci je úloha. |
| Prostředek | Název účtu Automation. |
| SourceSystem | Jak analýzy protokolů shromáždit data. Vždy *Azure* Azure Diagnostics. |
| ResultDescription |Popisuje výsledný stav úlohy runbooku.  Možné hodnoty:<br>- Úloha se spustila<br>- Zpracování úlohy se nezdařilo<br>- Úloha je dokončená |
| CorrelationId |Identifikátor GUID, který představuje ID korelace úlohy runbooku. |
| ID prostředku |Určuje id prostředku účet Azure Automation runbook. |
| SubscriptionId | Předplatné Azure Id (GUID) pro účet služby Automation. |
| ResourceGroup | Název skupiny prostředků pro účet služby Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Datové proudy úlohy
| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas provedení úlohy runbooku. |
| RunbookName_s |Název runbooku. |
| Caller_s |Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| StreamType_s |Typ datového proudu úlohy. Možné hodnoty:<br>- Průběh<br>- Výstup<br>- Varování<br>- Chyba<br>- Ladění<br>- Podrobné |
| Tenant_g | Identifikátor GUID, který identifikuje klienta pro volajícího. |
| JobId_g |Identifikátor GUID, který představuje ID úlohy runbooku. |
| ResultType |Stav úlohy runbooku.  Možné hodnoty:<br>– V průběhu |
| Kategorie | Klasifikace typu dat.  Službě Automation odpovídá hodnota JobStreams. |
| OperationName | Určuje typ operace prováděné v Azure.  Hodnota pro automatizaci je úloha. |
| Prostředek | Název účtu Automation. |
| SourceSystem | Jak analýzy protokolů shromáždit data. Vždy *Azure* Azure Diagnostics. |
| ResultDescription |Zahrnuje výstupní datový proud z runbooku. |
| CorrelationId |Identifikátor GUID, který představuje ID korelace úlohy runbooku. |
| ID prostředku |Určuje id prostředku účet Azure Automation runbook. |
| SubscriptionId | Předplatné Azure Id (GUID) pro účet služby Automation. |
| ResourceGroup | Název skupiny prostředků pro účet služby Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Zobrazení automatizace přihlásí analýzy protokolů
Teď, když jste spustili odesílání protokolů úlohy služby Automation k analýze protokolů, podíváme se, co můžete dělat s tyto protokoly uvnitř analýzy protokolů.

Pokud chcete zobrazit protokoly, spusťte následující dotaz:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Odeslat e-mail, dojde k selhání úlohy runbooku nebo pozastaví
Jeden z našich zákazníků nejvyšší požádá, je pro možnost odesílat e-mailem nebo jako text v případě problémů s úlohy runbooku.   

Pokud chcete vytvořit pravidlo výstrahy, začněte vytvořením hledání protokolů pro záznamy úlohy sady runbook, které by měla vyvolat výstrahu.  Klikněte **výstraha** tlačítko Vytvořit a nakonfigurovat pravidlo výstrahy.

1. Na stránce Přehled protokolu Analytics klikněte na tlačítko **hledání protokolů**.
2. Vytvoření vyhledávací dotaz protokolu pro upozornění zadáním následujících hledání do pole dotazu: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` můžete taky Seskupit podle RunbookName pomocí:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Pokud jste nastavili protokolů z více než jeden účet Automation nebo odběr do pracovního prostoru, můžete je seskupovat vaše předplatné a účet Automation.  Název účtu Automation může být odvozen z pole prostředků do vyhledávání JobLogs.  
3. Chcete-li otevřít **přidat pravidlo výstrahy** obrazovky, klikněte na tlačítko **výstrah** v horní části stránky. Další informace o možnostech konfigurace upozornění najdete v tématu [výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Najít všechny úlohy, které byly dokončeny s chybami
Kromě zobrazení výstrah o selhání, můžete najít při úlohy runbooku se neukončující chybu. V těchto případech prostředí PowerShell vytvoří chybový proud, ale chyby neukončující nezpůsobí úlohu pozastavit nebo selže.    

1. V pracovním prostoru analýzy protokolů, klikněte na tlačítko **hledání protokolů**.
2. V poli dotazu zadejte `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` a pak klikněte na **vyhledávání**.

### <a name="view-job-streams-for-a-job"></a>Zobrazení datové proudy úlohy pro úlohu
Když ladíte úlohu, můžete také viděl datové proudy úlohy.  Následující dotaz zobrazí všechny datové proudy pro jednu úlohu s identifikátorem GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Zobrazit stav historie úlohy
Nakonec můžete vizualizovat historii úlohy v čase.  Tento dotaz můžete použít k vyhledání stav úloh v čase.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS historie úlohy stavu grafu](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Souhrn
Odeslání dat datového proudu a stav úlohy automatizace k analýze protokolů, lze získat lepší přehled o stavu vaší automatizace úloh podle:
+ Nastavení výstrah upozornění v případě, že se vyskytl problém
+ Pomocí vlastních zobrazení a vyhledávací dotazy k vizualizaci výsledky sady runbook, stav úlohy sady runbook a další související klíčové ukazatele nebo metriky.  

Log Analytics poskytuje větší provozní viditelnost do automatizace úloh a může pomoct adresu incidenty rychlejší.  

## <a name="next-steps"></a>Další kroky
* Další informace o tom, jak vytvářet různé vyhledávací dotazy a kontrolovat protokoly úloh služby Automation s použitím služby Log Analytics, najdete v článku [Vyhledávání protokolů v Log Analytics](../log-analytics/log-analytics-log-searches.md)
* Chcete-li pochopit, jak vytvořit a ze sady runbook načíst výstupní a chybové zprávy, přečtěte si téma [Runbook výstup a zprávy](automation-runbook-output-and-messages.md)
* Další informace o spouštění runbooků, postupy při monitorování úloh runbooků a další technické podrobnosti najdete v článku [Sledování úlohy runbooku](automation-runbook-execution.md).
* Další informace o službě Log Analytics v OMS a o zdrojích pro shromažďování dat najdete v článku [Přehled shromažďování dat úložiště Azure ve službě Log Analytics](../log-analytics/log-analytics-azure-storage.md).
