---
title: "Volání runbooku Azure Automation z upozornění Log Analytics | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled způsobů, jak vyvolat runbook Automation z upozornění Microsoft OMS Log Analytics."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 3370efe7696a6ffe9013886e07392806e008993b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Volání runbooku Azure Automation z upozornění OMS Log Analytics

Pokud je v Log Analytics nakonfigurované upozornění tak, aby v případě, že výsledky odpovídají kritériím (jako je například dlouhotrvající špička využití procesoru nebo selhání konkrétního procesu aplikace důležitého pro fungování podnikové aplikace), vytvořilo záznam o upozornění a zapsalo odpovídající událost do protokolu událostí Windows, může takové upozornění automaticky spouštět runbook Automation ve snaze automaticky napravit problém.  

V konfiguraci upozornění existují dvě možnosti volání runbooku:

1. Pomocí webhooku.
   * Toto je jediná dostupná možnost, pokud váš pracovní prostor OMS není propojený s účtem Automation.
   * Tato možnost je k dispozici i v případě, že již máte účet Automation propojený s pracovním prostorem OMS.  

2. Přímým výběrem runbooku.
   * Tato možnost je dostupná, pouze 
   *  pokud je pracovní prostor OMS propojený s účtem Automation.  

## <a name="calling-a-runbook-using-a-webhook"></a>Volání runbooku pomocí webhooku

Webhook umožňuje spuštění konkrétního runbooku v Azure Automation prostřednictvím jednoho požadavku HTTP.  Před konfigurací [upozornění Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules), aby jako akci upozornění volalo runbook pomocí webhooku, je potřeba nejprve vytvořit webhook pro runbook, který je touto metodou volán.  Proveďte kroky v článku popisujícím [vytvoření webhooku](automation-webhooks.md#creating-a-webhook) a nezapomeňte si poznamenat adresu URL webhooku, abyste na něj mohli odkazovat při konfiguraci pravidla upozornění.   

## <a name="calling-a-runbook-directly"></a>Přímé volání runbooku

Pokud máte ve svém pracovním prostoru OMS nainstalovanou a nakonfigurovanou nabídku Automation & Control, při konfiguraci možnosti akcí runbooku pro upozornění můžete všechny runbooky zobrazit v rozevíracím seznamu **Vybrat runbook** a vybrat konkrétní runbook, který chcete v reakci na upozornění spustit.  Vybraný runbook se může spustit v pracovním prostoru v cloudu Azure nebo v procesu Hybrid Runbook Worker.  Po vytvoření upozornění s možností runbooku se pro tento runbook vytvoří webhook.  Webhook zobrazíte tak, že přejdete do účtu Automation a pak do podokna webhooku vybraného runbooku.  Pokud odstraníte upozornění, webhook se neodstraní, ale uživatel může webhook odstranit ručně.  Není problém, když se webhook neodstraní, je to jenom osiřelá položka, kterou časem budete muset odstranit pro zachování organizovaného účtu Automation.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Vlastnosti runbooku (pro obě možnosti)

Obě metody volání runbooku z upozornění Log Analytics mají rozdílné vlastnosti chování, kterým je nutné porozumět před konfigurací pravidel upozornění.  

* Musíte mít vstupní parametr runbooku s názvem **WebhookData**, který je typu **Objekt**.  Může být povinný nebo volitelný.  Upozornění předává výsledky hledání do runbooku pomocí tohoto vstupního parametru.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )

*  Musíte mít kód, který převede WebhookData na objekt prostředí PowerShell.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* je pole objektů, z nichž každý obsahuje pole s hodnotami z jednoho výsledku hledání.

### <a name="webhookdata-inconsistencies-between-the-webhook-option-and-runbook-option"></a>Nekonzistence parametru WebhookData mezi možnostmi webhooku a runbooku

* Při konfiguraci upozornění volajícího webhook zadejte adresu URL webhooku, který jste vytvořili pro runbook, a klikněte na tlačítko **Otestovat webhook**.  Výsledný parametr WebhookData odeslaný do runbooku neobsahuje buď *.SearchResult*, nebo *.SearchResults*.

*  Toto upozornění se po uložení aktivuje, zavolá webhook a parametr WebhookData odeslaný do runbooku bude obsahovat *.SearchResult*.
* Pokud vytvoříte upozornění a nakonfigurujete jej, aby volalo runbook (což zároveň vytvoří webhook), upozornění při aktivaci odešle do runbooku parametr WebhookData obsahující *.SearchResults*.

Proto je ve výše uvedeném příkladu kódu nutné získat *.SearchResult*, pokud upozornění volá webhook, a naopak *.SearchResults*, pokud upozornění volá runbook přímo.

## <a name="example-walkthrough"></a>Ukázka podrobného postupu

Na následujícím příkladu grafického runbooku, který spouští službu pro Windows, ukazujeme, jak to funguje.<br><br> ![Spuštění grafického runbooku služby pro Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Runbook má jeden vstupní parametr typu **Objekt** s názvem **WebhookData**, který zahrnuje data webhooku předaná z upozornění a obsahuje *.SearchResults*.<br><br> ![Vstupní parametry runbooku](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

V tomto příkladu jsme v Log Analytics vytvořili dvě vlastní pole, *SvcDisplayName_CF* a *SvcState_CF*, pro extrakci zobrazovaného názvu a stavu služby (tj. spuštěná nebo zastavená) z události zapsané do protokolu událostí systému.  Následně vytvoříme pravidlo upozornění s tímto vyhledávacím dotazem: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"`. Díky tomu můžeme detekovat zastavení služby zařazování tisku v systému Windows.  Může se jednat o libovolnou službu, která vás zajímá, ale pro tento příklad používáme jednu z již existujících služeb, které jsou součástí operačního systému Windows.  Akce upozornění je nakonfigurována ke spouštění runbooku použitého v tomto příkladu v procesu Hybrid Runbook Worker, který je na cílovém systému povolený.   

Aktivita runbooku s kódem **Get Service Name from LA** (Získání názvu služby z Log Analytics) převede řetězec ve formátu JSON na objekt a filtrováním podle položky *SvcDisplayName_CF* extrahuje zobrazovaný název služby pro Windows, který předá do další aktivity. Ta před pokusem o restartování služby ověří, jestli je zastavená.  *SvcDisplayName_CF* je [vlastní pole](../log-analytics/log-analytics-custom-fields.md) vytvořené v Log Analytics pro účely tohoto příkladu.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Když se služba zastaví, pravidlo upozornění v Log Analytics zjistí shodu a aktivuje runbook, do kterého odešle kontext upozornění. Runbook provede ověření, jestli je služba zastavená, a pokud ano, pokusí se službu restartovat, ověří, že se spustila správně a vypíše výsledky.     

Pokud by váš účet Automation nebyl propojený s pracovním prostorem OMS, nakonfigurovali byste pravidlo upozornění s akcí webhooku aktivující runbook a runbook byste nakonfigurovali pro převod řetězce ve formátu JSON a filtrování položky *.SearchResult* podle pokynů uvedených výše.    

## <a name="next-steps"></a>Další kroky

* Další informace o upozorněních v Log Analytics a jejich vytváření najdete v tématu [Upozornění v Log Analytics](../log-analytics/log-analytics-alerts.md).

* Informace o tom, jak aktivovat runbooky pomocí webhooku, najdete v tématu [Webhooky Azure Automation](automation-webhooks.md).
