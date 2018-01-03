---
title: "Akce a NotActions – řízení přístupu Azure na základě rolí (RBAC) | Microsoft Docs"
description: "Toto téma popisuje předdefinovaných do rolí pro řízení přístupu na základě role (RBAC). Role jsou neustále přidali, proto zkontrolujte aktuálnosti dokumentaci."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
editor: 
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/28/2017
ms.author: andredm
ms.reviewer: 
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e7c563547f04a16a1059ed709d9ded25d60792f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Předdefinované role pro řízení přístupu Azure na základě rolí
Azure na základě rolí řízení přístupu (RBAC) obsahuje následující předdefinované role, které lze přiřadit uživatelům, skupinám a službám. Definice předdefinované role se nedá změnit. Můžete však vytvořit [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md) podle konkrétních potřeb vaší organizace.

## <a name="roles-in-azure"></a>Role v Azure
Následující tabulka obsahuje stručný popis předdefinovaných rolí. Klikněte na název role, které chcete zobrazit podrobný seznam **akce** a **notactions** pro roli. **Akce** vlastnost určuje povolené akce u prostředků Azure. Řetězce akce můžete použít zástupné znaky. **Notactions** vlastnost určuje akce, které jsou vyloučeny z povolených akcí.

Akce definuje, jaký typ operace můžete provádět na typ daného prostředku. Příklad:
- **Zápis** umožňuje provádět operace PUT, POST, PATCH a DELETE.
- **Čtení** umožňuje provádět operace GET.

V tomto článku pouze adresy různé role, které existují ještě dnes. Když přiřadíte roli pro uživatele, ale můžete omezit povolených akcí další definováním obor. To je užitečné, pokud chcete, aby někdo webu Přispěvatel, ale jenom u jedné skupiny prostředků.

> [!NOTE]
> Definice Azure role se neustále vyvíjejí. Tento článek se ukládají jako aktuální nejblíže, ale vždy najdete nejnovější definice rolí v prostředí Azure PowerShell. Použití [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) rutiny seznamu všechny aktuální role. Můžete podrobné informace konkrétní roli pomocí `(get-azurermroledefinition "<role name>").actions` nebo `(get-azurermroledefinition "<role name>").notactions` podle. Použití [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) k operacím seznamu zprostředkovatelů konkrétních prostředků Azure.


| Název role | Popis |
| --- | --- |
| [Rozhraní API správy služby přispěvatele](#api-management-service-contributor) |Můžete spravovat služba API Management a rozhraní API |
| [Role operátora služby API Management](#api-management-service-operator-role) | Můžete spravovat služba API Management, ale není sami rozhraní API |
| [Role čtenáře služby API Management](#api-management-service-reader-role) | Přístup jen pro čtení k služba API Management a rozhraní API |
| [Application Insights součást Přispěvatel](#application-insights-component-contributor) |Může spravovat součásti Application Insights. |
| [Operátor automatizace](#automation-operator) |Možnost spuštění, zastavení, pozastavení a obnovení úlohy |
| [Zálohování přispěvatele](#backup-contributor) | Můžete spravovat zálohy v trezoru služeb zotavení |
| [Operátor zálohování](#backup-operator) | Můžete spravovat zálohování, vyjma případu odebírání zálohování, v trezoru služeb zotavení |
| [Zálohování čtečky](#backup-reader) | Můžete zobrazit všechny služby zálohování správy  |
| [Čtečka fakturace](#billing-reader) | Můžete zobrazit všechny fakturační informace  |
| [BizTalk přispěvatele](#biztalk-contributor) |Můžete spravovat služby BizTalk services |
| [Databáze MySQL cleardb – DB přispěvatele](#cleardb-mysql-db-contributor) |Můžete spravovat databáze MySQL cleardb – |
| [Přispěvatel](#contributor) |Můžou spravovat všechno kromě přístupu. |
| [Přispěvatel objekt pro vytváření dat](#data-factory-contributor) |Můžete vytvořit a spravovat datové továrny a podřízené prostředky v nich. |
| [Uživatel DevTest Labs](#devtest-labs-user) |Můžete zobrazit vše, co a připojit, spuštění, restart a vypnutí virtuálního počítače |
| [Přispěvatel zóny DNS](#dns-zone-contributor) |Můžete spravovat zóny DNS a záznamy |
| [Přispěvatel účtu DocumentDB](#documentdb-account-contributor) |Můžete spravovat účty pro Azure Cosmos DB |
| [Přispěvatel účet inteligentních systémů](#intelligent-systems-account-contributor) |Můžete spravovat účty inteligentních systémů |
| Přispěvatel aplikace logiky | Můžete spravovat všechny aspekty aplikace logiky, ale není vytvořte novou. |
| Operátor aplikace logiky |Můžete spustit a zastavit pracovní postupy definované v rámci aplikace logiky. |
| [Monitorování čtečky](#monitoring-reader) |Může číst veškerá data monitorování |
| [Monitorování přispěvatele](#monitoring-contributor) |Můžete číst data monitorování a upravit nastavení monitorování |
| [Přispěvatel sítě](#network-contributor) |Můžete spravovat všechny síťové prostředky |
| [Nový přispěvatel účet New Relic APM](#new-relic-apm-account-contributor) |Můžete spravovat novou Správa výkonu aplikací New Relic účty a aplikace |
| [Vlastník](#owner) |Můžou spravovat všechno včetně přístupu |
| [Čtecí modul](#reader) |Můžou zobrazit všechno, ale nelze provádět změny |
| [Přispěvatel mezipaměti redis](#redis-cache-contributor) |Můžete spravovat mezipaměti Redis |
| [Přispěvatel kolekce úloh plánovače](#scheduler-job-collections-contributor) |Můžete spravovat kolekce úloh plánovače |
| [Přispěvatel služby vyhledávání](#search-service-contributor) |Můžete spravovat služby vyhledávání |
| [Správce zabezpečení](#security-manager) |Můžete spravovat součásti zabezpečení, zásady zabezpečení a virtuálních počítačů |
| [Přispěvatel obnovení lokality](#site-recovery-contributor) | Site Recovery můžete spravovat v trezoru služeb zotavení |
| [Operátor obnovení lokality](#site-recovery-operator) | Můžete spravovat převzetí služeb při selhání a navrácení služeb po obnovení operace obnovení lokality v trezoru služeb zotavení |
| [Čtečka obnovení lokality](#site-recovery-reader) | Můžete zobrazit všechny operace správy Site Recovery  |
| [Přispěvatel databází SQL](#sql-db-contributor) |Můžete spravovat databáze SQL, ale není jejich zásady zabezpečení |
| [Správce zabezpečení SQL](#sql-security-manager) |Můžete spravovat zásady vztahující se k zabezpečení serverů SQL a databáze |
| [Přispěvatel serveru SQL](#sql-server-contributor) |Můžete spravovat servery SQL Server a databáze, ale není jejich zásady zabezpečení |
| [Přispěvatel účet úložiště Classic](#classic-storage-account-contributor) |Můžete spravovat klasických účtů úložiště |
| [Přispěvatel účtu úložiště](#storage-account-contributor) |Můžete spravovat účty úložiště |
| [Přispěvatel žádosti o podporu](#support-request-contributor) | Můžete vytvořit a spravovat žádosti o podporu |
| [Správce přístupu uživatelů](#user-access-administrator) |Můžete spravovat přístup uživatelů k prostředkům Azure |
| [Přispěvatel Classic virtuálních počítačů](#classic-virtual-machine-contributor) |Můžete spravovat klasické virtuální počítače, ale není virtuální sítě nebo úložiště účet ke kterému jsou připojeny |
| [Přispěvatel virtuálních počítačů](#virtual-machine-contributor) |Můžete spravovat virtuální počítače, ale není virtuální sítě nebo úložiště účet ke kterému jsou připojeny |
| [Přispěvatel klasických sítí](#classic-network-contributor) |Můžete spravovat klasické virtuální sítě a vyhrazené IP adresy |
| [Plán přispěvatelů webu](#web-plan-contributor) |Můžete spravovat plány web |
| [Přispěvatel webu](#website-contributor) |Můžete spravovat weby, ale není webové plány, na které jsou připojeny |

## <a name="role-permissions"></a>Oprávnění role
Následující tabulky popisují konkrétní oprávnění na každou roli. To může zahrnovat **akce**, který udělit oprávnění, a **NotActions**, což je omezit.

### <a name="api-management-service-contributor"></a>Přispěvatel služby API Management
Můžete spravovat služby API Management

| **Akce** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/* |Vytvářet a spravovat služby API Management |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Čtení rolí a přiřazení rolí |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="api-management-service-operator-role"></a>Role Operátor služby API Management
Můžete spravovat služby API Management

| **Akce** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Instance služby API Management pro čtení |
| Microsoft.ApiManagement/Service/backup/action | Zálohování služby API Management do zadaného kontejneru. v uživatel zadaný účet úložiště |
| Microsoft.ApiManagement/Service/delete | Odstranění instance služby API Management |
| Microsoft.ApiManagement/Service/managedeployments/action | Změna SKU nebo jednotky; Přidat nebo odebrat místní nasazení služby API Management |
| Microsoft.ApiManagement/Service/read | Číst metadata pro instanci služby API Management |
| Microsoft.ApiManagement/Service/restore/action | Obnovení ze zadaného kontejneru v uživatel zadaný účet úložiště služby API Management |
| Microsoft.ApiManagement/Service/updatehostname/action | Nastavit, aktualizovat nebo odebrat vlastní názvy domén pro služby API Management |
| Microsoft.ApiManagement/Service/write | Vytvoření nové instance služby API Management |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Čtení rolí a přiřazení rolí |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="api-management-service-reader-role"></a>Role Čtenář služby API Management
Můžete spravovat služby API Management

| **Akce** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Instance služby API Management pro čtení |
| Microsoft.ApiManagement/Service/read | Číst metadata pro instanci služby API Management |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Čtení rolí a přiřazení rolí |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="application-insights-component-contributor"></a>Přispěvatel součástí Application Insights
Může spravovat součásti Application Insights.

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.Insights/components/* |Vytvoření a správa přehled komponenty |
| Microsoft.Insights/webtests/* |Vytvoření a Správa webové testy |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="automation-operator"></a>Operátor služby Automation
Možnost spuštění, zastavení, pozastavení a obnovení úlohy

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.Automation/automationAccounts/jobs/read |Číst úlohy účet automation. |
| Microsoft.Automation/automationAccounts/jobs/resume/action |Obnovit úlohu účet služby automation. |
| Microsoft.Automation/automationAccounts/jobs/stop/action |Zastavit úlohu účet služby automation. |
| Microsoft.Automation/automationAccounts/jobs/streams/read |Číst datové proudy úlohy účet automation. |
| Microsoft.Automation/automationAccounts/jobs/suspend/action |Pozastavit úlohu účet služby automation. |
| Microsoft.Automation/automationAccounts/jobs/write |Zápis úloh účtu automation |
| Microsoft.Automation/automationAccounts/jobSchedules/read |Číst plán úloh účtu automation |
| Microsoft.Automation/automationAccounts/jobSchedules/write |Číst plán úloh účtu automation |
| Microsoft.Automation/automationAccounts/read |Účty automation pro čtení |
| Microsoft.Automation/automationAccounts/runbooks/read |Runbooky služby automatizace pro čtení |
| Microsoft.Automation/automationAccounts/schedules/read |Číst plány účet automation. |
| Microsoft.Automation/automationAccounts/schedules/write |Zápis plány účet automation. |
| Microsoft.Insights/components/* |Vytvoření a správa přehled komponenty |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="backup-contributor"></a>Přispěvatel zálohování
Můžete spravovat všechny akce správy zálohování, s výjimkou vytvoření trezoru služeb zotavení a udělení přístupu ke ostatními

| **Akce** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Spravovat výsledky operace na správu záloh |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Vytvářet a spravovat zálohy kontejnery uvnitř zálohování Fabric trezor služeb zotavení |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Vytvářet a spravovat úlohy zálohování |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Úlohy zálohování export do aplikace excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Vytvářet a spravovat metadata související se správou zálohování |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Vytvořit a spravovat výsledky operací zálohování správy |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Vytvářet a spravovat zásady zálohování |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Vytváření a Správa položek, které lze zálohovat |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Vytváření a správa zálohovaných položek |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Vytvoření a Správa kontejnerů, která uchovává položky zálohování |
| Microsoft.RecoveryServices/Vaults/certificates/* | Vytváření a správě certifikátů související s zálohování v trezoru služeb zotavení |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Vytváření a správě rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/read | Číst trezory služeb zotavení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Spravovat operaci zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Vytvářet a spravovat registrovaná identit |
| Microsoft.RecoveryServices/Vaults/usages/* | Vytvářet a spravovat využití trezor služeb zotavení |
| Microsoft.Resources/deployments/* | Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/read | Účty úložiště pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="backup-operator"></a>Operátor zálohování
Můžete spravovat všechny akce správy zálohování kromě vytváření trezory, odebrání zálohování a uvádějící přístup ostatním uživatelům

| **Akce** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Přečtěte si výsledky operace na správu záloh |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Přečtěte si výsledky operace kontejnerům ochrany |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Provedení operace zálohování na vyžádání na zálohované položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Čtení výsledek operace provedené na zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read | Načíst stav operace provedené na zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Čtení zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Přečtěte si bod obnovení zálohovaných položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Provedení operace obnovení pomocí bodu obnovení zálohovaných položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Vytvořit položku Zálohování |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Načetla kontejnery, která uchovává zálohování položek |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Vytvářet a spravovat úlohy zálohování |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Úlohy zálohování export do aplikace excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Číst metadata související se správou zálohování |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Vytvořit a spravovat výsledky operací zálohování správy |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Výsledky operace provedené na zásady zálohování pro čtení |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Zásady zálohování pro čtení |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Vytváření a Správa položek, které lze zálohovat |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Čtení zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Čtení zálohovat kontejnery, která uchovává položky zálohování |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Číst rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Rozšířené informace o související s trezoru |
| Microsoft.RecoveryServices/Vaults/read | Číst trezory služeb zotavení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Spravovat operaci zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Čtení výsledky operace provedené na registrovaná položky trezoru |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Číst registrované položky trezoru |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Zápis registrované položky do trezoru |
| Microsoft.RecoveryServices/Vaults/usages/read | Přečtěte si využití trezor služeb zotavení |
| Microsoft.Resources/deployments/* | Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/read | Účty úložiště pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat lístky žádostí o podporu |

### <a name="backup-reader"></a>Čtenář záloh
Můžete monitorovat správu záloh v trezoru služeb zotavení

| **Akce** | |
| --- | --- |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read  | Přečtěte si výsledky operace na správu záloh |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read  | Přečtěte si výsledky operace kontejnerům ochrany |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read  | Čtení výsledek operace provedené na zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read  | Načíst stav operace provedené na zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read  | Čtení zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read  | Načetla kontejnery, která uchovává zálohování položek |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read  | Přečtěte si výsledky úloh zálohování |
| Microsoft.RecoveryServices/Vaults/backupJobs/read  | Úlohy zálohování pro čtení |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Úlohy zálohování export do aplikace excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read  | Číst metadata související se správou zálohování |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read  | Přečtěte si správu záloh výsledky operace |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read  | Výsledky operace provedené na zásady zálohování pro čtení |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read  | Zásady zálohování pro čtení |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read  |  Čtení zálohovat položky |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read  | Čtení zálohovat kontejnery, která uchovává položky zálohování |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Číst rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/read  | Číst trezory služeb zotavení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Načíst výsledek operace zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | Čtení výsledky operace provedené na registrovaná položky trezoru |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Číst registrované položky trezoru |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Přečtěte si využití trezor služeb zotavení |

### <a name="billing-reader"></a>Čtenář fakturace
Můžete zobrazit všechny informace o fakturaci

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.Billing/*/read |Číst informace o fakturaci |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="biztalk-contributor"></a>Přispěvatel BizTalk
Můžete spravovat služby BizTalk services

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.BizTalkServices/BizTalk/* |Vytvářet a spravovat služby BizTalk services |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="cleardb-mysql-db-contributor"></a>Přispěvatel databází ClearDB MySQL
Můžete spravovat databáze MySQL cleardb –

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |
| successbricks.cleardb/Databases/* |Vytvářet a spravovat databáze MySQL cleardb – |

### <a name="contributor"></a>Přispěvatel
Můžou spravovat všechno kromě přístupu

| **Akce** |  |
| --- | --- |
| * |Vytvořit a spravovat prostředky všech typů |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete |Nelze odstranit role a přiřazení rolí |
| Microsoft.Authorization/*/Write |Nelze vytvořit role a přiřazení rolí |

### <a name="data-factory-contributor"></a>Přispěvatel Data Factory
Vytvořit a spravovat datové továrny a podřízené prostředky v nich.

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.DataFactory/dataFactories/* |Vytvořit a spravovat datové továrny a podřízené prostředky v nich. |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="devtest-labs-user"></a>Uživatel služby DevTest Labs
Můžete zobrazit vše, co a připojit, spuštění, restart a vypnutí virtuálního počítače

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Compute/availabilitySets/read |Číst vlastnosti služby skupiny dostupnosti |
| Microsoft.Compute/virtualMachines/*/read |Číst vlastnosti virtuálního počítače (velikosti virtuálních počítačů, stav modulu runtime, rozšíření virtuálního počítače, atd.) |
| Microsoft.Compute/virtualMachines/deallocate/action |Deallocate – virtuální počítače |
| Microsoft.Compute/virtualMachines/read |Číst vlastnosti virtuálního počítače |
| Microsoft.Compute/virtualMachines/restart/action |Restartovat virtuální počítače |
| Microsoft.Compute/virtualMachines/start/action |Spuštění virtuálních počítačů |
| Microsoft.DevTestLab/*/read |Číst vlastnosti služby testovacího prostředí |
| Microsoft.DevTestLab/labs/createEnvironment/action |Vytvoření testovacího prostředí |
| Microsoft.DevTestLab/labs/formulas/delete |Odstranit vzorce |
| Microsoft.DevTestLab/labs/formulas/read |Vzorce pro čtení |
| Microsoft.DevTestLab/labs/formulas/write |Přidat nebo upravit vzorce |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action |Vyhodnocení zásad testovacího prostředí |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Připojení k fondu back-end adresy Vyrovnávání zatížení. |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Připojení k službě Vyrovnávání zatížení příchozí pravidlo NAT |
| Microsoft.Network/networkInterfaces/*/read |Při čtení vlastností rozhraní sítě (například všechny služby Vyrovnávání zatížení, které síťové rozhraní je součástí) |
| Microsoft.Network/networkInterfaces/join/action |Připojit virtuální počítač k síťovému rozhraní |
| Microsoft.Network/networkInterfaces/read |Síťová rozhraní pro čtení |
| Microsoft.Network/networkInterfaces/write |Zápis síťových rozhraní |
| Microsoft.Network/publicIPAddresses/*/read |Číst vlastnosti služby veřejnou IP adresu |
| Microsoft.Network/publicIPAddresses/join/action |Spojit veřejnou IP adresu |
| Microsoft.Network/publicIPAddresses/read |Veřejné IP adresy sítě pro čtení |
| Microsoft.Network/virtualNetworks/subnets/join/action |Připojení k virtuální síti |
| Microsoft.Resources/deployments/operations/read |Operace čtení nasazení |
| Microsoft.Resources/deployments/read |Nasazení pro čtení |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/listKeys/action |Vypsat klíče účtu úložiště |

### <a name="dns-zone-contributor"></a>Přispěvatel zóny DNS
Můžete spravovat zóny DNS a záznamy.

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/ \* /číst |Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/\* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.Network/dnsZones/\* |Vytváření a Správa zón DNS a záznamů |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/\* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/\* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="documentdb-account-contributor"></a>Přispěvatel účtů DocumentDB
Ke správě účtů Azure Cosmos DB. Azure Cosmos DB je dříve označované jako DocumentDB.

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.DocumentDb/databaseAccounts/* |Vytvořit a spravovat účty pro Azure Cosmos DB |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="intelligent-systems-account-contributor"></a>Přispěvatel účtů Inteligentních systémů
Můžete spravovat účty inteligentních systémů

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.IntelligentSystems/accounts/* |Vytváření a správě účtů inteligentních systémů |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="monitoring-reader"></a>Čtenář monitorování
Může číst veškerá monitorování data (metriky, protokoly, atd.). Viz také [začít pracovat s rolí, oprávnění a zabezpečení pomocí Azure monitorování](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Akce** |  |
| --- | --- |
| * / Číst |Přečtěte si prostředky všech typů, s výjimkou tajných klíčů. |
| Microsoft.OperationalInsights/workspaces/search/action |Vyhledávání dat analýzy protokolů |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="monitoring-contributor"></a>Přispěvatel monitorování
Můžete načíst všechna data monitorování a upravit nastavení monitorování. Viz také [začít pracovat s rolí, oprávnění a zabezpečení pomocí Azure monitorování](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Akce** |  |
| --- | --- |
| * / Číst |Přečtěte si prostředky všech typů, s výjimkou tajných klíčů. |
| Microsoft.Insights/AlertRules/* |Pravidla výstrah pro čtení, zápisu a odstranění. |
| Microsoft.Insights/components/* |Součásti Application Insights pro čtení, zápisu a odstranění. |
| Microsoft.Insights/DiagnosticSettings/* |Nastavení diagnostiky pro čtení, zápisu a odstranění. |
| Microsoft.Insights/eventtypes/* |Zobrazí seznam aktivity protokolu události (události management) v předplatném. Toto oprávnění se vztahuje na portálu i programový přístup k protokolu aktivit. |
| Microsoft.Insights/LogDefinitions/* |Tato oprávnění jsou nezbytné pro uživatele, kteří potřebují přístup k protokolům aktivity prostřednictvím portálu. Seznam kategorií protokolu v protokolu aktivit. |
| Microsoft.Insights/MetricDefinitions/* |Číst definice metrik (seznamu dostupných typů metriky pro prostředek). |
| Microsoft.Insights/Metrics/* |Číst metriky pro prostředek. |
| Microsoft.Insights/Register/Action |Zaregistrujte zprostředkovatele Microsoft.Insights. |
| Microsoft.Insights/webtests/* |Application Insights pro čtení, zápisu a odstranění webové testy. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* |Balíčky řešení analýzy protokolů pro čtení, zápisu a odstranění. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* |Analýzy protokolů pro čtení, zápisu a odstraňování uložená hledání. |
| Microsoft.OperationalInsights/workspaces/search/action |Hledání pracovních prostorů analýzy protokolů. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action |Seznam klíčů pro pracovní prostor analýzy protokolů. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* |Konfigurace přehledy úložiště analýzy protokolů pro čtení, zápisu a odstranění. |

### <a name="network-contributor"></a>Přispěvatel sítě
Můžete spravovat všechny síťové prostředky

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.Network/* |Vytvoření a Správa sítě |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="new-relic-apm-account-contributor"></a>Přispěvatel účtů New Relic APM
Můžete spravovat novou Správa výkonu aplikací New Relic účty a aplikace

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |
| NewRelic.APM/accounts/* |Vytvářet a spravovat účty pro správu výkonu aplikací New Relic. |

### <a name="owner"></a>Vlastník
Můžou spravovat všechno včetně přístupu

| **Akce** |  |
| --- | --- |
| * |Vytvořit a spravovat prostředky všech typů |

### <a name="reader"></a>Čtenář
Můžou zobrazit všechno, ale nelze provádět změny

| **Akce** |  |
| --- | --- |
| * / Číst |Přečtěte si prostředky všech typů, s výjimkou tajných klíčů. |

### <a name="redis-cache-contributor"></a>Přispěvatel služby Redis Cache
Můžete spravovat mezipaměti Redis

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Cache/redis/* |Vytváření a správě mezipaměti Redis |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="scheduler-job-collections-contributor"></a>Přispěvatel kolekcí úloh Scheduleru
Můžete spravovat kolekce úloh plánovače

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Scheduler/jobcollections/* |Vytvoření a správa kolekcí úloh |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="search-service-contributor"></a>Přispěvatel služby Search
Můžete spravovat služby vyhledávání

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Search/searchServices/* |Vytvoření a Správa služby vyhledávání |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="security-manager"></a>Správce zabezpečení
Můžete spravovat součásti zabezpečení, zásady zabezpečení a virtuálních počítačů

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.ClassicCompute/*/read |Přečtěte si informace o konfiguraci klasické výpočetní virtuální počítače |
| Microsoft.ClassicCompute/virtualMachines/*/write |Zapsat konfiguraci pro virtuální počítače |
| Microsoft.ClassicNetwork/*/read |Přečtěte si informace o konfiguraci o classic sítě |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Security/* |Vytvořit a spravovat zabezpečení komponenty a zásady |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="site-recovery-contributor"></a>Přispěvatel Site Recovery
Můžete spravovat všechny akce správy Site Recovery, s výjimkou vytvoření trezoru služeb zotavení a přiřazení přístupová práva jiným uživatelům

| **Akce** | |
| --- | --- |
| Microsoft.Authorization/*/read | Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/* | Vytvářet a spravovat pravidla výstrah |
| Microsoft.Network/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.RecoveryServices/Vaults/certificates/write | Aktualizace certifikát přihlašovacích údajů trezoru |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Vytváření a správě rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Načtení výstrah pro trezor služeb zotavení |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/čtení  | Konfigurace oznámení trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/read | Trezory služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Spravovat operaci zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Vytvářet a spravovat registrovaná identit |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Vytvořit nebo aktualizovat nastavení replikace výstrahy |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Číst události replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Vytvoření a Správa prostředků infrastruktury replikace |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Vytvářet a spravovat úlohy replikace |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Vytvoření a správě zásad replikace |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Vytváření a správě plánů obnovení |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Vytvoření a správě konfigurace úložiště z trezoru služeb zotavení |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Informace o tokenu trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/usages/read | Přečtěte si podrobnosti o využití trezoru služeb zotavení |
| Microsoft.ResourceHealth/availabilityStatuses/read | Načíst stav prostředků |
| Microsoft.Resources/deployments/* | Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/read | Účty úložiště pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="site-recovery-operator"></a>Operátor Site Recovery
Můžete převzetí služeb při selhání a navrácení služeb po obnovení, ale nemůže provádět další akce správy Site Recovery nebo přiřadit přístup ostatním uživatelům

| **Akce** | |
| --- | --- |
| Microsoft.Authorization/*/read | Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/* | Vytvářet a spravovat pravidla výstrah |
| Microsoft.Network/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Číst rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Načtení výstrah pro trezor služeb zotavení |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/čtení  | Konfigurace oznámení trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/read | Trezory služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Spravovat operaci zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Načíst stav operace a výsledek odeslaná operace |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kontejnery pro čtení zaregistrovat pro prostředek |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Čtení výstrah nastavení replikace |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Číst události replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Kontrola konzistence prostředků úložiště |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Prostředků infrastruktury replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ reassociateGateway nebo akce | Znovu přidružte replikační brána |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Obnovit certifikát prostředků infrastruktury replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Přečtěte si replikace sítě prostředků infrastruktury |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/čtení | Mapování sítě prostředků infrastruktury replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/čtení | Kontejnery ochrany pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/čtení | Získání seznamu všech chránitelné položek |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint nebo akce | Použít konkrétní bod obnovení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit nebo akce | Potvrzení převzetí služeb při selhání pro nezdařené přes položky |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover nebo akce | Spusťte plánované převzetí služeb při selhání pro chráněné položky |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/čtení | Získání seznamu všech chráněných položek |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/čtení | Získání seznamu dostupných bodů obnovení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication nebo akce | Oprava replikace pro chráněnou položku |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems nebo opětovné ochrany nebo akce | Spustit znovu nastavit ochranu pro chráněnou položku|
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover nebo akce | Spustit test převzetí služeb při selhání chráněné položky |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup nebo akce | Spustit čištění testovací převzetí služeb |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover nebo akce | Spustit neplánované převzetí služeb při selhání chráněné položky |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService nebo akce | Aktualizace služby mobility |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/čtení | Číst mapování kontejnerů ochrany |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/čtení | Poskytovatelé služeb zotavení pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider nebo akce | Aktualizujte zprostředkovatele služeb zotavení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/čtení | Klasifikace úložiště pro čtení pro prostředků infrastruktury replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/čtení | Přečtěte si mapování klasifikace úložiště |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Zaregistrovat informace k vCenter pro čtení |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Vytvářet a spravovat úlohy replikace |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Zásady replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ failoverCommit nebo akce | Potvrzení převzetí služeb při selhání obnovení plán převzetí služeb při selhání |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ plannedFailover nebo akce | Spustit převzetí služeb při selhání plánu obnovení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Plány obnovení pro čtení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Spustit znovu nastavit ochranu plánu obnovení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Spustit test převzetí služeb při selhání plánu obnovení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ testFailoverCleanup nebo akce | Spustit čištění obnovení plán testovací převzetí služeb při selhání |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ unplannedFailover nebo akce | Spustit neplánované převzetí služeb při selhání plánu obnovení |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Číst konfiguraci úložiště trezoru služeb zotavení |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Informace o tokenu trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/usages/read | Přečtěte si podrobnosti o využití trezoru služeb zotavení |
| Microsoft.ResourceHealth/availabilityStatuses/read | Načíst stav prostředků |
| Microsoft.Resources/deployments/* | Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/read | Účty úložiště pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat lístky žádostí o podporu |

### <a name="site-recovery-reader"></a>Čtenář Site Recovery
Můžete sledovat stav obnovení lokality v trezoru služeb zotavení a zvýšit lístky žádostí o podporu

| **Akce** | |
| --- | --- |
| Microsoft.Authorization/*/read | Čtení rolí a přiřazení rolí |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Číst rozšířené informace související s trezoru |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read  | Načtení výstrah pro trezor služeb zotavení |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/čtení  | Konfigurace oznámení trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/read  | Trezory služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Spravovat operaci zjišťování pro načítání nově vytvořený kontejnery |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | Načíst stav operace a výsledek odeslaná operace |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Kontejnery pro čtení zaregistrovat pro prostředek |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Čtení výstrah nastavení replikace |
| Microsoft.RecoveryServices/vaults/replicationEvents/read  | Číst události replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read  | Prostředků infrastruktury replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read  | Přečtěte si replikace sítě prostředků infrastruktury |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/čtení  | Mapování sítě prostředků infrastruktury replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/čtení  |  Kontejnery ochrany pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/čtení  | Získání seznamu všech chránitelné položek |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/čtení  | Získání seznamu všech chráněných položek |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/čtení  | Získání seznamu dostupných bodů obnovení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/čtení  | Číst mapování kontejnerů ochrany |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/čtení  | Poskytovatelé služeb zotavení pro čtení |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/čtení  | Klasifikace úložiště pro čtení pro prostředků infrastruktury replikace |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/čtení  |  Přečtěte si mapování klasifikace úložiště |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read  |  Zaregistrovat informace k vCenter pro čtení |
| Microsoft.RecoveryServices/vaults/replicationJobs/read  |  Načíst stav replikace úloh |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read  |  Zásady replikace pro čtení |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read  |  Plány obnovení pro čtení |
| Microsoft.RecoveryServices/Vaults/storageConfig/read  |  Číst konfiguraci úložiště trezoru služeb zotavení |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read  |  Informace o tokenu trezoru služeb zotavení pro čtení |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Přečtěte si podrobnosti o využití trezoru služeb zotavení |
| Microsoft.Support/*  |  Vytvářet a spravovat lístky žádostí o podporu |

### <a name="sql-db-contributor"></a>Přispěvatel databází SQL
Můžete spravovat databáze SQL, ale není jejich zásady zabezpečení

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení role a role přiřazení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/databases/* |Vytvářet a spravovat databáze SQL |
| Microsoft.Sql/servers/read |Servery SQL pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Nelze upravit zásady auditu |
| Microsoft.Sql/servers/databases/auditingSettings/* |Nelze upravit nastavení auditu |
| Microsoft.Sql/servers/databases/auditRecords/read |Nelze přečíst záznamů auditu |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Nelze upravit zásady připojení |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Nelze upravit zásady maskování dat. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Nelze upravovat zásady, výstrah zabezpečení |
| Microsoft.Sql/servers/databases/securityMetrics/* |Nelze upravit zabezpečení metriky |

### <a name="sql-security-manager"></a>Správce zabezpečení SQL
Můžete spravovat zásady vztahující se k zabezpečení serverů SQL a databáze

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace Microsoft pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/auditingPolicies/* |Vytvořit a spravovat zásady auditování SQL serveru |
| Microsoft.Sql/servers/auditingSettings/* |Vytvářet a spravovat nastavení auditování serveru SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Vytvářet a spravovat zásady auditu databáze serveru SQL |
| Microsoft.Sql/servers/databases/auditingSettings/* |Vytvářet a spravovat nastavení auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditRecords/read |Záznamy auditu pro čtení |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Vytvářet a spravovat zásady připojení databáze serveru SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Vytvářet a spravovat zásady maskování dat databáze serveru SQL |
| Microsoft.Sql/servers/databases/read |Databáze SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/read |Schémata databáze serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read |Sloupce tabulky databáze serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/tables/read |Tabulky databáze serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Vytvoření a Správa výstrah zásady zabezpečení serveru SQL server databáze |
| Microsoft.Sql/servers/databases/securityMetrics/* |Vytvoření a správa metriky zabezpečení databáze serveru SQL |
| Microsoft.Sql/servers/read |Servery SQL pro čtení |
| Microsoft.Sql/servers/securityAlertPolicies/* |Vytvoření a Správa výstrah zásady zabezpečení SQL serveru |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="sql-server-contributor"></a>Přispěvatel SQL Serveru
Můžete spravovat servery SQL Server a databáze, ale není jejich zásady zabezpečení

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Čtení rolí a přiřazení rolí |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/* |Vytvoření a správa serverů SQL |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* |Nelze upravit zásady auditu SQL serveru |
| Microsoft.Sql/servers/auditingSettings/* |Nastavení auditování serveru SQL server nelze upravit. |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Nelze upravit zásady auditu databáze serveru SQL |
| Microsoft.Sql/servers/databases/auditingSettings/* |Nelze upravit nastavení auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditRecords/read |Nelze přečíst záznamů auditu |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Nelze upravit zásady připojení databáze serveru SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Nelze upravit maskování zásady data v databázi SQL serveru |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Nelze upravit výstrahy zásady zabezpečení serveru SQL server databáze |
| Microsoft.Sql/servers/databases/securityMetrics/* |Nelze upravit metriky zabezpečení databáze serveru SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* |Nelze upravit výstrahy zásady zabezpečení SQL serveru |

### <a name="classic-storage-account-contributor"></a>Přispěvatel klasických účtů úložiště
Můžete spravovat klasických účtů úložiště

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.ClassicStorage/storageAccounts/* |Vytváření a správě účtů úložiště |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="storage-account-contributor"></a>Přispěvatel účtů úložiště
Můžete spravovat účty úložiště, ale ne přístup k nim.

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Číst všechny autorizace |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.Insights/diagnosticSettings/* |Spravovat nastavení pro diagnostiku |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/* |Vytváření a správě účtů úložiště |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="support-request-contributor"></a>Přispěvatel žádostí o podporu
Můžete vytvořit a spravovat lístky žádostí o podporu na obor předplatného

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Autorizace pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat lístky žádostí o podporu |
| Microsoft.Resources/subscriptions/resourceGroups/read | Čtení rolí a přiřazení rolí |

### <a name="user-access-administrator"></a>Správce přístupu uživatelů
Můžete spravovat přístup uživatelů k prostředkům Azure

| **Akce** |  |
| --- | --- |
| * / Číst |Přečtěte si prostředky všech typů, s výjimkou tajných klíčů. |
| Microsoft.Authorization/* |Spravovat autorizace |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="classic-virtual-machine-contributor"></a>Přispěvatel klasických virtuálních počítačů
Můžete spravovat klasické virtuální počítače, ale není virtuální sítě nebo úložiště účet ke kterému jsou připojeny

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.ClassicCompute/domainNames/* |Vytvoření a správa klasické výpočetní názvů domén |
| Microsoft.ClassicCompute/virtualMachines/* |Vytvářet a spravovat virtuální počítače |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |Připojení skupin zabezpečení sítě |
| Microsoft.ClassicNetwork/reservedIps/link/action |Propojení vyhrazené IP adresy |
| Microsoft.ClassicNetwork/reservedIps/read |Čtení rezervovaných IP adres |
| Microsoft.ClassicNetwork/virtualNetworks/join/action |Připojení virtuální sítě |
| Microsoft.ClassicNetwork/virtualNetworks/read |Virtuální sítě pro čtení |
| Microsoft.ClassicStorage/storageAccounts/disks/read |Číst disky účtu úložiště |
| Microsoft.ClassicStorage/storageAccounts/images/read |Přečtěte si účet úložiště bitových kopií |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action |Vypsat klíče účtu úložiště |
| Microsoft.ClassicStorage/storageAccounts/read |Přečtěte si klasických účtů úložiště |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="virtual-machine-contributor"></a>Přispěvatel virtuálních počítačů
Můžete spravovat virtuální počítače, ale není virtuální sítě nebo úložiště účet ke kterému jsou připojeny

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Compute/availabilitySets/* |Vytvářet a spravovat skupiny dostupnosti výpočetní |
| Microsoft.Compute/locations/* |Vytváření a správě výpočetních umístění |
| Microsoft.Compute/virtualMachines/* |Vytvářet a spravovat virtuální počítače |
| Microsoft.Compute/virtualMachineScaleSets/* |Vytvářet a spravovat sady škálování virtuálního počítače |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action |Fondy adres back-end brány aplikace síťové připojení |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Připojení k fondu adres back-end pro vyrovnávání zatížení |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action |Připojení k vyrovnávání zatížení příchozí NAT fondy |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Připojení k vyrovnávání zatížení příchozí pravidla NAT |
| Microsoft.Network/loadBalancers/read |Nástroje pro vyrovnávání zatížení pro čtení |
| Microsoft.Network/locations/* |Vytvářet a spravovat síťová umístění |
| Microsoft.Network/networkInterfaces/* |Vytváření a správě síťových rozhraní |
| Microsoft.Network/networkSecurityGroups/join/action |Připojení skupin zabezpečení sítě |
| Microsoft.Network/networkSecurityGroups/read |Číst skupiny zabezpečení sítě |
| Microsoft.Network/publicIPAddresses/join/action |Připojení k síti veřejné IP adresy |
| Microsoft.Network/publicIPAddresses/read |Veřejné IP adresy sítě pro čtení |
| Microsoft.Network/virtualNetworks/read |Virtuální sítě pro čtení |
| Microsoft.Network/virtualNetworks/subnets/join/action |Připojení k virtuální síti podsítě |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/listKeys/action |Vypsat klíče účtu úložiště |
| Microsoft.Storage/storageAccounts/read |Účty úložiště pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="classic-network-contributor"></a>Přispěvatel klasických sítí
Můžete spravovat klasické virtuální sítě a vyhrazené IP adresy

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.ClassicNetwork/* |Vytvoření a správa klasické sítě |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |

### <a name="web-plan-contributor"></a>Přispěvatel webových plánů
Můžete spravovat plány web

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |
| Microsoft.Web/serverFarms/* |Vytvoření a správa serverové farmy |

### <a name="website-contributor"></a>Přispěvatel webů
Můžete spravovat weby, ale není webové plány, na které jsou připojeny

| **Akce** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Autorizace pro čtení |
| Microsoft.Insights/alertRules/* |Vytvářet a spravovat pravidla výstrah statistiky |
| Microsoft.Insights/components/* |Vytvoření a správa přehled komponenty |
| Microsoft.ResourceHealth/availabilityStatuses/read |Načíst stav prostředků |
| Microsoft.Resources/deployments/* |Vytvářet a spravovat nasazení skupiny prostředků |
| Microsoft.Resources/subscriptions/resourceGroups/read |Skupiny prostředků pro čtení |
| Microsoft.Support/* |Vytvářet a spravovat lístky žádostí o podporu |
| Microsoft.Web/certificates/* |Vytvářet a spravovat certifikáty webu |
| Microsoft.Web/listSitesAssignedToHostName/read |Servery pro čtení, které jsou přiřazené název hostitele |
| Microsoft.Web/serverFarms/join/action |Připojit k serverové farmy |
| Microsoft.Web/serverFarms/read |Přečtěte si serverové farmy |
| Microsoft.Web/sites/* |Vytvoření a správa webů (vytváření webu taky vyžaduje oprávnění k zápisu do přidružené plán služby App Service) |

## <a name="see-also"></a>Další informace najdete v tématech
* [Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.
* [Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Naučte se vytvářet vlastní role podle vašich potřeb přístup.
* [Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md): udržování přehledu o změně přiřazení rolí v RBAC.
* [Na základě rolí řešení potíží s řízení přístupu](role-based-access-control-troubleshooting.md): umožňuje získat návrhy pro řešení běžných problémů.
