---
title: "Ukázek Azure CLI 1.0 monitorování rychlý start. | Dokumentace Microsoftu"
description: "Vzorové příkazy rozhraní příkazového řádku 1.0 pro Azure monitorování funkce. Azure monitorování je služba Microsoft Azure, který umožňuje odeslat oznámení o výstrahách, volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data a škálování cloudové služby, virtuální počítače a webové aplikace."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: ec4512500dc3c77a40d2ebd1e6b460d5bb005811
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Ukázek Azure CLI monitorování napříč platformami 1.0 rychlý start
Tento článek ukazuje ukázku, že příkazy rozhraní příkazového řádku (CLI) můžete získat přístup k funkcím Azure monitorování. Azure monitorování umožňuje škálování cloudové služby, virtuální počítače a webových aplikací a odesílat oznámení o výstrahách nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.

> [!NOTE]
> Azure monitorování je nový název pro co byla volána "Statistika Azure" až 25 září 2016. Však obory názvů a proto níže uvedených příkazů stále obsahovat "insights".
> 
> 

## <a name="prerequisites"></a>Požadavky
Pokud jste ještě nenainstalovali Azure CLI, přečtěte si téma [nainstalovat Azure CLI](../cli-install-nodejs.md). Pokud jste obeznámeni s rozhraní příkazového řádku Azure, můžete přečíst další informace naleznete na [pomocí rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../xplat-cli-azure-resource-manager.md).

V systému Windows, nainstalujte npm z [Node.js webu](https://nodejs.org/). Po dokončení instalace pomocí CMD.exe s oprávněním spustit jako správce, spusťte ze složky, kde je nainstalován npm následující:

```console
npm install azure-cli --global
```

Potom přejděte na všechny složky nebo umístění chcete a zadejte v příkazovém řádku:

```console
azure help
```

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Prvním krokem je přihlášení k účtu Azure.

```console
azure login
```

Po spuštění tohoto příkazu, budete muset přihlásit pomocí pokynů na obrazovce. Potom zobrazí váš účet, TenantId a výchozí ID předplatného. Všechny příkazy fungovat v rámci vašeho předplatného výchozí.

K zobrazení seznamu podrobnosti vaším aktuálním předplatným, použijte následující příkaz.

```console
azure account show
```

Chcete-li změnit pracovní kontext do jiného předplatného, použijte následující příkaz.

```console
azure account set "subscription ID or subscription name"
```

Pokud chcete používat příkazy Azure Resource Manager a monitorování Azure, musíte být v režimu Azure Resource Manager.

```console
azure config mode arm
```

Chcete-li zobrazit seznam všech podporovaných příkazů monitorování Azure, postupujte takto.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Zobrazit protokol aktivity pro předplatné
Chcete-li zobrazit seznam aktivity protokolu události, postupujte takto.

```console
azure insights logs list [options]
```

Zkuste následující příkaz a zobrazí všechny dostupné možnosti.

```console
azure insights logs list -help
```

Tady je příklad do seznamu protokolů podle resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Příklad do seznamu protokolů volající

```console
azure insights logs list --caller "myname@company.com"
```

Příklad seznamu protokolů volající na typ prostředku v rámci počáteční a koncové datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Práce s výstrahami
Informace v části slouží k práci s výstrahami.

### <a name="get-alert-rules-in-a-resource-group"></a>Získat pravidla výstrah ve skupině prostředků
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Vytvoření metriky pravidlo výstrahy
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Vytvořit pravidlo výstrahy testu webu
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Odstranit pravidlo výstrahy
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profily protokolu
Použijte informace v této části pro práci s profily protokolu.

### <a name="get-a-log-profile"></a>Získat profil protokolu
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Přidat profil protokolu bez uchování
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Přidat profil protokolu s dobou uchování
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Přidat profil protokolu s uchovávání a EventHub
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostika
Použijte informace v této části pro práci s nastavení diagnostiky.

### <a name="get-a-diagnostic-setting"></a>Získat nastavení diagnostiky
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Zakažte nastavení diagnostiky
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Povolit nastavení diagnostiky bez uchování
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatické škálování
Použijte informace v této části pro práci s nastavení automatického škálování. Budete muset upravit tyto příklady.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Získat nastavení automatického škálování pro skupinu prostředků.
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Získat nastavení automatického škálování podle názvu ve skupině prostředků
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Nastavení auotoscale
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
