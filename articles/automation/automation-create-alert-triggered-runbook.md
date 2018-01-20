---
title: "Reakce na výstrahy Azure s runbook služby automatizace | Microsoft Docs"
description: "Zjistěte, jak se aktivovat sadu runbook spustit, když dojde k výstrahám Azure."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Reakce na výstrahy Azure s runbook služby automation.

[Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) poskytuje základní úroveň metriky a protokoly pro většina služeb v rámci Microsoft Azure. Runbooky služby Azure automation je možné volat pomocí [skupiny akcí](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) nebo z klasického výstrah pro automatizaci úloh podle výstrahy. Tento článek ukazuje, jak nakonfigurovat a spuštění sady runbook na základě těchto výstrah.

## <a name="alert-types"></a>Typy výstrah

Runbooky jsou podporované akce na všechny tři typy výstrah. Když výstrahu volá sadu runbook, je vlastní volání požadavek HTTP POST webhooku. Text požadavku POST obsahuje objekt uvedena ve správném formátu JSON, který obsahuje užitečné vlastnosti související výstrahy. Následující tabulka obsahuje odkazy na schéma datové části pro jednotlivé typy výstrah:

|Výstrahy  |Popis|Datová část schématu  |
|---------|---------|---------|
|[Classic metriky výstrahy](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Přijímat oznámení v případě jakékoli úrovni platformy metrika splňuje určité podmínky (například % využití procesoru na virtuálním počítači je větší než 90 za posledních 5 minut).| [Datová část schématu](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Výstraha aktivity protokolu](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Obdržet oznámení, když všechny nové události v protokolu aktivit Azure odpovídá konkrétní podmínky (například kdy dochází k operaci "Odstranit virtuální počítač" v myProductionResourceGroup nebo pokud se zobrazí nové události stavu služby s "Aktivní", jako stav).| [Datová část schématu](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Téměř v reálném čase metriky výstrahy](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Přijímat oznámení rychleji než metriky výstrahy v případě, že jeden nebo více metriky úrovně platformu splňují určitá kritéria (například je větší než 90 % využití procesoru na virtuálním počítači a umístění v síti je větší než 500 MB za posledních 5 minut).| [Datová část schématu](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Vzhledem k tomu, že data poskytované jednotlivé výstrahy se liší, jinak zpracovávané musí každá výstraha. V další části zjistěte, jak vytvořit sadu runbook pro zpracování těchto různé typy výstrah.

## <a name="create-a-runbook-to-handle-alerts"></a>Vytvoření sady runbook pro zpracování výstrahy

Chcete-li použití automatizace s výstrahami, musíte sadu runbook, která má logiku ke správě upozornění datové části JSON, který je předán do runbooku. Následující ukázkový runbook musí být volána z Azure výstrahy. Jak je popsáno v předchozí části, každý typ typu výstrahy má jiné schéma. Skript provede v datech webhooku v `WebhookData` runbook vstupní parametr výstrahou a vyhodnocuje datové části JSON k určení, který typ výstrahy byl použit. V následujícím příkladu se použije na výstrahu z virtuálního počítače. Načte data virtuálního počítače z datové části a použije tyto informace k zastavení virtuálního počítače. Připojení musí být nastavené v automatizace účet, kde je sada runbook byla spuštěna.

Sada runbook používá **AzureRunAsConnection** [účet Spustit jako](automation-create-runas-account.md) k ověření pomocí Azure, abyste mohli provádět akce správy u virtuálního počítače.

Následující skript prostředí PowerShell může být změněna pro použití s mnoha různých prostředků.

Následující příklad a k vytvoření runbook s názvem **Stop-AzureVmInResponsetoVMAlert** s ukázkou prostředí PowerShell.

1. Otevřete váš účet služby Automation.
1. V části **AUTOMATIZACE PROCESŮ** klikněte na **Runbooky**. Zobrazí se seznam runbooků.
1. Klikněte na tlačítko **Přidat runbook** v horní části seznamu. Na **přidání stránky**, vyberte **rychle vytvořit**.
1. Zadejte pro sadu runbook "Stop-AzureVmInResponsetoVMAlert" **název**a vyberte **prostředí PowerShell** pro **typ Runbooku**. Klikněte na možnost **Vytvořit**.
1. Následující příklad PowerShell zkopírujte do podokně upravit. Klikněte na tlačítko **publikovat** uložte a publikovat sadu runbook.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

.NOTES
   AUTHOR: Azure Automation Team
   LASTEDIT: 2017-11-22
#>

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {
        # This is the near-real-time Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $ResourceName = $AlertContext.resourceName
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
        # This is the Activity Log Alert schema
        $AlertContext = [object] (($WebhookBody.data).context).activityLog
        $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq $null) {
        # This is the original Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $status = $WebhookBody.status
    }
    else {
        # Schema not supported
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    Write-Verbose "status: $status" -Verbose
    if ($status -eq "Activated")
    {
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId
        Write-Verbose "resourceType: $ResourceType" -Verbose
        Write-Verbose "resourceName: $ResourceName" -Verbose
        Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
        Write-Verbose "subscriptionId: $SubId" -Verbose

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

            # Authenticate to Azure with service principal and certificate and set subscription
            Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
            $ConnectionAssetName = "AzureRunAsConnection"
            Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Verbose "Authenticating to Azure with service principal." -Verbose
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Write-Verbose "Setting subscription to work against: $SubId" -Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Stop the VM
            Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
            Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
            # [OutputType(PSAzureOperationResponse")]
        }
        else {
            # ResourceType not supported
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    }
    else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Vytvořit skupinu akce

Skupinu akcí je kolekce akcí, které na základě z výstrahu. Runbooky jsou pouze jedním z mnoha akce, které jsou k dispozici skupinami akce.

1. Na portálu, vyberte **monitorování**.

1. Vyberte **skupiny akcí** pod **nastavení**.

1. Vyberte **přidat akci skupinu**a vyplňte příslušná pole.

1. Zadejte název do pole Název skupiny akce a zadejte název do pole krátkého názvu. Krátký název se používá namísto názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.

1. Autofills pole předplatné s vaším aktuálním předplatným. Toto předplatné je ten, ve kterém je akce skupinu uložit.
Vyberte skupinu prostředků, ve kterém je akce skupinu uložit.

V tomto příkladu vytvoříte dvě akce a akce runbook akci oznámení.

### <a name="runbook-action"></a>Akce sady Runbook

Následující kroky vytvoření sady runbook akce v rámci skupiny pro akce.

1. V části **akce**, zadejte název akce.

1. Vyberte **sady Automation Runbook** pro **typ akce**.

1. V části **podrobnosti** vybrat, **upravit podrobnosti**

1. Na **konfigurace Runbook** vyberte **uživatele** pod **zdroj Runbooku**.

1. Zvolte vaše **předplatné** a **účet Automation**a nakonec vyberte sadu runbook, které jste vytvořili v předchozím kroku názvem "Stop-AzureVmInResponsetoVMAlert".

1. Až budete hotoví, klikněte na tlačítko **OK**.

### <a name="notification-action"></a>Akce oznámení

Akci oznámení v rámci skupiny pro akce lze vytvořit následujícím postupem.

1. V části **akce**, zadejte název akce.

1. Vyberte **e-mailu** pro **typ akce**.

1. V části **podrobnosti** vybrat, **upravit podrobnosti**

1. Na **e-mailu** zadejte e-mailová adresa oznámení a klikněte na tlačítko **OK**. Přidání e-mailovou adresu a také runbook jako akce je užitečné, jako jsou oznámení při spuštění runbooku. Skupině akce by měl vypadat podobně jako na následujícím obrázku:

   ![Přidat stránku skupiny akce](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Klikněte na tlačítko **OK** vytvořit skupinu akce.

Ke skupině Akce vytvořen, můžete vytvořit [aktivity protokolu výstrahy](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) nebo [téměř v reálném čase výstrahy](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) a používat skupinu akce, kterou jste vytvořili.

## <a name="classic-alert"></a>Classic výstrahy

Classic výstrahy jsou založené na metriky a nechcete skupiny akcí, ale mají akce runbook na jejich základě. Chcete-li vytvořit classic výstrahu, použijte následující kroky:

1. Vyberte **+ přidat metriky upozornění**.

1. Název metriky upozornění "myVMCPUAlert" a zadejte stručný popis výstrahy.

1. Nastavení podmínky pro metriky výstrahu jako větší než, nastavte prahovou hodnotu jako "10" a jako "za posledních 5 minut, nastavit dobu.

1. V části **akce**, vyberte **spuštění sady runbook z této výstrahy**

1. Na **konfigurace Runbook** vyberte **uživatele** pro **zdroj Runbooku**. Vyberte svůj účet automation a klepněte **Stop-AzureVmInResponsetoVMAlert** sady runbook. Klikněte na tlačítko **OK**a potom klikněte na **OK** znovu a uložte pravidlo výstrahy.

## <a name="next-steps"></a>Další postup

* Další informace o spouštění runbooků služeb automatizace s webhooků, najdete v části [spuštění runbooku z webhook, jehož](automation-webhooks.md)
* Podrobnosti o různých způsobech spuštění sady runbook najdete v tématu [spuštění sady Runbook](automation-starting-a-runbook.md).
* Naučte se vytvořit výstrahu protokolu aktivit, najdete v tématu [vytvořit aktivitu protokolu výstrahy](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Chcete-li zjistit, jak vytvořit výstrahu pro near v reálném čase, navštivte [pomocí portálu Azure vytvořit pravidlo výstrahy](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).