---
title: " Opravit virtuální počítač Azure výstrah pomocí Runbooků Automation | Microsoft Docs"
description: "Tento článek ukazuje, jak integrovat Azure Automation runbook virtuální počítač Azure výstrahy a automatické odstranění problémů"
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: csand;magoedte
ms.openlocfilehash: 18cccc88ab74235722e2f4886671fc483ab67da8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Scénář automatizace Azure – opravit virtuální počítač Azure výstrahy
Azure Automation a virtuální počítače Azure byla vydána nová funkce umožňuje nakonfigurovat výstrahy virtuálního počítače (VM) ke spuštění sady Automation runbook. Tato nová funkce umožňuje automaticky provádět standardní nápravy v reakci na výstrahy virtuálních počítačů, jako je restartování nebo zastavení virtuálního počítače.

Při vytváření virtuálních počítačů pravidlo výstrahy bylo dřív možné [zadejte webhook automatizace](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) k sadě runbook ke spuštění sady runbook vždy, když se výstraha. Ale to vyžaduje k práci při vytváření sady runbook, vytváření webhooku pro sadu runbook a pak kopírování a vkládání webhook při vytváření pravidla výstrah. V této verzi nové proces je mnohem jednodušší, protože sady runbook můžete přímo vybrat ze seznamu během vytváření pravidlo výstrahy, a můžete si vybrat účet Automation, který se spouští sada runbook nebo snadno vytvořit účet.

V tomto článku jsme ukážeme, jak je snadné vytvořit výstrahu virtuální počítač Azure a nakonfigurovat runbook služby automatizace ke spuštění, kdykoli se aktivuje výstraha. Příklad scénáře zahrnují restartování virtuálního počítače, pokud využití paměti překročí některé prahovou hodnotu z důvodu aplikace ve virtuálním počítači s nevrácenou pamětí nebo zastavení virtuálního počítače při uživatelského času procesoru bylo nižší než 1 % po poslední hodinu a není používán. Vysvětlíme taky, jak automatické vytvoření objektu v účtu Automation služby zjednodušuje použití sad runbook v Azure výstrahy nápravu.

## <a name="create-an-alert-on-a-vm"></a>Vytvořit výstrahu na virtuálním počítači
Proveďte následující postup pro konfiguraci výstrahu pro spuštění sady runbook při jeho prahová hodnota byla splněna.

> [!NOTE]
> V této verzi jsme podporují jenom V2 virtuální počítače a podporu pro classic, který bude brzy přidat virtuální počítače.  
> 
> 

1. Přihlaste se k portálu Azure a klikněte na tlačítko **virtuální počítače**.  
2. Vyberte jednu z vašich virtuálních počítačů.  
3. Na obrazovce virtuálních počítačů v **monitorování** klikněte na tlačítko **výstrah pravidla**.
4. Na **výstrah pravidla** podokně klikněte na tlačítko **přidat upozornění**.

Otevře **přidání pravidla výstrahy** stránky, kde můžete konfigurovat podmínky pro výstrahu a zvolit si takový jeden nebo všechny tyto možnosti: odeslání e-mailu, použijte webhook, jehož a předat výstrahy do jiného systému, popř. spuštěn Sada Automation runbook v odpovědi se pokusí opravit tento problém.

## <a name="configure-a-runbook"></a>Konfigurace sady runbook
Konfigurace sady runbook spouštět, když je dosaženo prahové hodnoty pro výstrahu virtuálního počítače, vyberte **sady Automation Runbook**. V **konfigurace runbook** podokně, můžete vybrat spuštění sady runbook a účet Automation ke spuštění sady runbook.

![Nakonfigurujte runbook služby automatizace a vytvořte nový účet Automation.](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Pro tuto verzi můžete zvolit ze tří sady runbook, které poskytuje služba – restartování virtuálního počítače, zastavte virtuální počítač nebo odeberte virtuální počítač (odstranit).  Umožňuje vybrat jiné sady runbook nebo jeden z vlastní sady runbook bude k dispozici v budoucí verzi.
> 
> 

![Vybírejte z těchto možností sady Runbook](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Po výběru tři dostupné sady runbook, **účet Automation** rozevíracím seznamu se zobrazí a můžete vybrat účet automation runbook se spustit jako. Runbooky potřebují spustit v kontextu [účet Automation](automation-security-overview.md) se ve vašem předplatném Azure. Můžete vybrat účet Automation, zda jste již vytvořili, nebo máte vytvořený pro vás nový účet automatizace.

Sady runbook, které jsou k dispozici ověřování v Azure pomocí objektu služby. Pokud zvolíte možnost spuštění sady runbook v jednom z vaší existující účty Automation, můžeme automaticky službu hlavní pro vás vytvořil. Pokud se rozhodnete vytvořit nový účet Automation, pak automaticky vytvoříme účet a objekt služby. V obou případech jsou dva prostředky také vytvořené v účtu služby Automation – certifikát asset s názvem **AzureRunAsCertificate** a připojení asset s názvem **AzureRunAsConnection**. Použití sady runbook **AzureRunAsConnection** k ověření pomocí Azure, abyste mohli provádět akce správy u virtuálního počítače.

> [!NOTE]
> Objekt služby se vytvoří v oboru předplatného a má přiřazenou roli Přispěvatel. Tato role se vyžaduje, aby pro účet, který chcete mít oprávnění ke spouštění runbooků služeb automatizace spravovat virtuální počítače Azure.  Vytvoření Automaton účet nebo instančního objektu je jednorázové událost. Po vytvoření, můžete tento účet ke spuštění sady runbook pro jiné výstrahy virtuálního počítače Azure.
> 
> 

Když kliknete na tlačítko **OK** výstrahy je nakonfigurován, a pokud jste vybrali možnost vytvořit nový účet Automation, vytvoří se společně s objektu služby.  Může to trvat několik sekund.  

![Konfigurace sady Runbook](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Po dokončení konfigurace, zobrazí se název sady runbook v **přidání pravidla výstrahy** stránky.

![Runbook nakonfigurovat](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Klikněte na tlačítko **OK** v **přidání pravidla výstrahy** stránky.  Pravidlo výstrahy se vytvoří a aktivovat, pokud je virtuální počítač ve spuštěném stavu.

### <a name="enable-or-disable-a-runbook"></a>Povolit nebo zakázat sady runbook
Pokud je runbook nakonfigurovaný pro výstrahy, ji můžete vypnout bez odebrání konfigurace sady runbook. Můžete ponechat oznámení spuštění a případně testování pravidla výstrah a později znovu povolit sady runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Vytvoření sady runbook, který funguje s Azure výstrahy
Když zvolíte sady runbook v rámci Azure pravidla výstrahy, sada runbook musí mít logiku v ho ke správě dat výstrah, který je předán.  Pokud sady runbook je nakonfigurovaná v pravidlo výstrahy, vytvoří se webhook, jehož pro sadu runbook; Tento webhooku se pak používá ke spuštění sady runbook pokaždé, když se aktivuje výstrahu.  Vlastní volání pro spuštění sady runbook je požadavek HTTP POST na adresu URL webhooku. Text požadavku POST obsahuje objekt uvedena ve správném formátu JSON, který obsahuje užitečné vlastnosti související výstrahy.  Jak vidíte níže, data výstrah obsahuje podrobnosti, třeba ID předplatného, název skupiny prostředků, resourceName a typ prostředku.

### <a name="example-of-alert-data"></a>Příklad dat výstrah
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Když služba webhook automatizace obdrží HTTP POST extrahuje data výstrah a předává je pro sadu runbook v WebhookData vstupní parametr runbooku.  Níže je ukázkové sady runbook, který ukazuje, jak pomocí parametru WebhookData a extrahování dat výstrah a použít ho ke správě prostředků Azure, který aktivoval výstrahu.

### <a name="example-runbook"></a>Ukázkový runbook
```
#  This runbook restarts an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a>Souhrn
Když konfigurujete výstrahu na virtuální počítač Azure, nyní máte možnost snadno konfigurovat automaticky provádět akci nápravy, která aktivuje výstrahu, když runbook služby automatizace. Pro tuto verzi můžete ze sady runbook se znovu spustit, zastavit nebo odstranění virtuálního počítače v závislosti na vašem scénáři výstrahy. Toto je právě začátku povolení scénáře, kde můžete řídit akce (oznámení, řešení potíží s nápravy), které automaticky provede, když se aktivuje výstrahu.

## <a name="next-steps"></a>Další kroky
* První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* Další informace o typech runbooků, jejich výhodách a omezeních najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).

