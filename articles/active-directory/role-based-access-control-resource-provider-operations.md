---
title: Operace poskytovatele Azure Resource Manager | Microsoft Docs
description: "Podrobnosti o operací dostupných na poskytovateli prostředků Microsoft Azure Resource Manager"
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 9fe7a5f254d8b384cae10ecd34e0bdafa433ff13
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Operace v Azure Resource Manager poskytovatele prostředků

Tento dokument obsahuje seznam operací dostupných pro každý poskytovatel prostředků Microsoft Azure Resource Manager. To umožňuje v vlastní role poskytují podrobné řízení přístupu na základě Role (RBAC) oprávnění k prostředkům v Azure. Poznámka: Toto není kompletní seznam a operace může přidat nebo odebrat, protože každý poskytovatel je aktualizovat. Operace řetězce použijte formát `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Pro komplexní a aktuální seznam použijte `Get-AzureRmProviderOperation` (v prostředí PowerShell) nebo `azure provider operations show` (v Azure CLI) k operacím seznamu zprostředkovatelů prostředků Azure.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Operace | Popis |
|---|---|
|/ configuration/akce|Konfigurace aktualizací klienta.|
|/ services/akce|Aktualizace instance služby v klientovi.|
|/ configuration/zápisu|Vytvoří konfiguraci klienta.|
|/Configuration/Read|Načte konfiguraci klienta.|
|/ services/zápisu|Vytvoří instanci služby v klientovi.|
|/Services/Read|Přečte instance služby v klientovi.|
|/Services/DELETE|Odstraní instanci služby v klientovi.|
|/Services/servicemembers/Action|Vytvoří instanci člen služby ve službě.|
|/Services/servicemembers/Read|Přečte člen instance služby ve službě.|
|/Services/servicemembers/DELETE|Odstraní instanci člen služby ve službě.|
|/Services/servicemembers/Alerts/Read|Přečte výstrahy pro člena služby.|
|/Services/Alerts/Read|Přečte výstrahy pro službu.|
|/Services/Alerts/Read|Přečte výstrahy pro službu.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Operace | Popis |
|---|---|
|/ generateRecommendations nebo akce|Vygeneruje doporučení|
|/ suppressions nebo akce|Vytvoření nebo aktualizace suppressions|
|/ registrace nebo akce|Zaregistruje předplatné pro Microsoft Advisor|
|/generateRecommendations/Read|Získá generovat stav doporučení|
|/Recommendations/Read|Přečte doporučení|
|/suppressions/Read|Získá suppressions|
|/suppressions/DELETE|Odstraní potlačení|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Operace | Popis |
|---|---|
|/Servers/Read|Načte informace zadaný Analysis serveru.|
|/ servery a zápis|Vytvoří nebo aktualizuje zadaný Analysis serveru.|
|/Servers/DELETE|Odstraní Analysis serveru.|
|/Servers/suspend/Action|Pozastaví Analysis serveru.|
|/Servers/Resume/Action|Obnoví Analysis serveru.|
|/ servery/checkNameAvailability<br>/action|Kontroluje, zda zadaný Analysis Server je platný název a ne v používání.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Ověří, zda za předpokladu, že název služby je k dispozici|
|/ registrace nebo akce|Registrace předplatného pro poskytovatele prostředků Microsoft.ApiManagement|
|nebo zrušit registraci nebo akce|Zrušení registrace předplatného pro poskytovatele prostředků Microsoft.ApiManagement|
|/ service/zápisu|Vytvoření nové instance služby API Management|
|/Service/Read|Číst metadata pro instanci služby API Management|
|/Service/DELETE|Odstranění instance služby API Management|
|/Service/updatehostname/Action|Instalační program, aktualizovat nebo odebrat vlastní názvy domén pro služby API Management|
|/Service/uploadcertificate/Action|Nahrajte certifikát SSL pro službu správy rozhraní API|
|/Service/Backup/Action|Zálohování službu správy rozhraní API a zadaného kontejneru. v uživatel zadaný účet úložiště|
|/Service/Restore/Action|Obnovení ze zadaného kontejneru v uživatel zadaný účet úložiště služby API Management|
|/Service/managedeployments/Action|Změnit SKU nebo jednotky, přidat nebo odebrat místní nasazení služby API Management|
|/Service/getssotoken/Action|Získá token jednotného přihlašování, který lze použít k přihlášení na portál starší verze služby API Management jako správce|
|/Service/applynetworkconfigurationupdates/Action|Aktualizuje Microsoft.ApiManagement prostředky spuštěna ve virtuální síti a vyberte aktualizované nastavení sítě.|
|/Service/operationresults/Read|Získá aktuální stav operace probíhající dlouhou dobu|
|/Service/networkStatus/Read|Získá stav přístupu síťových prostředků.|
|/Service/loggers/Read|Získání seznamu protokolovačů nebo získat podrobnosti o protokolovacího nástroje|
|/Service/loggers/Write|Přidat nové protokoly nebo aktualizovat existující podrobnosti protokolovacího nástroje|
|/Service/loggers/DELETE|Odeberte existující protokolovacího nástroje.|
|/Service/Users/Read|Získat seznam registrovaní uživatelé nebo získat podrobnosti o účtu uživatele|
|/Service/Users/Write|Zaregistrovat nového uživatele nebo účet podrobnosti aktualizace stávajícího uživatele|
|/Service/Users/DELETE|Odebrat uživatelský účet|
|/Service/Users/generateSsoUrl/Action|Generování adresy URL jednotné přihlašování. Adresa URL slouží pro přístup k portálu pro správu|
|/Service/Users/Subscriptions/Read|Získat seznam uživatelů odběrů|
|/Service/Users/Keys/Read|Získat seznam klíčů uživatele|
|/Service/Users/groups/Read|Získání seznamu skupin uživatelů|
|/Service/tenant/operationResults/Read|Získejte seznam výsledky operace nebo získat výsledek určité operace|
|/Service/tenant/Policy/Read|Získat konfiguraci zásad pro klienta|
|/Service/tenant/Policy/Write|Nastavení zásad konfigurace pro klienta|
|/Service/tenant/Policy/DELETE|Odebrat konfiguraci zásad pro klienta|
|/Service/tenant/Configuration/Save/Action|Vytvoří potvrzení s snímku konfigurace pro zadaný větve v úložišti|
|/Service/tenant/Configuration/Deploy/Action|Spustí úlohu nasazení provést změny z větve zadaný git konfigurace v databázi.|
|/Service/tenant/Configuration/Validate/Action|Ověří změny z větve zadaný git|
|/Service/tenant/Configuration/operationResults/Read|Získejte seznam výsledky operace nebo získat výsledek určité operace|
|/Service/tenant/Configuration/syncState/Read|Získat stav poslední synchronizace git|
|/Service/tenant/Access/Read|Získat podrobné informace o klientovi přístup|
|/Service/tenant/Access/Write|Aktualizovat podrobnosti o informace o přístupu klienta|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Znova vygenerovat primární přístupový klíč|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Znova vygenerovat sekundární přístupový klíč|
|/Service/identityProviders/Read|Získání seznamu zprostředkovatelů Identity nebo získáte podrobnosti zprostředkovatele Identity|
|/Service/identityProviders/Write|Vytvořit nový zprostředkovatel Identity nebo aktualizace podrobnosti existujícího poskytovatele Identity|
|/Service/identityProviders/DELETE|Odeberte existující zprostředkovatele Identity|
|/Service/Subscriptions/Read|Získat seznam předplatných produktů nebo získat podrobnosti o odběru produktu|
|/Service/Subscriptions/Write|Přihlášení k odběru stávajícího uživatele k existujícímu produktu nebo aktualizovat existující podrobnosti o předplatném. Tato operace slouží k obnovení odběru|
|/Service/Subscriptions/DELETE|Odstraňte odběr. Tato operace slouží k odstranění předplatného|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Znova vygenerovat primární klíč předplatného|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Znova vygenerovat sekundární klíč předplatného|
|/Service/backends/Read|Získání seznamu back-EndY nebo získat podrobnosti o back-end|
|/Service/backends/Write|Přidat nový back-end nebo aktualizovat existující podrobnosti back-end|
|/Service/backends/DELETE|Odeberte existující back-end|
|/Service/APIs/Read|Získání seznamu všech registrované rozhraní API nebo Get podrobné informace o rozhraní API|
|/Service/APIs/Write|Nové rozhraní API umožňuje vytvořit nebo aktualizovat existující podrobnosti o rozhraní API|
|/Service/APIs/DELETE|Odeberte existující rozhraní API|
|/Service/APIs/Policy/Read|Získat podrobnosti o konfiguraci zásad pro rozhraní API|
|/Service/APIs/Policy/Write|Podrobnosti o konfiguraci zásad nastavení pro rozhraní API|
|/Service/APIs/Policy/DELETE|Odebrat konfiguraci zásad z rozhraní API|
|/Service/APIs/Operations/Read|Získání seznamu existující operace rozhraní API nebo získat podrobnosti operace rozhraní API|
|/Service/APIs/Operations/Write|Operaci nového rozhraní API umožňuje vytvořit nebo aktualizovat existující operace rozhraní API|
|/Service/APIs/Operations/DELETE|Odeberte existující operace rozhraní API|
|/Service/APIs/Operations/Policy/Read|Získat podrobnosti o konfiguraci zásad pro operaci|
|/Service/APIs/Operations/Policy/Write|Nastavení zásad konfigurace podrobnosti operace|
|/Service/APIs/Operations/Policy/DELETE|Odebrat konfiguraci zásad z operace|
|/Service/Products/Read|Získat seznam produktů nebo získat podrobnosti o produktu|
|/Service/Products/Write|Vytvořit nového produktu nebo aktualizovat existující podrobnosti o produktu|
|/Service/Products/DELETE|Odeberte existující produktu|
|/Service/Products/Subscriptions/Read|Získání seznamu předplatných produktu|
|/Service/Products/APIs/Read|Získání seznamu rozhraní API, které jsou přidány do existujících produktu|
|/Service/Products/APIs/Write|Přidání existujícího rozhraní API pro stávající produkt|
|/Service/Products/APIs/DELETE|Odeberte existující rozhraní API z existující produktu|
|/Service/Products/Policy/Read|Získání konfigurace zásad existující produktu|
|/Service/Products/Policy/Write|Nastavení zásad konfigurace pro stávající produkt|
|/Service/Products/Policy/DELETE|Odebrat konfiguraci zásad z existující produktu|
|/Service/Products/groups/Read|Získání seznamu skupin vývojáře spojené s produktem|
|/Service/Products/groups/Write|Existující produkt přidružit existující skupinu pro vývojáře|
|/Service/Products/groups/DELETE|Odstraňte přidružení existující produkt existující skupinu pro vývojáře|
|/Service/openidConnectProviders/Read|Získat seznam poskytovatelů OpenID Connect nebo získáte podrobnosti OpenID Connect zprostředkovatele|
|/Service/openidConnectProviders/Write|Vytvořit nové podrobnosti OpenID Connect zprostředkovatele nebo aktualizaci existujícího poskytovatele připojení OpenID|
|/Service/openidConnectProviders/DELETE|Odebrat existující OpenID Connect zprostředkovatele|
|/Service/Certificates/Read|Získání seznamu certifikátů nebo získat podrobnosti o certifikát|
|/Service/Certificates/Write|Přidat nový certifikát|
|/Service/Certificates/DELETE|Odebrat existující certifikát|
|/Service/Properties/Read|Získá seznam všech vlastností nebo získá podrobnosti o zadanou vlastnost|
|/Service/Properties/Write|Vytvoří novou vlastnost nebo aktualizuje hodnotu pro zadanou vlastnost|
|/Service/Properties/DELETE|Odebere existující vlastnost|
|/Service/groups/Read|Získání seznamu skupin nebo získá podrobnosti skupiny|
|/Service/groups/Write|Vytvořit novou skupinu nebo aktualizovat existující skupiny podrobnosti|
|/Service/groups/DELETE|Odeberte existující skupinu|
|/Service/groups/Users/Read|Získání seznamu skupin uživatelů|
|/Service/groups/Users/Write|Přidat stávajícího uživatele do stávající skupiny|
|/Service/groups/Users/DELETE|Odebrat existující uživatele z existující skupiny|
|/Service/authorizationServers/Read|Získat seznam serverů autorizace nebo získat podrobnosti o serveru ověřování|
|/Service/authorizationServers/Write|Vytvořit nový server autorizace nebo podrobné informace o aktualizaci existujícího serveru autorizace|
|/Service/authorizationServers/DELETE|Odeberte existující serveru ověřování|
|/Service/Reports/bySubscription/Read|Získáte sestavy agregovat tímto odběrem.|
|/Service/Reports/byRequest/Read|Získání žádostí o data pro vytváření sestav|
|/Service/Reports/byOperation/Read|Získat sestavy agregován pomocí operací|
|/Service/Reports/byGeo/Read|Získat sestavy agregovat v zeměpisné oblasti|
|/Service/Reports/byUser/Read|Získáte sestavy agregovat vývojáři.|
|/Service/Reports/byTime/Read|Získat sestavy agregovat v časových období|
|/Service/Reports/byApi/Read|Získat sestavy agregovat v rozhraní API|
|/Service/Reports/byProduct/Read|Získáte sestavy agregaci produkty.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Operace | Popis |
|---|---|
|/ elevateAccess nebo akce|Udělí volajícímu přístup Správce uživatelských přístupů v oboru tenanta.|
|/classicAdministrators/Read|Umožňuje načíst správce předplatného.|
|/ classicAdministrators/zápisu|Umožňuje přidat nebo změnit správce předplatného.|
|/classicAdministrators/DELETE|Umožňuje odebrat správce z předplatného.|
|/Locks/Read|Umožňuje načíst zámky v zadaném oboru.|
|/ zámky a zápis|Umožňuje přidat zámky v zadaném oboru.|
|/Locks/DELETE|Umožňuje odstranit zámky v zadaném oboru.|
|/policyAssignments/Read|Získání informací o přiřazení zásady|
|/ policyAssignments/zápisu|Vytvoření přiřazení zásady v zadaném oboru|
|/policyAssignments/DELETE|Odstranění přiřazení zásady v zadaném oboru|
|/Permissions/Read|Umožňuje vypsat seznam všech oprávnění, která má volající v daném oboru.|
|/roleDefinitions/Read|Umožňuje načíst informace o definici role.|
|/ roleDefinitions/zápisu|Odstraňte nebo aktualizujte definice vlastních rolí s určenými oprávněními a přiřaditelnými obory.|
|/roleDefinitions/DELETE|Odstraňte zadané definice vlastních rolí.|
|/providerOperations/Read|Získejte operace pro všechny poskytovatele prostředků, které je možné použít v definicích rolí.|
|/policyDefinitions/Read|Získání informací o definici zásady|
|/ policyDefinitions/zápisu|Vytvoření vlastních zásad pro definice.|
|/policyDefinitions/DELETE|Odstraní definici zásady.|
|/roleAssignments/Read|Umožňuje načíst informace o přiřazení role.|
|/ roleAssignments/zápisu|Umožňuje vytvořit přiřazení role v zadaném oboru.|
|/roleAssignments/DELETE|Umožňuje odstranit přiřazení role v zadaném oboru.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Operace | Popis |
|---|---|
|/automationAccounts/Read|Získá účet automatizace Azure|
|/ automationAccounts/zápisu|Vytvoří nebo aktualizuje účet Azure Automation.|
|/automationAccounts/DELETE|Odstraní účet Azure Automation|
|/automationAccounts/Configurations/readContent/Action|Získá obsah DSC Azure Automation.|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Čte prostředky Hybrid Runbook Worker|
|/automationAccounts/hybridRunbookWorkerGroups/DELETE|Odstraní prostředky Hybrid Runbook Worker|
|/automationAccounts/jobSchedules/Read|Získá plán úloh Azure Automation.|
|/automationAccounts/jobSchedules/Write|Vytvoří plán úloh Azure Automation.|
|/automationAccounts/jobSchedules/DELETE|Odstraní plán úloh Azure Automation.|
|/automationAccounts/connectionTypes/Read|Získá prostředek typu připojení automatizace Azure.|
|/automationAccounts/connectionTypes/Write|Vytvoří prostředek typu připojení automatizace Azure.|
|/automationAccounts/connectionTypes/DELETE|Odstraní prostředek typu připojení automatizace Azure.|
|/automationAccounts/Modules/Read|Získá modul automatizace Azure|
|/automationAccounts/Modules/Write|Vytvoří nebo aktualizuje modul Azure Automation|
|/automationAccounts/Modules/DELETE|Odstraní modul Azure Automation|
|/automationAccounts/credentials/Read|Získá prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/credentials/Write|Vytvoří nebo aktualizuje prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/credentials/DELETE|Odstraní prostředek přihlašovacích údajů Azure Automation.|
|/automationAccounts/Certificates/Read|Načte prostředek certifikátu automatizace Azure.|
|/automationAccounts/Certificates/Write|Vytvoří nebo aktualizuje prostředek certifikátu automatizace Azure|
|/automationAccounts/Certificates/DELETE|Odstraní prostředek certifikátu automatizace Azure|
|/automationAccounts/Schedules/Read|Získá prostředek plánování Azure Automation.|
|/automationAccounts/Schedules/Write|Vytvoří nebo aktualizuje prostředek plánování Azure Automation.|
|/automationAccounts/Schedules/DELETE|Odstraní prostředek plánování Azure Automation.|
|/automationAccounts/Jobs/Read|Získá úlohu automatizace Azure|
|/automationAccounts/Jobs/Write|Vytvoří úlohu Azure Automation|
|/automationAccounts/Jobs/stop/Action|Zastaví úlohu Azure Automation|
|/automationAccounts/Jobs/suspend/Action|Pozastaví úlohu Azure Automation|
|/automationAccounts/Jobs/Resume/Action|Obnoví úlohu automatizace Azure|
|/automationAccounts/Jobs/runbookContent/Action|Získá obsah runbooku automatizace Azure při provádění úlohy|
|/automationAccounts/Jobs/Output/Action|Načte výstup úlohy|
|/automationAccounts/Jobs/Read|Získá úlohu automatizace Azure|
|/automationAccounts/Jobs/Write|Vytvoří úlohu Azure Automation|
|/automationAccounts/Jobs/stop/Action|Zastaví úlohu Azure Automation|
|/automationAccounts/Jobs/suspend/Action|Pozastaví úlohu Azure Automation|
|/automationAccounts/Jobs/Resume/Action|Obnoví úlohu automatizace Azure|
|/automationAccounts/Jobs/Streams/Read|Získá stream úloh Azure Automation|
|/automationAccounts/Connections/Read|Načte prostředek připojení Azure Automation.|
|/automationAccounts/Connections/Write|Vytvoří nebo aktualizuje prostředek připojení Azure Automation.|
|/automationAccounts/Connections/DELETE|Odstraní prostředek připojení Azure Automation.|
|/automationAccounts/Variables/Read|Přečte variabilní prostředek Azure Automation.|
|/automationAccounts/Variables/Write|Vytvoří nebo aktualizuje variabilní prostředek Azure Automation.|
|/automationAccounts/Variables/DELETE|Odstraní variabilní prostředek Azure Automation.|
|/automationAccounts/runbooks/readContent/Action|Získá obsah runbooku automatizace Azure|
|/automationAccounts/runbooks/Read|Načte runbook služby automatizace Azure|
|/automationAccounts/runbooks/Write|Vytvoří nebo aktualizuje runbook služby automatizace Azure|
|/automationAccounts/runbooks/DELETE|Odstraní runbook služby automatizace Azure|
|/automationAccounts/runbooks/draft/readContent/Action|Získá obsah konceptu runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/writeContent/Action|Vytvoří obsah konceptu runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/Read|Získá koncept runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/Publish/Action|Publikuje koncept runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/undoEdit/Action|Vrátit zpět úpravy konceptu runbooku služby Azure Automation.|
|/automationAccounts/runbooks/draft/testJob/Read|Získá testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/Write|Vytvoří testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/stop/Action|Zastaví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/suspend/Action|Pozastaví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/runbooks/draft/testJob/Resume/Action|Obnoví testovací úlohu konceptu runbooku automatizace Azure.|
|/automationAccounts/webhooks/Read|Přečte webhook automatizace Azure|
|/automationAccounts/webhooks/Write|Vytvoří nebo aktualizuje webhook automatizace Azure|
|/automationAccounts/webhooks/DELETE|Odstraní webhook automatizace Azure |
|/automationAccounts/webhooks/generateUri/Action|Vygeneruje URI pro webhook automatizace Azure|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Tento zprostředkovatel není úplná zprostředkovatele ARM a neposkytuje žádné operace ARM.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro poskytovatele prostředků Batch a povolí vytvoření účtů Batch|
|/ batchAccounts/zápisu|Vytvoří nový účet Batch nebo aktualizuje existující účet Batch|
|/batchAccounts/Read|Zobrazí seznam účtů Batch nebo načte vlastnosti účtu Batch|
|/batchAccounts/DELETE|Odstraní účet Batch|
|/batchAccounts/listkeys/Action|Seznamy přístupové klíče pro účet Batch|
|/batchAccounts/regeneratekeys/Action|Obnoví přístupové klíče pro účet Batch|
|/batchAccounts/syncAutoStorageKeys/Action|Synchronizuje přístupové klíče pro účet úložiště automaticky nakonfigurován pro účet Batch|
|/batchAccounts/Applications/Read|Seznam aplikací, nebo načte vlastnosti aplikace|
|/batchAccounts/Applications/Write|Vytvoří novou aplikaci nebo aktualizuje existující aplikace|
|/batchAccounts/Applications/DELETE|Odstraní aplikaci|
|/batchAccounts/Applications/versions/Read|Získá vlastnosti balíčku aplikace|
|/batchAccounts/Applications/versions/Write|Vytvoří nový balíček aplikace nebo aktualizuje existující balíček aplikace|
|/batchAccounts/Applications/versions/Activate/Action|Aktivuje balíček aplikace|
|/batchAccounts/Applications/versions/DELETE|Odstraní balíček aplikace|
|/Locations/Quotas/Read|Získá Batch kvóty předplatného zadané v zadané oblasti Azure|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Operace | Popis |
|---|---|
|/Invoices/Read|K dispozici faktury seznamy|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Operace | Popis |
|---|---|
|/ mapApis/čtení|Operace čtení|
|/ mapApis/zápisu|Operace zápisu|
|/ mapApis/odstranit|Operace odstranění|
|/mapApis/regenerateKey/Action|Znovu vygeneruje klíč.|
|/mapApis/listSecrets/Action|Uvádí tajné klíče.|
|/mapApis/listSingleSignOnToken/Action|Přečíst autorizační token jednotného přihlašování pro prostředek|
|/ Operací/čtení|Popis operace.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Operace | Popis |
|---|---|
|/ checknameavailability nebo akce|Ověří, zda název je k dispozici pro použití s novou mezipaměť Redis|
|/ registrace nebo akce|Registruje zprostředkovatele prostředků 'Microsoft.Cache' s předplatným|
|nebo zrušit registraci nebo akce|Zrušení registrace poskytovatele prostředků 'Microsoft.Cache' s předplatným|
|/ redis a zápis|Upravit nastavení a konfiguraci služby Redis Cache na portálu pro správu|
|/redis/Read|Zobrazit nastavení a konfiguraci Redis Cache na portálu pro správu|
|/redis/DELETE|Odstranit celou službu Redis Cache|
|/redis/listKeys/Action|Zobrazit hodnotu přístupových klíčů služby Redis Cache na portálu pro správu|
|/redis/regenerateKey/Action|Změnit hodnotu přístupových klíčů služby Redis Cache na portálu pro správu|
|/redis/import/Action|Import dat určeného formátu z víc objektů blob do Redis|
|/redis/export/Action|Export dat Redis v určeném formátu do objektů blob úložiště označených prefixem|
|/redis/forceReboot/Action|Vynuťte restartování instance mezipaměti, což ale může mít za následek ztrátu dat.|
|/redis/stop/Action|Zastavte instance mezipaměti.|
|/redis/Start/Action|Spuštění instance mezipaměti.|
|/redis/metricDefinitions/Read|Načte dostupné metriky pro Redis Cache.|
|/redis/firewallRules/Read|Získat IP pravidla brány firewall Redis Cache|
|/redis/firewallRules/Write|Úprava pravidla brány firewall IP Redis Cache|
|/redis/firewallRules/DELETE|Odstranit pravidla firewallu IP mezipaměti Redis|
|/redis/listUpgradeNotifications/Read|Vypíše seznam nejnovějších upozornění na upgrady pro klienta mezipaměti.|
|/redis/linkedservers/Read|Získáte odkazované servery přidružené k mezipaměti redis.|
|/redis/linkedservers/Write|Odkazovaný Server přidat do mezipaměti Redis|
|/redis/linkedservers/DELETE|Odstranit odkazovaný Server z mezipaměti Redis|
|/redis/patchSchedules/Read|Získá oprav plán Redis Cache|
|/redis/patchSchedules/Write|Změnit plán oprav mezipaměti Redis|
|/redis/patchSchedules/DELETE|Odstranit plán oprava mezipaměti Redis|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Operace | Popis |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant nebo akce|Zřízení instančního objektu pro objekt zabezpečení služby aplikace|
|/ validateCertificateRegistrationInformation nebo akce|Ověření objektu nákupu certifikát bez odeslání|
|/ registrace nebo akce|Registrace poskytovatele prostředků Certificates společnosti Microsoft pro předplatné|
|/ certificateOrders/zápisu|Přidat nové certificateOrder nebo aktualizovat stávající|
|/ certificateOrders/odstranit|Odstraňte existující AppServiceCertificate|
|/ certificateOrders/čtení|Získání seznamu CertificateOrders|
|/certificateOrders/reissue/Action|Opakujte existující certificateorder|
|/certificateOrders/renew/Action|Obnovit existující certificateorder|
|/certificateOrders/retrieveCertificateActions/Action|Načtení seznamu akce certifikátu|
|/certificateOrders/retrieveEmailHistory/Action|Načtení historie certifikát e-mailu|
|/certificateOrders/resendEmail/Action|Znovu odeslat e-mail certifikátu|
|/certificateOrders/verifyDomainOwnership/Action|Ověřit vlastnictví domény|
|/certificateOrders/resendRequestEmails/Action|Znovu odeslat žádost o e-mailů jinou e-mailová adresa|
|/certificateOrders/resendRequestEmails/Action|Načtení lokality zapečetění vydaného certifikátu služby aplikace|
|/certificateOrders/Certificates/Write|Přidat nový certifikát nebo aktualizovat stávající|
|/certificateOrders/Certificates/DELETE|Odstraňte existující certifikát|
|/certificateOrders/Certificates/Read|Získat seznam certifikátů|

## <a name="microsoftclassiccompute"></a>Microsoft.Network

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrovat pro klasické výpočetní prostředky|
|/ checkDomainNameAvailability nebo akce|Umožňuje zkontrolovat dostupnost zadaného názvu domény.|
|/ moveSubscriptionResources nebo akce|Přesune všechny klasické prostředky do jiného předplatného.|
|/ validateSubscriptionMoveAvailability nebo akce|Ověří dostupnost klasické operace přesunu u předplatného.|
|/operatingSystemFamilies/Read|Uvádí hostovaného operačního systému rodiny, který je k dispozici ve službě Microsoft Azure a také uvádí verze operačního systému k dispozici pro každý f
|/Capabilities/Read|Umožňuje zobrazit schopnosti.|
|/operatingSystems/Read|Vypíše seznam verzí hostovaných operačních systémů, které jsou aktuálně dostupné v Microsoft Azure.|
|/resourceTypes/skus/Read|Načte seznam skladových položek pro podporované typy prostředků.|
|/domainNames/Read|Umožňuje načíst názvy domén pro prostředky.|
|/ domainNames/zápisu|Umožňuje přidat nebo změnit názvy domén pro prostředky.|
|/domainNames/DELETE|Umožňuje odebrat názvy domén pro prostředky.|
|/domainNames/swap/Action|Umožňuje přepnout přípravný slot na produkční.|
|/domainNames/serviceCertificates/Read|Umožňuje načíst používané certifikáty služeb.|
|/domainNames/serviceCertificates/Write|Umožňuje přidat nebo změnit používané certifikáty služeb.|
|/domainNames/serviceCertificates/DELETE|Umožňuje odstranit používané certifikáty služeb.|
|/domainNames/serviceCertificates/operationStatuses/Read|Přečte stav operace pro certifikáty služeb názvů domén.|
|/domainNames/Capabilities/Read|Ukazuje možnosti názvu domény|
|/domainNames/Extensions/Read|Vrátí rozšíření názvu domény.|
|/domainNames/Extensions/Write|Přidá rozšíření názvu domény.|
|/domainNames/Extensions/DELETE|Odebere rozšíření názvu domény.|
|/domainNames/Extensions/operationStatuses/Read|Přečte stav operace pro rozšíření názvů domén.|
|/domainNames/Active/Write|Slouží k nastavení názvu aktivní domény.|
|/domainNames/slots/Read|Umožňuje zobrazit sloty pro nasazení.|
|/domainNames/slots/Write|Umožňuje vytvořit nebo aktualizovat slot pro nasazení.|
|/domainNames/slots/DELETE|Umožňuje odstranit zadaný slot pro nasazení.|
|/domainNames/slots/Start/Action|Umožňuje spustit slot pro nasazení.|
|/domainNames/slots/stop/Action|Umožňuje pozastavit slot pro nasazení.|
|/domainNames/slots/operationStatuses/Read|Přečte stav operace pro sloty názvů domén.|
|/domainNames/slots/Roles/Read|Umožňuje načíst roli slotu pro nasazení.|
|/domainNames/slots/Roles/extensionReferences/Read|Vrátí odkaz na rozšíření u role slotu nasazení.|
|/domainNames/slots/Roles/extensionReferences/Write|Přidá nebo upraví odkaz na rozšíření u role slotu nasazení.|
|/domainNames/slots/Roles/extensionReferences/DELETE|Odebere odkaz na rozšíření u role slotu nasazení.|
|/domainNames/slots/Roles/extensionReferences/operationStatuses/Read|Přečte stav operace pro reference na rozšíření rolí slotů názvů domén.|
|/domainNames/slots/Roles/roleInstances/Read|Umožňuje načíst instanci role.|
|/domainNames/slots/Roles/roleInstances/restart/Action|Restartování instance role.|
|/domainNames/slots/Roles/roleInstances/reimage/Action|Umožňuje obnovit instanci role z image.|
|/domainNames/slots/Roles/roleInstances/operationStatuses/Read|Přečte stav operace pro instance rolí u rolí slotů názvů domén.|
|/domainNames/slots/State/Start/Write|Umožňuje změnit stav slotu pro nasazení na Zastavený.|
|/domainNames/slots/State/stop/Write|Umožňuje změnit stav slotu pro nasazení na Spuštěný.|
|/domainNames/slots/upgradeDomain/Write|Umožňuje upgradovat doménu.|
|/domainNames/internalLoadBalancers/Read|Umožňuje načíst interní nástroje pro vyrovnávání zatížení.|
|/domainNames/internalLoadBalancers/Write|Umožňuje vytvořit nový interní nástroj pro vyrovnávání zatížení.|
|/domainNames/internalLoadBalancers/DELETE|Umožňuje odebrat interní nástroj pro vyrovnávání zatížení.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Přečte stav operace pro interní služby Load Balancer názvů domén.|
|/domainNames/loadBalancedEndpointSets/Read|Umožňuje zobrazit skupiny koncových bodů s vyrovnáváním zatížení.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Přečte stav operace pro sady koncových bodů s vyrovnaným zatížením u názvů domén.|
|/domainNames/availabilitySets/Read|Umožňuje zobrazit skupinu dostupnosti pro prostředek.|
|/Quotas/Read|Umožňuje načíst kvótu pro předplatné.|
|/virtualMachines/Read|Umožňuje načíst seznam virtuálních počítačů.|
|/ virtuálních počítačů a zápis|Umožňuje přidat nebo upravit virtuální počítače.|
|/virtualMachines/DELETE|Odebere virtuální počítače.|
|/virtualMachines/Start/Action|Umožňuje spustit virtuální počítač.|
|/virtualMachines/redeploy/Action|Opětovně nasadí virtuální počítač.|
|/virtualMachines/restart/Action|Restartuje virtuální počítače.|
|/virtualMachines/stop/Action|Umožňuje zastavit virtuální počítač.|
|/virtualMachines/Shutdown/Action|Umožňuje vypnout virtuální počítač.|
|/virtualMachines/attachDisk/Action|Umožňuje připojit datový disk k virtuálnímu počítači.|
|/virtualMachines/detachDisk/Action|Umožňuje odpojit datový disk od virtuálního počítače.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|Umožňuje stáhnout soubor RDP pro virtuální počítač.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/čtení|Načte skupinu zabezpečení sítě spojenou se síťovým rozhraním.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups a zápis|Přidá skupinu zabezpečení sítě spojenou se síťovým rozhraním.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups nebo odstranění|Odstraní skupinu zabezpečení sítě spojenou se síťovým rozhraním.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/operationStatuses/čtení|Přečte stav operace pro skupiny zabezpečení sítě spojené s virtuálními počítači.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/Read|Umožňuje načíst definice metrik.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Read|Umožňuje načíst nastavení diagnostiky.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Write|Umožňuje přidat nebo změnit nastavení diagnostiky.|
|/virtualMachines/Metrics/Read|Umožňuje načíst metriky.|
|/virtualMachines/operationStatuses/Read|Přečte stav operace pro virtuální počítače|
|/virtualMachines/Extensions/Read|Umožňuje načíst rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/Write|Umožňuje vložit rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/operationStatuses/Read|Přečte stav operace pro rozšíření virtuálních počítačů.|
|/virtualMachines/asyncOperations/Read|Umožňuje získat seznam možných asynchronních operací.|
|/virtualMachines/Disks/Read|Umožňuje načíst seznam datových disků.|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Načte skupinu zabezpečení sítě spojenou s virtuálním počítačem.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Přidá skupinu zabezpečení sítě spojenou s virtuálním počítačem.|
|/virtualMachines/associatedNetworkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě spojenou s virtuálním počítačem.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Přečte stav operace pro skupiny zabezpečení sítě spojené s virtuálními počítači.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrace pro klasickou síť|
|/gatewaySupportedDevices/Read|Umožňuje načíst seznam podporovaných zařízení.|
|/reservedIps/Read|Umožňuje načíst vyhrazené IP adresy.|
|/ rezervovaných adres IP a zápis|Umožňuje přidat novou vyhrazenou IP adresu.|
|/reservedIps/DELETE|Umožňuje odstranit vyhrazenou IP adresu.|
|/reservedIps/Link/Action|Odkaz na rezervovanou IP adresu|
|/reservedIps/JOIN/Action|Připojit se k rezervované IP adrese|
|/reservedIps/operationStatuses/Read|Přečte stav operace pro rezervované IP adresy.|
|/virtualNetworks/Read|Umožňuje načíst virtuální síť.|
|/ virtualNetworks/zápisu|Umožňuje přidat novou virtuální síť.|
|/virtualNetworks/DELETE|Umožňuje odstranit virtuální síť.|
|/virtualNetworks/peer/Action|Vytvoří partnerskou virtuální síť s jinou virtuální sítí.|
|/virtualNetworks/JOIN/Action|Připojí se k virtuální síti.|
|/virtualNetworks/checkIPAddressAvailability/Action|Zkontroluje dostupnost dané IP adresy ve virtuální síti.|
|/virtualNetworks/Capabilities/Read|Umožňuje zobrazit schopnosti.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups/čtení|Načte skupinu zabezpečení sítě spojenou s podsítí.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups a zápis|Přidá skupinu zabezpečení sítě spojenou s podsítí.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups nebo odstranění|Odstraní skupinu zabezpečení sítě spojenou s podsítí.|
|/virtualNetworks/podsítě /<br>associatedNetworkSecurityGroups/operationStatuses/čtení|Přečte stav operace pro skupinu zabezpečení sítě spojenou s podsítí virtuální sítě.|
|/virtualNetworks/operationStatuses/Read|Přečte stav operace pro virtuální sítě.|
|/virtualNetworks/gateways/Read|Umožňuje načíst brány virtuální sítě.|
|/virtualNetworks/gateways/Write|Umožňuje přidat bránu virtuální sítě.|
|/virtualNetworks/gateways/DELETE|Umožňuje odstranit bránu virtuální sítě.|
|/virtualNetworks/gateways/startDiagnostics/Action|Umožňuje spustit diagnostiku pro bránu virtuální sítě.|
|/virtualNetworks/gateways/stopDiagnostics/Action|Umožňuje zastavit diagnostiku pro bránu virtuální sítě.|
|/virtualNetworks/gateways/downloadDiagnostics/Action|Umožňuje stáhnout diagnostiku brány.|
|/virtualNetworks/gateways/listCircuitServiceKey/Action|Umožňuje načíst klíč služby okruhů.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/Action|Umožňuje stáhnout skript pro konfiguraci zařízení.|
|/virtualNetworks/gateways/listPackage/Action|Umožňuje vypsat balíček bran virtuální sítě.|
|/virtualNetworks/gateways/operationStatuses/Read|Přečte stav operace pro brány virtuálních sítí.|
|/virtualNetworks/gateways/Packages/Read|Umožňuje načíst balíček bran virtuální sítě.|
|/virtualNetworks/gateways/Connections/Read|Umožňuje načíst seznam připojení.|
|/virtualNetworks/gateways/Connections/Connect/Action|Umožňuje připojit lokalitu k připojení brány lokality.|
|/virtualNetworks/gateways/Connections/Disconnect/Action|Umožňuje odpojit lokalitu od připojení brány lokality.|
|/virtualNetworks/gateways/Connections/test/Action|Umožňuje otestovat připojení lokality k bráně lokality.|
|/virtualNetworks/gateways/clientRevokedCertificates/Read|Umožňuje načíst odvolané klientské certifikáty.|
|/virtualNetworks/gateways/clientRevokedCertificates/Write|Umožňuje odvolat klientský certifikát.|
|/virtualNetworks/gateways/clientRevokedCertificates/DELETE|Umožňuje zrušit odvolání klientského certifikátu.|
|/virtualNetworks/gateways/clientRootCertificates/Read|Umožňuje vyhledat klientské kořenové certifikáty.|
|/virtualNetworks/gateways/clientRootCertificates/Write|Umožňuje nahrát nový klientský kořenový certifikát.|
|/virtualNetworks/gateways/clientRootCertificates/DELETE|Umožňuje odstranit klientský certifikát brány virtuální sítě.|
|/virtualNetworks/gateways/clientRootCertificates/download/Action|Umožňuje stáhnout certifikát podle kryptografického otisku.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/Action|Vypíše balíček certifikátů bran virtuální sítě.|
|/networkSecurityGroups/Read|Načte skupinu zabezpečení sítě.|
|/ skupin zabezpečení sítě a zápis|Přidá novou skupinu zabezpečení sítě.|
|/networkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě.|
|/networkSecurityGroups/operationStatuses/Read|Přečte stav operace pro skupinu zabezpečení sítě.|
|/networkSecurityGroups/securityRules/Read|Načte pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/Write|Přidá nebo aktualizuje pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/DELETE|Odstraní pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Přečte stav operace pro pravidla zabezpečení skupiny u zabezpečení sítě.|
|/Quotas/Read|Umožňuje načíst kvótu pro předplatné.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Umožňuje registraci v klasickém úložišti.|
|/ checkStorageAccountAvailability nebo akce|Umožňuje zkontrolovat dostupnost účtu úložiště.|
|/Capabilities/Read|Umožňuje zobrazit schopnosti.|
|/publicImages/Read|Načte veřejnou image virtuálního počítače.|
|/Images/Read|Vrátí image.|
|/storageAccounts/Read|Umožňuje načíst účet úložiště se zadaným účtem.|
|/ storageAccounts/zápisu|Umožňuje přidat nový účet úložiště.|
|/storageAccounts/DELETE|Umožňuje odstranit účet úložiště.|
|/storageAccounts/listKeys/Action|Umožňuje vypsat seznam přístupových klíčů pro účty úložiště.|
|/storageAccounts/regenerateKey/Action|Umožňuje znovu vytvořit existující přístupové klíče pro účet úložiště.|
|/storageAccounts/operationStatuses/Read|Umožňuje načíst stav operace pro prostředek.|
|/storageAccounts/Images/Read|Vrátí image účtu Storage.|
|/storageAccounts/Images/DELETE|Odstraní danou image účtu Storage.|
|/storageAccounts/Disks/Read|Vrátí disk účtu úložiště.|
|/storageAccounts/Disks/Write|Přidá disk účtu úložiště.|
|/storageAccounts/Disks/DELETE|Odstraní daný disk účtu úložiště.|
|/storageAccounts/Disks/operationStatuses/Read|Umožňuje načíst stav operace pro prostředek.|
|/storageAccounts/osImages/Read|Vrátí image operačního systému pro účet Storage.|
|/storageAccounts/osImages/DELETE|Odstraní danou image operačního systému pro účet Storage.|
|/storageAccounts/Services/Read|Umožňuje načíst dostupné služby.|
|/storageAccounts/Services/metricDefinitions/Read|Umožňuje načíst definice metrik.|
|/storageAccounts/Services/Metrics/Read|Umožňuje načíst metriky.|
|/storageAccounts/Services/diagnosticSettings/Read|Umožňuje načíst nastavení diagnostiky.|
|/storageAccounts/Services/diagnosticSettings/Write|Umožňuje přidat nebo změnit nastavení diagnostiky.|
|/Disks/Read|Vrátí disk účtu úložiště.|
|/osImages/Read|Vrátí image operačního systému.|
|/Quotas/Read|Umožňuje načíst kvótu pro předplatné.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Operace | Popis |
|---|---|
|/Accounts/Read|Načte účty rozhraní API.|
|/ účty a zápis|Zapíše účty rozhraní API.|
|/Accounts/DELETE|Odstraní účty rozhraní API|
|/Accounts/listKeys/Action|Seznam klíčů|
|/Accounts/regenerateKey/Action|Znovu vygenerovat klíč|
|/Accounts/skus/Read|Načte dostupné SKU pro existující prostředek.|
|/Accounts/usages/Read|Získáte využití kvóty pro existující prostředek.|
|/ Operací/čtení|Popis operace.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Operace | Popis |
|---|---|
|/ RateCard/čtení|Vrátí nabízejí data, prostředku nebo měření metadata a sazby pro dané předplatné.|
|/ UsageAggregates/čtení|Získá předplatné Microsoft Azure spotřeby. Výsledek obsahuje agreguje data o využití, předplatné a prostředků související informace, na konkrétní časové rozmezí.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Umožňuje registrovat předplatné zprostředkovatele prostředků Microsoft.Compute.|
|/restorePointCollections/Read|Umožňuje získat vlastnosti kolekce bodu obnovení|
|/ restorePointCollections/zápisu|Vytvoří novou kolekci bodu obnovení nebo aktualizuje existující|
|/restorePointCollections/DELETE|Odstranění obnovení bodu kolekce a obsažené bodů obnovení|
|/restorePointCollections/restorePoints/Read|Umožňuje získat vlastnosti bod obnovení|
|/restorePointCollections/restorePoints/Write|Vytvoří nový bod obnovení|
|/restorePointCollections/restorePoints/DELETE|Odstraní bodu obnovení|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|Umožňuje získat vlastnosti bod obnovení společně s identifikátory URI SAS objektu blob|
|/virtualMachineScaleSets/Read|Umožňuje získat vlastnosti škálovací sady virtuálních počítačů.|
|/ virtualMachineScaleSets/zápisu|Umožňuje vytvořit novou nebo aktualizovat stávající škálovací sadu virtuálních počítačů.|
|/virtualMachineScaleSets/DELETE|Umožňuje odstranit škálovací sadu virtuálních počítačů.|
|/virtualMachineScaleSets/Start/Action|Umožňuje spustit instance škálovací sady virtuálních počítačů.|
|/virtualMachineScaleSets/powerOff/Action|Umožňuje vypnout instance škálovací sady virtuálních počítačů.|
|/virtualMachineScaleSets/restart/Action|Umožňuje restartovat instance škálovací sady virtuálních počítačů.|
|/virtualMachineScaleSets/deallocate/Action|Umožňuje vypnout instance škálovací sady virtuálních počítačů a uvolnit výpočetní prostředky, které tyto instance zabíraly. |
|/virtualMachineScaleSets/manualUpgrade/Action|Umožňuje ručně aktualizovat instance na poslední model škálovací sady virtuálních počítačů.|
|/virtualMachineScaleSets/Scale/Action|Škálování v / set horizontální navýšení kapacity počet instancí existující škálování virtuálních počítačů|
|/virtualMachineScaleSets/instanceView/Read|Umožňuje získat zobrazení instance škálovací sady virtuálních počítačů.|
|/virtualMachineScaleSets/skus/Read|Umožňuje zobrazit seznam platných SKU pro existující škálovací sadu virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/Read|Umožňuje načíst vlastnosti virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/DELETE|Umožňuje odstranit konkrétní virtuální počítač ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/Start/Action|Umožňuje spustit instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Umožňuje vypnout instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Umožňuje restartovat instanci virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Umožňuje vypnout a uvolnit výpočetní prostředky pro virtuální počítač ve škálovací sadě virtuálních počítačů.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Umožňuje načíst zobrazení instance virtuálního počítače ve škálovací sadě virtuálních počítačů.|
|/Images/Read|Umožňuje získat vlastnosti bitové kopie|
|/ bitové kopie a zápis|Vytvoří novou bitovou kopii nebo aktualizuje existující|
|/Images/DELETE|Odstraní bitovou kopii|
|/Operations/Read|Umožňuje zobrazit seznam operací dostupných na zprostředkovateli prostředků Microsoft.Compute.|
|/Disks/Read|Získá vlastnosti disku.|
|/ disků a zápis|Vytvoří nový disk nebo aktualizuje stávající.|
|/Disks/DELETE|Odstraní disky.|
|/Disks/beginGetAccess/Action|Získat identifikátor URI SAS disku pro přístup k objektu blob|
|/Disks/endGetAccess/Action|Odvolá identifikátor URI SAS disku|
|/snapshots/Read|Získat vlastnosti snímku|
|/ snímky a zápis|Vytvoří nový snímek nebo aktualizuje stávající.|
|/snapshots/DELETE|Odstranit snímek|
|/availabilitySets/Read|Umožňuje získat vlastnosti sady dostupnosti.|
|/ availabilitySets/zápisu|Umožňuje vytvořit novou nebo aktualizovat stávající sadu dostupnosti.|
|/availabilitySets/DELETE|Umožňuje odstranit sadu dostupnosti.|
|/availabilitySets/vmSizes/Read|Umožňuje zobrazit seznam dostupných velikostí pro vytváření nebo aktualizaci virtuálního počítače v sadě dostupnosti.|
|/virtualMachines/Read|Umožňuje získat vlastnosti virtuálního počítače.|
|/ virtuálních počítačů a zápis|Umožňuje vytvořit nový nebo aktualizovat stávající virtuální počítač.|
|/virtualMachines/DELETE|Umožňuje odstranit virtuální počítač.|
|/virtualMachines/Start/Action|Umožňuje spustit virtuální počítač.|
|/virtualMachines/powerOff/Action|Umožňuje vypnout virtuální počítač. Všimněte si, že virtuální počítač bude dál účtovat.|
|/virtualMachines/redeploy/Action|Opětovně nasadí virtuální počítač|
|/virtualMachines/restart/Action|Umožňuje restartovat virtuální počítač.|
|/virtualMachines/deallocate/Action|Umožňuje vypnout virtuální počítač a uvolnit výpočetní prostředky.|
|/virtualMachines/generalize/Action|Umožňuje nastavit stav virtuálního počítače na Zobecněno a připravit virtuální počítač na zachycení.|
|/virtualMachines/Capture/Action|Umožňuje zachytit virtuální počítač zkopírováním virtuálních pevných disků a vygenerováním šablony, kterou je možné použít k vytvoření podobných virtuálních počítačů.|
|/virtualMachines/convertToManagedDisks/Action|Převede objekt blob na základě disky virtuálního počítače na spravovaného disky|
|/virtualMachines/vmSizes/Read|Umožňuje zobrazit seznam dostupných velikostí, na které je možné virtuální počítač aktualizovat.|
|/virtualMachines/instanceView/Read|Umožňuje získat podrobný stav runtime virtuálního počítače a jeho prostředků.|
|/virtualMachines/Extensions/Read|Umožňuje získat vlastnosti rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/Write|Umožňuje vytvořit nové nebo aktualizovat stávající rozšíření virtuálního počítače.|
|/virtualMachines/Extensions/DELETE|Umožňuje odstranit rozšíření virtuálního počítače.|
|/Locations/vmSizes/Read|Umožňuje zobrazit seznam dostupných velikostí virtuálních počítačů v daném umístění.|
|/Locations/usages/Read|Umožňuje získat limity služby a aktuální využívaná množství výpočetních prostředků předplatného v daném umístění.|
|/Locations/Operations/Read|Umožňuje získat stav asynchronní operace.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro zprostředkovatele prostředků registru kontejneru a umožňuje vytvoření kontejneru registrech.|
|/checknameavailability/Read|Ověří, že název registru je platný a není používán.|
|/registries/Read|Vrátí seznam registrech kontejneru nebo načte vlastnosti zadaného kontejneru registru.|
|/ registrech a zápis|Vytvoří kontejner registru se zadanými parametry nebo aktualizovat vlastnosti a značky pro zadaný kontejner registru.|
|/registries/DELETE|Odstraní existujícího registru kontejneru.|
|/registries/listCredentials/Action|Uvádí přihlašovací údaje pro zadaný kontejner registru.|
|/registries/regenerateCredential/Action|Regeneruje přihlašovací údaje pro zadaný kontejner registru.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Operace | Popis |
|---|---|
|/containerServices/Subscriptions/Read|Získá určené služby kontejneru na základě předplatného.|
|/containerServices/resourceGroups/Read|Získá určené služby kontejneru na základě skupiny prostředků.|
|/containerServices/resourceGroups/ContainerServiceName/Read|Získá určenou službu kontejneru.|
|/containerServices/resourceGroups/ContainerServiceName/Write|Použije nebo aktualizuje určenou službu kontejneru.|
|/containerServices/resourceGroups/ContainerServiceName/DELETE|Odstraní určenou službu kontejneru.|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Operace | Popis |
|---|---|
|/ updateCommunicationPreference nebo akce|Předvolby komunikace aktualizace|
|/ listCommunicationPreference nebo akce|Seznam komunikace předvoleb|
|/Applications/Read|Operace čtení|
|/ aplikace/zápisu|Operace zápisu|
|/ aplikace/zápisu|Operace zápisu|
|/Applications/DELETE|Operace odstranění|
|/Applications/listSecrets/Action|Výpis tajných klíčů|
|/Applications/listSingleSignOnToken/Action|Přečíst jednotného přihlašování tokeny|
|/Operations/Read|operace čtení|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Operace | Popis |
|---|---|
|/hubs/Read|Číst všechny statistiky centra Azure zákazníků|
|/ hubs a zápis|Vytvořit nebo aktualizovat všechny statistiky centra Azure zákazníků|
|/hubs/DELETE|Odstranit všechny statistiky Centrum Azure zákazníků|
|/hubs/providers/Microsoft.Insights/metricDefinitions/Read|Načte dostupné metriky pro prostředek|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Read|Získá nastavení diagnostiky pro tento prostředek.|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Write|Vytvoří nebo aktualizuje nastavení diagnostiky pro tento prostředek.|
|/hubs/providers/Microsoft.Insights/logDefinitions/Read|Získá dostupné protokoly pro prostředek|
|/hubs/authorizationPolicies/Read|Přečtěte si, že žádné přehledy Azure zákazníků sdílené zásady přístupu podpis|
|/hubs/authorizationPolicies/Write|Vytvořit nebo aktualizovat žádné zásady Azure zákazníků Statistika sdílený přístupový podpis|
|/hubs/authorizationPolicies/DELETE|Odstranit všechny zásady Azure zákazníků Statistika sdílený přístupový podpis|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Znova vygenerovat primární klíč Azure zákazníka Statistika zásada sdíleného přístupu podpis|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Znova vygenerovat sekundární klíč Azure zákazníka Statistika zásada sdíleného přístupu podpis|
|/hubs/Profiles/Read|Číst všechny statistiky profil Azure zákazníků|
|/hubs/Profiles/Write|Zápis žádný profil Statistika Azure zákazníků|
|/hubs/KPI/Read|Číst všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/KPI/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/KPI/DELETE|Odstranit všechny Azure zákazníků Statistika klíčového ukazatele výkonu|
|/hubs/views/Read|Přečtěte si všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/views/Write|Vytvořit nebo aktualizovat všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/views/DELETE|Odstranit všechna zobrazení statistiky aplikace Azure zákazníků|
|/hubs/interactions/Read|Číst interakce Statistika Azure zákazníků|
|/hubs/interactions/Write|Vytvořit nebo aktualizovat interakce Statistika Azure zákazníků|
|/hubs/roleAssignments/Read|Číst všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/roleAssignments/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/roleAssignments/DELETE|Odstranit všechny Azure zákazníků Statistika Rbac přiřazení|
|/hubs/Connectors/Read|Číst všechny konektor služby Statistika Azure zákazníků|
|/hubs/Connectors/Write|Vytvořit nebo aktualizovat všechny konektor služby Statistika Azure zákazníků|
|/hubs/Connectors/DELETE|Odstranit všechny konektory Statistika Azure zákazníků|
|/hubs/Connectors/Mappings/Read|Číst všechny Azure zákazníků Statistika konektor mapování|
|/hubs/Connectors/Mappings/Write|Vytvořit nebo aktualizovat všechny Azure zákazníků Statistika konektor mapování|
|/hubs/Connectors/Mappings/DELETE|Odstranit všechny Azure zákazníků Statistika konektor mapování|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost názvu katalogu pro klienta.|
|/catalogs/Read|Načíst vlastnosti pro katalogu nebo katalogů pod předplatné nebo skupinu prostředků.|
|/ katalogů a zápis|Katalog vytvoří nebo aktualizuje značky a vlastnosti pro katalog.|
|/catalogs/DELETE|Odstraní katalogu.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Operace | Popis |
|---|---|
|/datafactories/Read|Čte Data Factory.|
|/ datafactories/zápisu|Vytvořit nebo aktualizovat objekt pro vytváření dat|
|/datafactories/DELETE|Odstraní Data Factory.|
|/datafactories/datapipelines/Read|Přečte kanálu.|
|/datafactories/datapipelines/DELETE|Odstraní kanálu.|
|/datafactories/datapipelines/pause/Action|Pozastaví kanálu.|
|/datafactories/datapipelines/Resume/Action|Obnoví kanálu.|
|/datafactories/datapipelines/Update/Action|Aktualizace kanálu.|
|/datafactories/datapipelines/Write|Vytvořit nebo aktualizovat kanál|
|/datafactories/linkedServices/Read|Přečte propojenou službu.|
|/datafactories/linkedServices/DELETE|Odstraní propojenou službu.|
|/datafactories/linkedServices/Write|Vytvoření nebo aktualizace propojené služby|
|/datafactories/Tables/Read|Čtení tabulky.|
|/datafactories/Tables/DELETE|Odstraní tabulku.|
|/datafactories/Tables/Write|Vytvořit nebo aktualizovat tabulky|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Operace | Popis |
|---|---|
|/Accounts/Read|Získání informací o účtu DataLakeAnalytics.|
|/ účty a zápis|Vytvořit nebo aktualizovat účet DataLakeAnalytics.|
|/Accounts/DELETE|Odstranění účtu DataLakeAnalytics.|
|/Accounts/firewallRules/Read|Získání informací o pravidlo brány firewall.|
|/Accounts/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall.|
|/Accounts/firewallRules/DELETE|Odstraňte pravidlo brány firewall.|
|/Accounts/storageAccounts/Read|U účtu DataLakeAnalytics získáte propojený účet úložiště.|
|/Accounts/storageAccounts/Write|Účet úložiště propojte DataLakeAnalytics účtu.|
|/Accounts/storageAccounts/DELETE|Zrušit propojení účtu úložiště z účtu DataLakeAnalytics.|
|/Accounts/storageAccounts/Containers/Read|Získat kontejnery v účtu úložiště|
|/Accounts/storageAccounts/Containers/listSasTokens/Action|Seznam tokeny SAS pro kontejner úložiště|
|/Accounts/dataLakeStoreAccounts/Read|Získáte účet propojené DataLakeStore DataLakeAnalytics účtu.|
|/Accounts/dataLakeStoreAccounts/Write|Účet DataLakeStore propojte DataLakeAnalytics účtu.|
|/Accounts/dataLakeStoreAccounts/DELETE|Zrušit propojení účtu DataLakeStore z DataLakeAnalytics účtu.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Operace | Popis |
|---|---|
|/Accounts/Read|Získání informací o účtu DataLakeStore byl vytvořen.|
|/ účty a zápis|Vytvořit nový účet DataLakeStore nebo aktualizovat účet byl vytvořen DataLakeStore.|
|/Accounts/DELETE|Odstraňte účet byl vytvořen DataLakeStore.|
|/Accounts/firewallRules/Read|Získání informací o pravidlo brány firewall.|
|/Accounts/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall.|
|/Accounts/firewallRules/DELETE|Odstraňte pravidlo brány firewall.|
|/Accounts/trustedIdProviders/Read|Získáte informace o zprostředkovateli důvěryhodné identity.|
|/Accounts/trustedIdProviders/Write|Vytvořit nebo aktualizovat zprostředkovatele důvěryhodné identity.|
|/Accounts/trustedIdProviders/DELETE|Odstraňte zprostředkovatele důvěryhodné identity.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrace předplatného pro poskytovatele prostředků IotHub a umožňuje vytváření prostředků IotHub|
|/ checkNameAvailability nebo akce|Zkontrolujte, zda je název IotHub-li k dispozici|
|/ použití nebo pro čtení|Získáte podrobnosti o použití předplatné pro tohoto zprostředkovatele.|
|/ operací/čtení|Získat všechny operace ResourceProvider|
|/ iotHubs/čtení|Získá prostředky IotHub|
|/ iotHubs/zápisu|Vytvořit nebo aktualizovat prostředek IotHub|
|/ iotHubs/odstranit|Odstranit prostředek IotHub|
|/iotHubs/listkeys/Action|Získat všechny klíče IotHub|
|/iotHubs/exportDevices/Action|Export zařízení|
|/iotHubs/importDevices/Action|Import zařízení|
|/ IotHubs/metricDefinitions/čtení|Načte dostupné metriky pro službu IotHub|
|/iotHubs/iotHubKeys/listkeys/Action|Získat klíč IotHub pro daným názvem.|
|/iotHubs/iotHubStats/Read|Získat statistiku IotHub|
|/iotHubs/quotaMetrics/Read|Získat metriky kvóty|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Vytvoření skupiny příjemců EventHub|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Získat EventHub příjemce skupiny|
|/iotHubs/eventHubEndpoints/consumerGroups/DELETE|Odstranění skupiny příjemců EventHub|
|/iotHubs/Routing/Routes/$ testall nebo akce|Testovací zpráva proti všechny existující trasy|
|/iotHubs/Routing/Routes/$ testnew nebo akce|Testovací zpráva proti testu zadané trasy|
|/ IotHubs/diagnosticSettings/čtení|Získá nastavení diagnostiky pro tento prostředek.|
|/ IotHubs/diagnosticSettings/zápis|Vytvoří nebo aktualizuje nastavení diagnostiky pro tento prostředek.|
|/iotHubs/skus/Read|Získání platných SKU IotHub|
|/iotHubs/Jobs/Read|Získání úloh Podrobnosti odeslaného na daný IotHub|
|/iotHubs/routingEndpointsHealth/Read|Získá stav všech směrování koncové body pro platformou IotHub|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Operace | Popis |
|---|---|
|/ Předplatné nebo registrace nebo akce|Zaregistruje předplatné|
|/Labs/DELETE|Odstraňte laboratoře.|
|/Labs/Read|Přečtěte si laboratoře.|
|/ labs a zápis|Přidat nebo změnit laboratoře.|
|/Labs/ListVhds/Action|Zobrazí seznam bitových kopií disku k dispozici pro vytvoření vlastní image.|
|/Labs/GenerateUploadUri/Action|Generovat identifikátor URI pro nahrávání vlastní diskové Image do testovacího prostředí.|
|/Labs/CreateEnvironment/Action|Vytvoření virtuálních počítačů v testovacím prostředí.|
|/Labs/ClaimAnyVm/Action|Deklarace identity náhodných vymahatelných virtuálního počítače v testovacím prostředí.|
|/Labs/ExportResourceUsage/Action|Exportuje využití prostředků testovacího prostředí do účtu úložiště|
|/Labs/Users/DELETE|Odstraňte profily uživatelů.|
|/Labs/Users/Read|Číst profily uživatelů.|
|/Labs/Users/Write|Přidat nebo upravit profily uživatelů.|
|/Labs/Users/secrets/DELETE|Odstraňte tajných klíčů.|
|/Labs/Users/secrets/Read|Čtení tajných klíčů.|
|/Labs/Users/secrets/Write|Přidat nebo změnit tajných klíčů.|
|/Labs/Users/Environments/DELETE|Odstraňte prostředí.|
|/Labs/Users/Environments/Read|Přečtěte si prostředí.|
|/Labs/Users/Environments/Write|Přidat nebo změnit prostředí.|
|/Labs/Users/Disks/DELETE|Odstraňte disky.|
|/Labs/Users/Disks/Read|Přečtěte si disky.|
|/Labs/Users/Disks/Write|Přidat nebo změnit disky.|
|/Labs/Users/Disks/Attach/Action|Připojit a vytvořit zapůjčení disku k virtuálnímu počítači.|
|/Labs/Users/Disks/Detach/Action|Odpojte a rozdělit zapůjčení disk připojený k virtuálnímu počítači.|
|/Labs/customImages/DELETE|Odstraňte vlastní Image.|
|/Labs/customImages/Read|Přečtěte si vlastních bitových kopií.|
|/Labs/customImages/Write|Přidat nebo upravit vlastní Image.|
|/Labs/serviceRunners/DELETE|Odstraňte závodníky služby.|
|/Labs/serviceRunners/Read|Přečtěte si závodníky služby.|
|/Labs/serviceRunners/Write|Přidat nebo změnit závodníky služby.|
|/Labs/artifactSources/DELETE|Odstraňte artefaktů zdroje.|
|/Labs/artifactSources/Read|Přečtěte si artefaktů zdroje.|
|/Labs/artifactSources/Write|Přidat nebo změnit artefaktů zdroje.|
|/Labs/artifactSources/artifacts/Read|Přečtěte si artefakty.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Generuje šablony ARM pro danou artefaktů, odešle požadovaných souborů do účtu úložiště a ověří generovaného artefaktů.|
|/Labs/artifactSources/armTemplates/Read|Přečtěte si šablony správce prostředků azure.|
|/Labs/costs/Read|Přečtěte si náklady.|
|/Labs/costs/Write|Přidat nebo změnit náklady.|
|/Labs/virtualNetworks/DELETE|Odstraňte virtuální sítě.|
|/Labs/virtualNetworks/Read|Čtení virtuální sítě.|
|/Labs/virtualNetworks/Write|Přidat nebo upravit virtuální sítě.|
|/Labs/Formulas/DELETE|Odstraňte vzorce.|
|/Labs/Formulas/Read|Přečtěte si vzorce.|
|/Labs/Formulas/Write|Přidat nebo upravit vzorce.|
|/Labs/Schedules/DELETE|Odstraňte plány.|
|/Labs/Schedules/Read|Plánuje pro čtení.|
|/Labs/Schedules/Write|Přidat nebo změnit plány.|
|/Labs/Schedules/Execute/Action|Spuštění plánu.|
|/Labs/Schedules/ListApplicable/Action|Zobrazí všechny příslušné plány|
|/Labs/galleryImages/Read|Přečtěte si Galerie obrázků.|
|/Labs/policySets/EvaluatePolicies/Action|Vyhodnotí zásady testovacího prostředí.|
|/Labs/policySets/Policies/DELETE|Odstraňte zásady.|
|/Labs/policySets/Policies/Read|Přečtěte si zásady.|
|/Labs/policySets/Policies/Write|Přidat nebo upravit zásady.|
|/Labs/virtualMachines/DELETE|Odstraňte virtuální počítače.|
|/Labs/virtualMachines/Read|Přečtěte si virtuálních počítačů.|
|/Labs/virtualMachines/Write|Umožňuje přidat nebo upravit virtuální počítače.|
|/Labs/virtualMachines/Start/Action|Spuštění virtuálního počítače.|
|/Labs/virtualMachines/stop/Action|Zastavit virtuální počítač|
|/Labs/virtualMachines/ApplyArtifacts/Action|Artefakty se vztahují na virtuální počítač.|
|/Labs/virtualMachines/AddDataDisk/Action|Připojte nové nebo stávající datový disk k virtuálnímu počítači.|
|/Labs/virtualMachines/DetachDataDisk/Action|Zadaný disk z virtuálního počítače odpojte.|
|/Labs/virtualMachines/Claim/Action|Převzetí vlastnictví existujícího virtuálního počítače|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Zobrazí všechny příslušné plány|
|/Labs/virtualMachines/Schedules/DELETE|Odstraňte plány.|
|/Labs/virtualMachines/Schedules/Read|Plánuje pro čtení.|
|/Labs/virtualMachines/Schedules/Write|Přidat nebo změnit plány.|
|/Labs/virtualMachines/Schedules/Execute/Action|Spuštění plánu.|
|/Labs/notificationChannels/DELETE|Odstraňte notificationchannels.|
|/Labs/notificationChannels/Read|Přečtěte si notificationchannels.|
|/Labs/notificationChannels/Write|Přidat nebo změnit notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Odeslat oznámení na zadané kanálu.|
|/Schedules/DELETE|Odstraňte plány.|
|/Schedules/Read|Plánuje pro čtení.|
|/ plány a zápis|Přidat nebo změnit plány.|
|/Schedules/Execute/Action|Spuštění plánu.|
|/Schedules/Retarget/Action|Aktualizace plán cílový prostředek ID.|
|/Locations/Operations/Read|Operace čtení.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Operace | Popis |
|---|---|
|/databaseAccountNames/Read|Zkontroluje dostupnost názvu.|
|/databaseAccounts/Read|Přečte databázového účtu.|
|/ databaseAccounts/zápisu|Aktualizace databáze účtů.|
|/databaseAccounts/listKeys/Action|Seznam klíčů účtu databáze|
|/databaseAccounts/regenerateKey/Action|Otočit klíče účtu databáze|
|/databaseAccounts/listConnectionStrings/Action|Získat připojovací řetězce pro databázový účet|
|/databaseAccounts/changeResourceGroup/Action|Změnit skupinu prostředků účtu databáze|
|/databaseAccounts/failoverPriorityChange/Action|Změnit priorit převzetí služeb při selhání oblastí databázového účtu. To se používá k provedení operace ruční převzetí služeb při selhání|
|/databaseAccounts/DELETE|Odstraní účty databáze.|
|/databaseAccounts/metricDefinitions/Read|Čte databázi definice metrik účtu.|
|/databaseAccounts/Metrics/Read|Přečte metriky účtu databáze.|
|/databaseAccounts/usages/Read|Čte databázi použití účtu.|
|/databaseAccounts/Databases/Collections/metricDefinitions/Read|Načte kolekci definice metrik.|
|/databaseAccounts/Databases/Collections/Metrics/Read|Načte kolekci metriky.|
|/databaseAccounts/Databases/Collections/usages/Read|Načte kolekci použití.|
|/databaseAccounts/Databases/metricDefinitions/Read|Čte databázi definice metrik|
|/databaseAccounts/Databases/Metrics/Read|Přečte metriky databáze.|
|/databaseAccounts/Databases/usages/Read|Přečte použití databáze.|
|/databaseAccounts/readonlykeys/Read|Čte databázi, klíče účtu jen pro čtení.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Operace | Popis |
|---|---|
|/ generateSsoRequest nebo akce|Generovat žádost o přihlášení k doméně řídicí centrum.|
|/ validateDomainRegistrationInformation nebo akce|Ověření objektu nákupu domény bez odeslání|
|/ checkDomainAvailability nebo akce|Zkontrolujte, jestli je možné zakoupit domény|
|/ listDomainRecommendations nebo akce|Načtení seznamu domény doporučení na základě klíčových slov|
|/ registrace nebo akce|Registrace poskytovatele prostředků Microsoft Domains pro předplatné|
|/ domény/čtení|Získat seznam domén|
|/ domény/zápisu|Přidat novou doménu nebo aktualizovat stávající|
|/ domény/odstranit|Odstraňte existující domény.|
|/Domains/operationresults/Read|Získat operace domény|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Operace | Popis |
|---|---|
|/lcsprojects/Read|Zobrazení služby životního cyklu aplikace Microsoft Dynamics projektů, které patří uživateli|
|/ lcsprojects/zápisu|Vytvářet a aktualizovat projekty služby Microsoft Dynamics životního cyklu, které patří uživateli. Lze aktualizovat pouze vlastnosti název a popis. Po vytvoření nelze aktualizovat předplatné a umístění přidružený k projektu|
|/lcsprojects/DELETE|Odstranění služby životního cyklu aplikace Microsoft Dynamics projekty, které patří uživateli|
|/lcsprojects/clouddeployments/Read|V projektu služby Microsoft Dynamics životního cyklu, které patří uživateli zobrazit Microsoft Dynamics AX 2012 R3 vyhodnocení nasazení|
|/lcsprojects/clouddeployments/Write|Vytvořte Microsoft Dynamics AX 2012 R3 vyhodnocení nasazení v projektu služby Microsoft Dynamics životního cyklu, které patří uživateli. Nasazení lze provádět z portálu správy Azure|
|/lcsprojects/Connectors/Read|Konektory pro čtení, které patří do služby Microsoft Dynamics životního cyklu projektu|
|/lcsprojects/Connectors/Write|Vytváření a aktualizaci konektory, které patří do služby Microsoft Dynamics životního cyklu projektu|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje předplatné pro poskytovatele prostředků EventHub a povolí vytváření prostředků EventHub.|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav Namespace jsou vlastnosti, které lze aktualizovat.|
|/namespaces/Read|Získá seznam popisů prostředku oboru názvů.|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/metricDefinitions/Read|Získání seznamu Namespace metrik popisy prostředků|
|/namespaces/authorizationRules/Read|Získá seznam popisů autorizačních pravidel oboru názvů.|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec k oboru názvů|
|/namespaces/authorizationRules/regenerateKeys/Action|Znovu vygeneruje primární nebo sekundární klíč k prostředku.|
|/namespaces/eventhubs/Write|Vytvoření nebo aktualizace EventHub vlastnosti.|
|/namespaces/eventhubs/Read|Získání seznamu popisů EventHub prostředků|
|/namespaces/eventhubs/DELETE|Operace odstranění EventHub prostředků|
|/namespaces/eventHubs/consumergroups/Write|Vytvoření nebo aktualizace ConsumerGroup vlastnosti.|
|/namespaces/eventHubs/consumergroups/Read|Získání seznamu popisů ConsumerGroup prostředků|
|/namespaces/eventHubs/consumergroups/DELETE|Operace odstranění ConsumerGroup prostředků|
|/namespaces/eventhubs/authorizationRules/Read| Získání seznamu EventHub autorizační pravidla|
|/namespaces/eventhubs/authorizationRules/Write|Centrum EventHub autorizační pravidla vytvořit a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/eventhubs/authorizationRules/DELETE|Operace odstranění EventHub autorizační pravidla|
|/namespaces/eventhubs/authorizationRules/listkeys/Action|Získat připojovací řetězec k centru EventHub|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/Action|Znovu vygeneruje primární nebo sekundární klíč k prostředku.|
|/namespaces/diagnosticSettings/Read|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/diagnosticSettings/Write|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/logDefinitions/Read|Získání seznamu protokolů Namespace popisy prostředků|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Operace | Popis |
|---|---|
|/providers/features/Read|Získá funkci předplatného v daném zprostředkovateli prostředků.|
|/providers/features/Register/Action|Registruje funkci pro předplatné v daném zprostředkovateli prostředků.|
|/features/Read|Získá funkce předplatného.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Operace | Popis |
|---|---|
|/ clustery a zápis|Vytvořit nebo aktualizovat HDInsight Cluster|
|/Clusters/Read|Získejte podrobnosti o clusteru HDInsight|
|/Clusters/DELETE|Odstranění clusteru HDInsight|
|/Clusters/changerdpsetting/Action|Změna nastavení protokolu RDP pro HDInsight Cluster|
|/Clusters/Configurations/Action|Aktualizace konfigurace clusteru HDInsight|
|/Clusters/Configurations/Read|Získání konfigurace clusteru HDInsight|
|/Clusters/Roles/Resize/Action|Změnit velikost clusteru HDInsight|
|/Locations/Capabilities/Read|Získat možnosti předplatného|
|/Locations/checkNameAvailability/Read|Zkontrolujte dostupnost názvu|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro poskytovatele prostředků importu a exportu a umožňuje vytvoření úlohy importu a exportu.|
|/ úlohy/zápisu|Vytvoří úlohu se zadanými parametry nebo aktualizovat vlastnosti a značky pro zadanou úlohu.|
|/Jobs/Read|Získá vlastnosti pro zadanou úlohu nebo vrátí seznam úloh.|
|/Jobs/listBitLockerKeys/Action|Získá klíče nástroje BitLocker pro zadanou úlohu.|
|/Jobs/DELETE|Odstraní stávající úloze.|
|/Locations/Read|Získá vlastnosti pro zadané umístění nebo vrátí seznam umístění.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Operace | Popis |
|---|---|
|/ Registrace nebo akce|Registrovat poskytovatele platformy Microsoft Insights|
|/ AlertRules/zápis|Zapisuje se do konfigurace pravidla výstrahy.|
|AlertRules nebo odstranění|Odstraňuje se konfigurace pravidla výstrahy.|
|/ AlertRules/čtení|Čte se konfigurace pravidla výstrahy.|
|/ AlertRules nebo aktivovat nebo akce|Pravidlo výstrahy, které jsou aktivované|
|/ AlertRules/vyřešit nebo akce|Pravidlo výstrahy vyřešil|
|/ AlertRules/omezeny nebo akce|Pravidlo výstrahy je omezen.|
|/ AlertRules/incidenty/čtení|Čte se konfigurace incidentu pravidla výstrahy.|
|/ MetricDefinitions/čtení|Číst definice metrik|
|/eventtypes/Values/Read|Číst hodnoty typů událostí správy|
|/eventtypes/digestevents/Read|Číst výtah typů událostí správy|
|/ Metriky/čtení|Číst metriky|
|/ LogProfiles/zápis|Zápis do protokolu konfigurace profilu|
|LogProfiles nebo odstranění|Odstranit konfiguraci profilů protokolu|
|/ LogProfiles/čtení|Profily protokolu pro čtení|
|/ AutoscaleSettings/zápis|Zapisuje se do konfigurace nastavení automatického škálování.|
|AutoscaleSettings nebo odstranění|Odstraňuje se konfigurace nastavení automatického škálování.|
|/ AutoscaleSettings/čtení|Čte se konfigurace nastavení automatického škálování.|
|/ AutoscaleSettings/Scaleup nebo akce|Operace automatického vertikálního navýšení kapacity|
|/ AutoscaleSettings/Scaledown nebo akce|Škálování vertikálně operace|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Číst definice metrik|
|/ ActivityLogAlerts nebo aktivovat nebo akce|Aktivoval výstrahu protokolu aktivit|
|/ DiagnosticSettings/zápis|Zapisuje se do konfiguračního nastavení diagnostiky.|
|DiagnosticSettings nebo odstranění|Odstraňuje se konfigurace nastavení pro diagnostiku|
|/ DiagnosticSettings/čtení|Čte se konfigurační nastavení diagnostiky.|
|/ LogDefinitions/čtení|Číst definice protokolu|
|/ ExtendedDiagnosticSettings/zápis|Zápis do konfigurace rozšířených nastavení diagnostiky|
|ExtendedDiagnosticSettings nebo odstranění|Odstraňuje se konfigurace rozšířených nastavení diagnostiky|
|/ ExtendedDiagnosticSettings/čtení|Čtení konfigurace rozšířených nastavení diagnostiky|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné|
|/checkNameAvailability/Read|Zkontroluje, jestli je název trezoru klíčů platný a jestli se nepoužívá.|
|/vaults/Read|Zobrazí vlastnosti trezoru klíčů.|
|/ trezory a zápis|Vytvoří nový trezor klíčů nebo aktualizuje vlastnosti existujícího trezoru klíčů.|
|/vaults/DELETE|Odstranění trezoru klíčů|
|/vaults/Deploy/Action|Umožňuje přístup k tajných klíčů v trezoru klíčů při nasazování prostředků Azure|
|/vaults/secrets/Read|Zobrazí vlastnosti tajného kódu, ale ne jeho hodnotu.|
|/vaults/secrets/Write|Vytvoří nový tajný kód nebo aktualizuje hodnotu existujícího tajného kódu.|
|/vaults/accessPolicies/Write|Existující zásady přístupu můžete aktualizovat sloučením nebo nahrazením, případně můžete do trezoru přidat nové zásady přístupu.|
|/deletedVaults/Read|Zobrazení vlastností logicky odstraněné trezorů klíčů|
|/Locations/operationResults/Read|Zkontrolujte výsledek operace dlouho spustit|
|/Locations/deletedVaults/Read|Zobrazení vlastností logicky odstraněné trezoru klíčů|
|/Locations/deletedVaults/PURGE/Action|Vyprázdnění logicky odstraněné trezoru klíčů|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Operace | Popis |
|---|---|
|/workflows/Read|Načte pracovní postup.|
|/ pracovní postupy a zápis|Vytvoří nebo aktualizuje pracovní postup.|
|/workflows/DELETE|Odstraní pracovní postup.|
|/workflows/Run/Action|Spustí běh pracovního postupu.|
|/workflows/disable/Action|Zakáže pracovní postup.|
|/workflows/enable/Action|Povolí pracovní postup.|
|/workflows/Validate/Action|Ověří pracovní postup.|
|/workflows/Move/Action|Přesune pracovní postup z existujícího ID předplatného, skupiny prostředků a/nebo názvu na jiné ID předplatného, skupinu prostředků nebo název.|
|/workflows/listSwagger/Action|Získá swagger definice pracovního postupu.|
|/workflows/regenerateAccessKey/Action|Obnoví tajné přístupové klíče.|
|/workflows/listCallbackUrl/Action|Získá adresu URL zpětné volání pro pracovní postup.|
|/workflows/versions/Read|Načte verzi pracovního postupu.|
|/workflows/versions/Triggers/listCallbackUrl/Action|Získá adresu URL zpětné volání pro aktivační událost.|
|/ pracovních/spouští/číst|Načte běh pracovního postupu.|
|/workflows/runs/Cancel/Action|Zruší běh pracovního postupu.|
|/workflows/runs/Actions/Read|Načte akci běhu pracovního postupu.|
|/workflows/runs/Operations/Read|Načte stav operace běhu pracovního postupu.|
|/workflows/Triggers/Read|Načte trigger.|
|/workflows/Triggers/Run/Action|Spustí trigger.|
|/workflows/Triggers/listCallbackUrl/Action|Získá adresu URL zpětné volání pro aktivační událost.|
|/workflows/Triggers/histories/Read|Načte historie triggerů.|
|/workflows/Triggers/histories/resubmit/Action|Znovu odešle aktivační událost pracovního postupu.|
|/workflows/accessKeys/Read|Načte přístupový klíč.|
|/workflows/accessKeys/Write|Vytvoří nebo aktualizuje přístupový klíč.|
|/workflows/accessKeys/DELETE|Odstraní přístupový klíč.|
|/workflows/accessKeys/list/Action|Zobrazí seznam tajných přístupových klíčů.|
|/workflows/accessKeys/regenerate/Action|Obnoví tajné přístupové klíče.|
|/Locations/workflows/Validate/Action|Ověří pracovní postup.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro strojové učení zprostředkovatel prostředků webové služby a povolí vytvoření webových služeb.|
|/ webovým službám nebo akce|Vytvořit místní vlastnosti webové služby pro podporované oblasti|
|/commitmentPlans/Read|Číst všechny strojového učení závazků plán|
|/ commitmentPlans/zápisu|Vytvořit nebo aktualizovat každého Machine Learning závazků plánu|
|/commitmentPlans/DELETE|Odstranit všechny strojového učení závazků plán|
|/commitmentPlans/JOIN/Action|Připojte všechny strojového učení závazků plán|
|/commitmentPlans/commitmentAssociations/Read|Číst všechny strojového učení závazků přidružení plánu|
|/commitmentPlans/commitmentAssociations/Move/Action|Přesunout všechny strojového učení závazků přidružení plánu|
|/ Pracovních prostorů/čtení|Číst všechny strojového učení pracovního prostoru|
|A pracovní prostory/zápis|Vytvořit nebo aktualizovat všechny strojového učení pracovního prostoru|
|Pracovních prostorů a odstranění|Odstranit všechny strojového učení pracovního prostoru|
|/ Pracovních prostorů nebo listworkspacekeys nebo akce|Seznam klíčů pro pracovní prostor Machine Learning|
|/ Pracovních prostorů nebo resyncstoragekeys nebo akce|Nové synchronizace klíče účtu úložiště, které jsou nakonfigurované pro pracovní prostor Machine Learning|
|/webServices/Read|Číst všechny webová služba Machine Learning|
|/ webovým službám a zápis|Vytvořit nebo aktualizovat žádné webové služby Machine Learning|
|/webServices/DELETE|Odstranit všechny webová služba Machine Learning|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Operace | Popis |
|---|---|
|/agreements/OFFERS/plans/Read|Vrátí smlouvu pro položku daného webu marketplace|
|/agreements/OFFERS/plans/Sign/Action|Podepsat smlouvu pro položku daného webu marketplace|
|/agreements/OFFERS/plans/Cancel/Action|Zrušit smlouvu pro položku daného webu marketplace|

## <a name="microsoftmedia"></a>Microsoft.Media

| Operace | Popis |
|---|---|
|/mediaservices/Read||
|/ mediaservices/zápisu||
|/mediaservices/DELETE||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné|
|nebo zrušit registraci nebo akce|Zruší registraci předplatného|
|/ checkTrafficManagerNameAvailability nebo akce|Zkontroluje dostupnost názvu DNS relativní Traffic Manageru.|
|/dnszones/Read|Umožňuje načíst zónu DNS, ve formátu JSON. Vlastnosti zóny zahrnují značky, etag, numberOfRecordSets a maxNumberOfRecordSets. Všimněte si, že tento příkaz nenačítá sady záznamů v rámci zóny.|
|/ dnszones/zápisu|Vytvořit nebo aktualizovat zónu DNS v rámci skupiny prostředků.  Použít k aktualizaci značek u prostředku zóny DNS. Všimněte si, že tento příkaz nelze použít k vytvoření nebo aktualizaci sad záznamů v rámci zóny.|
|/dnszones/DELETE|Odstraňte zónu DNS, ve formátu JSON. Vlastnosti zóny zahrnují značky, etag, numberOfRecordSets a maxNumberOfRecordSets.|
|/dnszones/MX/Read|Načíst sadu záznamů typu "MX", ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/MX/Write|Vytvořit nebo aktualizovat sadu záznamů typu "MX" v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/MX/DELETE|Odebrat sadu záznamů daného názvu a typu "MX" ze zóny DNS.|
|/dnszones/NS/Read|Získá DNS sadu záznamů typu NS|
|/dnszones/NS/Write|Vytvoří nebo aktualizuje DNS sadu záznamů typu NS|
|/dnszones/NS/DELETE|Umožňuje odstranit sadu záznamů typu NS DNS|
|/dnszones/AAAA/Read|Načíst sadu záznamů typu 'AAAA, ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/AAAA/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'AAAA, v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/AAAA/DELETE|Odebrat sadu záznamů daného názvu a typu 'AAAA, umožňuje ze zóny DNS.|
|/dnszones/CNAME/Read|Načíst sadu záznamů typu 'CNAME' ve formátu JSON. Sada záznamů obsahuje TTL, značky a etag.|
|/dnszones/CNAME/Write|Vytvořit nebo aktualizovat sadu záznamů typu, CNAME, v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/CNAME/DELETE|Odebrat sadu záznamů daného názvu a typu, CNAME, umožňuje ze zóny DNS.|
|/dnszones/SOA/Read|Získá sadu záznamů DNS typu SOA|
|/dnszones/SOA/Write|Vytvoří nebo aktualizuje DNS sadu záznamů typu SOA|
|/dnszones/SRV/Read|Načíst sadu záznamů typu 'SRV' ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/SRV/Write|Vytvořit nebo aktualizovat sadu záznamů typu SRV|
|/dnszones/SRV/DELETE|Odebrat sadu záznamů daného názvu a typu SRV "služby" ze zóny DNS.|
|/dnszones/PTR/Read|Načíst sadu záznamů typu 'PTR, ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/PTR/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'PTR, v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/PTR/DELETE|Odebrat sadu záznamů daného názvu a typu 'PTR, umožňuje ze zóny DNS.|
|/dnszones/A/Read|Načíst sadu záznamů typu "A", ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/A/Write|Vytvořit nebo aktualizovat sadu záznamů typu "A" v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/A/DELETE|Odebrat sadu záznamů daného názvu a typu "A" ze zóny DNS.|
|/dnszones/txt/Read|Načíst sadu záznamů typu 'TXT, ve formátu JSON. Sada záznamů obsahuje seznam záznamů, jakož i TTL, značky a etag.|
|/dnszones/txt/Write|Vytvořit nebo aktualizovat sadu záznamů typu 'TXT, v rámci zóny DNS. Zadané záznamy nahradí aktuální záznamy v sadě záznamů.|
|/dnszones/txt/DELETE|Odebrat sadu záznamů daného názvu a typu 'TXT, umožňuje ze zóny DNS.|
|/dnszones/recordsets/Read|Získá sady záznamů DNS mezi typy|
|/networkInterfaces/Read|Získá definici rozhraní sítě. |
|/ networkInterfaces/zápisu|Vytvoří rozhraní sítě nebo aktualizuje existující rozhraní sítě. |
|/networkInterfaces/JOIN/Action|Virtuální počítač připojí k síťovému rozhraní|
|/networkInterfaces/DELETE|Odstraní rozhraní sítě|
|/networkInterfaces/effectiveRouteTable/Action|Získejte tabulku směrování nakonfigurovaný v síťovém rozhraní virtuálního počítače|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Získat skupiny zabezpečení sítě nakonfigurované na síťové rozhraní z virtuálního počítače|
|/networkInterfaces/loadBalancers/Read|Získá všech služeb Vyrovnávání zatížení, které je součástí rozhraní sítě|
|/networkInterfaces/ipconfigurations/Read|Získá definici konfigurace ip rozhraní sítě. |
|/publicIPAddresses/Read|Získá definici veřejné ip adresy.|
|/ publicipaddresses, na které a zápis|Vytvoří veřejnou Ip adresu nebo aktualizuje existující veřejnou Ip adresu. |
|/publicIPAddresses/DELETE|Odstraní veřejnou Ip adresu.|
|/publicIPAddresses/JOIN/Action|Spojí veřejnou ip adresu.|
|/routeFilters/Read|Získá definici filtru trasy|
|/routeFilters/JOIN/Action|Spojí filtr trasy|
|/routeFilters/DELETE|Odstraní definici směrovací filtru|
|/ routeFilters/zápisu|Vytvoří filtr trasu nebo aktualizuje existující směrovací filtr|
|/routeFilters/Rules/Read|Získá definici pravidla filtru trasy|
|/routeFilters/Rules/Write|Vytvoří pravidlo filtru trasu nebo aktualizuje existující pravidlo filtru trasy|
|/routeFilters/Rules/DELETE|Odstraní definici směrovací filtr pravidlo|
|/networkWatchers/Read|Získat definici sledovací proces sítě|
|/ networkWatchers/zápisu|Sledovací proces sítě vytvoří nebo aktualizuje existující sledovací proces sítě|
|/networkWatchers/DELETE|Odstraní sledovací proces sítě|
|/networkWatchers/configureFlowLog/Action|Nakonfiguruje toku protokolování pro cílový prostředek.|
|/networkWatchers/ipFlowVerify/Action|Vrátí, jestli je paket povolený nebo zakázaný do nebo z konkrétní cíl.|
|/networkWatchers/nextHop/Action|U zadaného cíle a cílové IP adresy vrátí typ dalšího směrování a další naděje IP adresu.|
|/networkWatchers/queryFlowLogStatus/Action|Získá stav toku protokolování na prostředku.|
|/networkWatchers/queryTroubleshootResult/Action|Získá řešení potíží výsledek z dříve spuštění nebo aktuálně spuštěna operace odstraňování potíží.|
|/networkWatchers/securityGroupView/Action|Zobrazení pravidel skupiny zabezpečení sítě nakonfigurované a efektivní použita na virtuálním počítači.|
|/networkWatchers/Topology/Action|Získá úrovně zobrazení síťových prostředků a jejich vztahů ve skupině prostředků.|
|/networkWatchers/troubleshoot/Action|Spustí se Poradce při potížích s na prostředku sítě v Azure.|
|/networkWatchers/packetCaptures/queryStatus/Action|Získá informace o vlastnostech a stav prostředku zachytávání paketů.|
|/networkWatchers/packetCaptures/stop/Action|Zastavte spuštěné relaci zachytávání paketů.|
|/networkWatchers/packetCaptures/Read|Získat definici zachytávání paketů|
|/networkWatchers/packetCaptures/Write|Vytvoří zachytávání paketů|
|/networkWatchers/packetCaptures/DELETE|Odstraní zachytávání paketů|
|/loadBalancers/Read|Získá definici nástroje pro vyrovnávání zatížení.|
|/ loadBalancers/zápisu|Vytvoří nástroj pro vyrovnávání zatížení nebo aktualizuje existující pro vyrovnávání zatížení|
|/loadBalancers/DELETE|Odstraní nástroj pro vyrovnávání zatížení|
|/loadBalancers/networkInterfaces/Read|Získá odkazy na rozhraní sítě pod nástrojem pro vyrovnávání zatížení|
|/loadBalancers/loadBalancingRules/Read|Získá zatížení nástroje pro vyrovnávání zatížení vyrovnávání definici pravidla|
|/loadBalancers/backendAddressPools/Read|Získá definici fondu adres back-end zatížení nástroje pro vyrovnávání|
|/loadBalancers/backendAddressPools/JOIN/Action|Spojí fond back-end adresy Vyrovnávání zatížení.|
|/loadBalancers/inboundNatPools/Read|Získá Vyrovnávání zatížení příchozí nat definici fondu|
|/loadBalancers/inboundNatPools/JOIN/Action|Spojí Vyrovnávání zatížení fond příchozích pravidel nat|
|/loadBalancers/inboundNatRules/Read|Nástroj pro vyrovnávání zatížení získá definici příchozího pravidla nat|
|/loadBalancers/inboundNatRules/Write|Vytvoří pravidlo příchozí nat nástroje pro vyrovnávání zatížení nebo aktualizuje existující pravidlo příchozí nat nástroje pro vyrovnávání zatížení|
|/loadBalancers/inboundNatRules/DELETE|Odstraní pravidlo příchozí nat nástroje pro vyrovnávání zatížení.|
|/loadBalancers/inboundNatRules/JOIN/Action|Spojí pravidlo příchozí nat nástroje pro vyrovnávání zatížení.|
|/loadBalancers/outboundNatRules/Read|Získá definici pravidla odchozí nat nástroje pro vyrovnávání zatížení|
|/loadBalancers/probes/Read|Získá sondu nástroje pro vyrovnávání zatížení|
|/loadBalancers/virtualMachines/Read|Získá odkazy na všechny virtuální počítače pod nástrojem pro vyrovnávání zatížení|
|/loadBalancers/frontendIPConfigurations/Read|Získá definice konfigurace IP front-endu nástroje pro vyrovnávání zatížení|
|/trafficManagerGeographicHierarchies/Read|Získá obsahující oblastí, které se dají použít s metodu směrování provozu geografické hierarchii Geographic Traffic Manager|
|/bgpServiceCommunities/Read|Získat komunit protokolu Bgp služby|
|/applicationGatewayAvailableWafRuleSets/Read|Získá aplikační bránu, kterou sad pravidel k dispozici firewall webových aplikací|
|/virtualNetworks/Read|Získat definici virtuální sítě|
|/ virtualNetworks/zápisu|Vytvoří virtuální síť nebo aktualizuje existující virtuální síť|
|/virtualNetworks/DELETE|Odstraní virtuální sítě|
|/virtualNetworks/peer/Action|Partnerský vztah virtuální síť s jinou virtuální sítí|
|/virtualNetworks/virtualNetworkPeerings/Read|Získá definici partnerského vztahu virtuální sítě|
|/virtualNetworks/virtualNetworkPeerings/Write|Vytvoří partnerský vztah virtuální sítě nebo aktualizuje existující virtuální síť partnerský vztah|
|/virtualNetworks/virtualNetworkPeerings/DELETE|Odstraní virtuální sítě partnerského vztahu|
|/virtualNetworks/subnets/Read|Získá definici podsítě virtuální sítě|
|/virtualNetworks/subnets/Write|Vytvoří podsíť virtuální sítě nebo aktualizuje existující podsíť virtuální sítě|
|/virtualNetworks/subnets/DELETE|Odstraní podsíť virtuální sítě|
|/virtualNetworks/subnets/JOIN/Action|Připojí virtuální sítě|
|/virtualNetworks/subnets/joinViaServiceTunnel/Action|Spojí prostředků, jako je například účet úložiště nebo SQL database k podsíti tunelování služby povolena.|
|/virtualNetworks/subnets/virtualMachines/Read|Získá odkazy na všechny virtuální počítače v podsíti virtuální sítě|
|/virtualNetworks/checkIpAddressAvailability/Read|Zkontrolujte, jestli je k dispozici v zadané virtuální síti Ip adresy|
|/virtualNetworks/virtualMachines/Read|Získá odkazy na všechny virtuální počítače ve virtuální síti|
|/expressRouteServiceProviders/Read|Získá poskytovatele služeb Expressroute|
|/dnsoperationresults/Read|Získá výsledky operace DNS|
|/localnetworkgateways/Read|Získá LocalNetworkGateway|
|/ localnetworkgateways/zápisu|Vytvoří nebo aktualizuje existující LocalNetworkGateway|
|/localnetworkgateways/DELETE|Odstraní LocalNetworkGateway|
|/trafficManagerProfiles/Read|Získání konfigurace profilu Správce provozu. To zahrnuje nastavení DNS, nastavení směrování provozu, nastavení monitorování koncového bodu a seznam koncových bodů směrovaných tímto profilem Traffic Manager.|
|/ trafficManagerProfiles/zápisu|Vytvoření profilu Správce provozu nebo úprava konfigurace existujícího profilu Správce provozu. To zahrnuje zapnutí nebo vypnutí profilu a změna nastavení DNS, nastavení směrování provozu nebo nastavení monitorování koncového bodu. Koncových bodů směrovaných tímto profilem správce provozu můžete přidat, odebrat, povolit nebo zakázat.|
|/trafficManagerProfiles/DELETE|Odstraňte profil služby Traffic Manager. Všechna nastavení spojená s profilem správce provozu se ztratí a profil slouží už pro směrování provozu.|
|/dnsoperationstatuses/Read|Získá stav operace DNS |
|/Operations/Read|Získat dostupné operace|
|/expressRouteCircuits/Read|Získat ExpressRouteCircuit|
|/ expressRouteCircuits/zápisu|Vytvoří nebo aktualizuje existující ExpressRouteCircuit|
|/expressRouteCircuits/DELETE|Odstraní ExpressRouteCircuit|
|/expressRouteCircuits/stats/Read|Získá ExpressRouteCircuit Stat|
|/expressRouteCircuits/peerings/Read|Získá partnerský vztah ExpressRouteCircuit|
|/expressRouteCircuits/peerings/Write|Vytvoří nebo aktualizuje existující ExpressRouteCircuit partnerský vztah|
|/expressRouteCircuits/peerings/DELETE|Odstraní partnerský vztah ExpressRouteCircuit|
|/expressRouteCircuits/peerings/arpTables/Action|Získá ArpTable ExpressRouteCircuit partnerského vztahu|
|/expressRouteCircuits/peerings/routeTables/Action|Získá RouteTable ExpressRouteCircuit partnerského vztahu|
|/expressRouteCircuits/partnerských vztahů /<br>routeTablesSummary nebo akce|Získá ExpressRouteCircuit partnerský vztah RouteTable souhrn|
|/expressRouteCircuits/peerings/stats/Read|Získá ExpressRouteCircuit partnerský vztah Statistika|
|/expressRouteCircuits/authorizations/Read|Získá povolení ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/Write|Vytvoří nebo aktualizuje existující povolení ExpressRouteCircuit|
|/expressRouteCircuits/authorizations/DELETE|Odstraní ExpressRouteCircuit povolení|
|/Connections/Read|Získá VirtualNetworkGatewayConnection|
|/ připojení a zápis|Vytvoří nebo aktualizuje existující VirtualNetworkGatewayConnection|
|/Connections/DELETE|Odstraní VirtualNetworkGatewayConnection|
|/Connections/sharedKey/Read|Získá VirtualNetworkGatewayConnection SharedKey|
|/Connections/sharedKey/Write|Vytvoří nebo aktualizuje existující VirtualNetworkGatewayConnection SharedKey|
|/networkSecurityGroups/Read|Získá definici skupiny zabezpečení sítě|
|/ skupin zabezpečení sítě a zápis|Vytvoří skupinu zabezpečení sítě nebo aktualizuje existující skupinu zabezpečení sítě|
|/networkSecurityGroups/DELETE|Odstraní skupinu zabezpečení sítě|
|/networkSecurityGroups/JOIN/Action|Spojí skupinu zabezpečení sítě|
|/networkSecurityGroups/defaultSecurityRules/Read|Získá definici pravidla zabezpečení výchozí|
|/networkSecurityGroups/securityRules/Read|Získá definici pravidla zabezpečení|
|/networkSecurityGroups/securityRules/Write|Vytvoří pravidlo zabezpečení nebo aktualizuje existující pravidlo zabezpečení.|
|/networkSecurityGroups/securityRules/DELETE|Odstraní pravidlo zabezpečení|
|/applicationGateways/Read|Získá aplikační bránu.|
|/ applicationGateways/zápisu|Služby application gateway vytvoří nebo aktualizuje aplikační bránu.|
|/applicationGateways/DELETE|Odstraní aplikační bránu.|
|/applicationGateways/backendhealth/Action|Získá stavu back-end brány aplikaci|
|/applicationGateways/Start/Action|Spuštění služby application gateway|
|/applicationGateways/stop/Action|Zastavení služby application gateway|
|/applicationGateways/backendAddressPools/JOIN/Action|Spojí fond adres back-end brány aplikace|
|/routeTables/Read|Získá definici směrovací tabulky|
|/ routeTables/zápisu|Vytvoří směrovací tabulku nebo aktualizuje existující směrovací tabulku.|
|/routeTables/DELETE|Odstraní definici směrovací tabulky|
|/routeTables/JOIN/Action|Spojí směrovací tabulku|
|/routeTables/Routes/Read|Získá definici trasy|
|/routeTables/Routes/Write|Vytvoří trasu nebo aktualizuje existující trasu.|
|/routeTables/Routes/DELETE|Odstraní definici směrovací|
|/Locations/operationResults/Read|Získá výsledek operace asynchronní POST nebo operace odstranění|
|/Locations/checkDnsNameAvailability/Read|Zkontroluje, jestli je popisek dns k dispozici v zadaném umístění|
|/Locations/usages/Read|Získá metriky využití prostředků|
|/Locations/Operations/Read|Získá prostředek operace, který reprezentuje stav asynchronní operace|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné u poskytovatele prostředků NotificationHubs a povolí vytváření oborů názvů a NotificationHubs.|
|/ CheckNamespaceAvailability nebo akce|Zkontroluje, jestli je ve službě NotificationHub k dispozici název prostředku oboru názvů, nebo ne.|
|A obory názvů/zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav Namespace jsou vlastnosti, které lze aktualizovat.|
|/ Obory názvů/čtení|Získá seznam popisů prostředku oboru názvů.|
|Obory názvů nebo odstranění|Odstranit prostředek Namespace|
|/ Obory názvů nebo authorizationRules nebo akce|Získá seznam popisů autorizačních pravidel oboru názvů.|
|/ Obory názvů nebo CheckNotificationHubAvailability nebo akce|Zkontroluje, jestli je v oboru názvů k dispozici název NotificationHub, nebo ne.|
|A obory názvů nebo authorizationRules/zápis|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/ Obory názvů nebo authorizationRules/čtení|Získá seznam popisů autorizačních pravidel oboru názvů.|
|Obory názvů, authorizationRules nebo odstranění|Odstraňte Namespace autorizační pravidlo. Výchozí Namespace autorizační pravidlo nelze odstranit. |
|/ Obory názvů nebo authorizationRules/listkeys nebo akce|Získat připojovací řetězec k oboru názvů|
|/ Obory názvů nebo authorizationRules/regenerateKeys nebo akce|Autorizační pravidlo oboru názvů – opětovné vygenerování primárního nebo sekundárního klíče. Zadejte klíč, který je nutné znovu vygenerovat.|
|A obory názvů nebo NotificationHubs/zápis|Vytvoření centra oznámení a aktualizujte jeho vlastnosti. Jeho vlastnosti především zahrnovat přihlašovací údaje systému PNS. Autorizační pravidla a hodnota TTL|
|/ Obory názvů nebo NotificationHubs/čtení|Získá seznam popisů prostředků Centra oznámení.|
|Obory názvů, NotificationHubs nebo odstranění|Odstranit prostředek centra oznámení|
|/ Obory názvů nebo NotificationHubs/authorizationRules nebo akce|Získá seznam autorizačních pravidel Centra oznámení.|
|/ Obory názvů nebo NotificationHubs/pnsCredentials nebo akce|Získáte všechna pověření systému PNS centra oznámení. To zahrnuje, přihlašovací údaje WNS, MPNS, APNS, GCM a Baidu|
|/ Obory názvů nebo NotificationHubs/debugSend nebo akce|Poslat testovací nabízené oznámení|
|/ Obory názvů nebo NotificationHubs/metricDefinitions/čtení|Získání seznamu Namespace metrik popisy prostředků|
|/Namespaces/NotificationHubs /<br>authorizationRules a zápis|Vytvoření pravidla autorizace centra oznámení a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/Namespaces/NotificationHubs /<br>authorizationRules/čtení|Získá seznam autorizačních pravidel Centra oznámení.|
|/Namespaces/NotificationHubs /<br>authorizationRules nebo odstranění|Odstranit autorizační pravidla Centra oznámení|
|/Namespaces/NotificationHubs /<br>authorizationRules/listkeys nebo akce|Získá připojovací řetězec k Centru oznámení.|
|/Namespaces/NotificationHubs /<br>authorizationRules/regenerateKeys nebo akce|Autorizační pravidlo Centra oznámení – opětovné vygenerování primárního nebo sekundárního klíče. Zadejte klíč, který je nutné znovu vygenerovat.|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrujte předplatné zprostředkovatele prostředků.|
|/linkTargets/Read|Zobrazí seznam stávajících účtů, které nejsou přidružené předplatné Azure. Toto předplatné odkaz na pracovním prostoru, použijte id zákazníka vrácená touto operací ve vlastnosti id zákazníka operace vytvořit pracovní prostor.|
|/ pracovních prostorů a zápis|Vytvoří nový pracovní prostor nebo odkazy k existujícímu pracovnímu prostoru tím, že poskytuje id zákazníka z existující pracovního prostoru.|
|/workspaces/Read|Získá existujícímu pracovnímu prostoru|
|/workspaces/DELETE|Odstraní pracovního prostoru. Pokud byl pracovním prostoru propojený k existujícímu pracovnímu prostoru v okamžiku vytvoření není odstranit pracovní prostor, který byl propojen s.|
|/workspaces/generateregistrationcertificate/Action|Generuje registrační certifikát pro pracovní prostor. Tento certifikát se používá k připojení do pracovního prostoru Microsoft System Center Operation Manager.|
|/workspaces/sharedKeys/Action|Načte se sdílené klíče pro pracovní prostor. Tyto klíče se používají k připojení agentů statistice provozu Microsoft do pracovního prostoru.|
|/workspaces/Search/Action|Provede vyhledávací dotaz.|
|/workspaces/Datasources/Read|Získáte zdrojů dat v pracovním prostoru.|
|/workspaces/Datasources/Write|Vytvořit nebo aktualizovat zdroje dat v pracovním prostoru.|
|/workspaces/Datasources/DELETE|Odstraňte zdrojů dat v pracovním prostoru.|
|/workspaces/managementGroups/Read|Získá názvy a metadat pro připojení k tomuto pracovnímu prostoru skupiny pro správu System Center Operations Manager.|
|/workspaces/Schema/Read|Získá schéma vyhledávání pro pracovní prostor.  Hledání schématu zahrnuje zveřejněné pole a jejich typy.|
|/workspaces/usages/Read|Získá data o využití pro pracovní prostor, včetně množství dat číst v pracovním prostoru.|
|/workspaces/intelligencepacks/Read|Obsahuje seznam všech intelligence Pack, které jsou viditelné pro danou worksapce a také uvádí, jestli je sada povolit nebo zakázat pracovního prostoru.|
|/workspaces/intelligencepacks/enable/Action|Umožňuje intelligence pack pro daný pracovní prostor.|
|/workspaces/intelligencepacks/disable/Action|Zakáže intelligence pack pro daný pracovní prostor.|
|/workspaces/sharedKeys/Read|Načte se sdílené klíče pro pracovní prostor. Tyto klíče se používají k připojení agentů statistice provozu Microsoft do pracovního prostoru.|
|/workspaces/savedSearches/Read|Získá uložené vyhledávací dotaz.|
|/workspaces/savedSearches/Write|Vytvoří uložené vyhledávací dotaz.|
|/workspaces/savedSearches/DELETE|Odstraní uložené vyhledávací dotaz.|
|/workspaces/storageinsightconfigs/Write|Vytvoří novou konfiguraci úložiště. Tyto konfigurace se používají k načítání dat z umístění, do stávající účet úložiště.|
|/workspaces/storageinsightconfigs/Read|Získá konfiguraci úložiště.|
|/workspaces/storageinsightconfigs/DELETE|Odstraní konfigurace úložiště. To se zastaví statistice provozu Microsoft z čtení dat z účtu úložiště.|
|/workspaces/configurationScopes/Read|Získání konfigurace oboru|
|/workspaces/configurationScopes/Write|Nastavení konfigurace oboru|
|/workspaces/configurationScopes/DELETE|Odstranit konfigurace oboru|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registrujte předplatné zprostředkovatele prostředků.|
|/ řešení a zápis|Vytvořte nové řešení OMS|
|/Solutions/Read|Získat ukončení OMS řešení|
|/Solutions/DELETE|Odstraňte existující řešení OMS|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Operace | Popis |
|---|---|
|/ Trezory/backupJobsExport nebo akce|Export úloh|
|A trezory/zápis|Operace Vytvořit trezor vytvoří prostředek Azure typu trezor.|
|/ Trezory/čtení|Operace získání trezoru získá objekt, který reprezentuje prostředků Azure typu 'trezoru.|
|Trezory nebo odstranění|Operace odstranění trezoru Odstraní zadaný prostředek Azure typu 'trezoru.|
|/ Trezory/refreshContainers/čtení|Aktualizuje seznam kontejneru|
|/ Trezory/backupJobsExport/operationResults/čtení|Vrátí výsledek operace úlohy exportu.|
|/ Trezory/backupOperationResults/čtení|Vrátí výsledek operace zálohování trezoru Recovery Services.|
|/ Trezory/monitoringAlerts/čtení|Získá výstrahy pro trezor služeb zotavení.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / číst|Získá podrobnosti výstrahy.|
|/ Trezory/backupSecurityPIN/čtení|Vrátí zabezpečení PIN kódu informace pro obnovení služeb trezoru.|
|/vaults/replicationEvents/Read|Číst všechny události|
|/ Trezory/backupProtectableItems/čtení|Vrátí seznam chránitelných položek.|
|/vaults/replicationFabrics/Read|Číst všechny prostředky infrastruktury|
|/vaults/replicationFabrics/Write|Vytvořit nebo aktualizovat všechny prostředky infrastruktury|
|/vaults/replicationFabrics/Remove/Action|Odebrání infrastruktury|
|/vaults/replicationFabrics/checkConsistency/Action|Zkontroluje konzistenci prostředí infrastruktury.|
|/vaults/replicationFabrics/DELETE|Odstranit všechny prostředky infrastruktury|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Nasadit Image procesového serveru|
|/vaults/replicationFabrics/reassociateGateway/Action|Přidružení brány|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>Pro čtení|Přečtěte si zprostředkovatelů služby obnovení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>odebrat nebo akce|Odebrat zprostředkovatele služeb zotavení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>Odstranit|Odstranit zprostředkovatelů služby obnovení|
|/ trezory/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider nebo akce|Aktualizujte zprostředkovatele|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Číst všechny klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/čtení|Číst veškerá jeho mapování klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování klasifikace úložiště|
|/ trezory/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings nebo odstranění|Odstranit veškerá jeho mapování klasifikace úložiště|
|/vaults/replicationFabrics/replicationvCenters/Read|Číst všechny úlohy|
|/vaults/replicationFabrics/replicationvCenters/Write|Vytvořit nebo aktualizovat všechny úlohy|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Odstranit všechny úlohy|
|/vaults/replicationFabrics/replicationNetworks/Read|Přečtěte si žádné sítě.|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/čtení|Číst veškerá jeho mapování sítě|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování sítě|
|/ trezory/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings nebo odstranění|Odstranit všechny mapování sítě|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>Pro čtení|Číst všechny kontejnery ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem nebo akce|Zjistit Chránitelná položka|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>zápis|Vytvořit nebo aktualizovat všechny kontejnery ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>odebrat nebo akce|Kontejner ochrany odebrat.|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>switchprotection nebo akce|Kontejner ochrany přepínače|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems/čtení|Číst všechny položky, které jsou předmětem ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/čtení|Číst veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings a zápis|Vytvořit nebo aktualizovat veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings, odebrat nebo akce|Odebrat mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings nebo odstranění|Odstranit veškerá jeho mapování kontejnerů ochrany|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/čtení|Číst všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems a zápis|Vytvořit nebo aktualizovat všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems nebo odstranění|Odstranit všechny chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems, odebrat nebo akce|Odebrat chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/plannedFailover nebo akce|Plánované převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/unplannedFailover nebo akce|Převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailover nebo akce|Testovací převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/testFailoverCleanup nebo akce|Vyčistit testovací převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/failoverCommit nebo akce|Potvrzení převzetí služeb při selhání|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems nebo opětovné ochrany nebo akce|Znovu aktivujte ochranu chráněné položky|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/updateMobilityService nebo akce|Aktualizace služby Mobility|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/repairReplication nebo akce|Oprava replikace|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/applyRecoveryPoint nebo akce|Použít bod obnovení|
|/ trezory/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/recoveryPoints/čtení|Číst všechny body obnovení replikace|
|/vaults/replicationPolicies/Read|Číst všechny zásady|
|/vaults/replicationPolicies/Write|Vytvořit nebo aktualizovat všechny zásady|
|/vaults/replicationPolicies/DELETE|Odstranit všechny zásady|
|/vaults/replicationRecoveryPlans/Read|Číst všechny plány obnovení|
|/vaults/replicationRecoveryPlans/Write|Vytvořit nebo aktualizovat všechny plány obnovení|
|/vaults/replicationRecoveryPlans/DELETE|Odstranit všechny plány obnovení|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Plán obnovení plánované převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plán obnovení převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/testFailover/Action|Testovací převzetí služeb při selhání obnovení plán|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Plán obnovení testovací převzetí služeb při selhání čištění|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Plán obnovení potvrzení převzetí služeb při selhání|
|/vaults/replicationRecoveryPlans/reProtect/Action|Znovu nastavte ochranu plánu obnovení|
|/ Trezory/extendedInformation/čtení|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|A trezory/extendedInformation/zápis|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|Trezory, extendedInformation nebo odstranění|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|/ Trezory/backupManagementMetaData/čtení|Vrátí metadata správy zálohování trezoru Recovery Services.|
|/ Trezory/backupProtectionContainers/čtení|Vrátí všechny kontejnery, které patří k předplatnému|
|/ Trezory/backupFabrics/operationResults/čtení|Vrátí stav operace|
|/ Trezory/backupFabrics/protectionContainers/čtení|Vrátí všechny registrované kontejnery|
|/ Trezory/backupFabrics/protectionContainers /<br>operationResults/čtení|Načte výsledky operace provedené na kontejneru ochrany.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/čtení|Vrátí podrobnosti o objektu chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems a zápis|Vytvoření zálohy chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems nebo odstranění|Odstranění chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/zálohování nebo akce|Provede zálohování chráněné položky.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/operationResults/čtení|Načte výsledky operace provedené na chráněných položkách.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/operationStatus/čtení|Načte stav operace provedené na chráněných položkách.|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints/čtení|Načíst body obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>obnovení nebo akce|Obnoví body obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery nebo akce|Zřízení rychlých položky obnovení pro chráněné položky|
|/ Trezory/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery nebo akce|Odvolat rychlých položky obnovení pro chráněné položky|
|/ Trezory nebo použití/čtení|Vrátí podrobnosti využití trezoru Recovery Services.|
|/vaults/usages/Read|Číst všechny použití trezoru|
|A trezory/certifikátů/zápis|Operace aktualizace prostředek certifikátu aktualizuje prostředek nebo trezoru certifikát přihlašovacích údajů.|
|/ Trezory/tokenInfo/čtení|Vrátí token informace o trezoru služeb zotavení.|
|/vaults/replicationAlertSettings/Read|Číst všechny nastavení výstrah|
|/vaults/replicationAlertSettings/Write|Vytvořit nebo aktualizovat nastavení výstrah|
|/ Trezory/backupOperations/čtení|Operace zálohování vrátí stav pro obnovení služeb trezoru.|
|/ Trezory/storageConfig/čtení|Vrátí konfiguraci úložiště pro obnovení služeb trezoru.|
|A trezory/storageConfig/zápis|Aktualizace konfiguraci úložiště pro obnovení služeb trezoru.|
|/ Trezory/backupUsageSummaries/čtení|Vrátí souhrny pro chráněné položky a chráněné servery pro služeb zotavení.|
|/ Trezory/backupProtectedItems/čtení|Vrátí seznam všech chráněných položek.|
|/ Trezory/backupconfig/vaultconfig/čtení|Vrátí konfiguraci pro obnovení služeb trezoru.|
|A trezory/backupconfig/vaultconfig/zápis|Konfigurace aktualizací pro obnovení služeb trezoru.|
|A trezory/registeredIdentities/zápis|Operaci zaregistrovat kontejneru služby lze použít k registraci kontejner službou obnovení.|
|/ Trezory/registeredIdentities/čtení|Získat kontejnerů, které můžete použít operaci získat kontejnery zaregistrovat pro prostředek.|
|Trezory, registeredIdentities nebo odstranění|Zrušit registraci kontejneru operaci lze použít se zrušit registraci kontejneru.|
|/ Trezory/registeredIdentities/operationResults/čtení|Výsledky operace Get, lze operace získat stav operace a výsledek asynchronně odeslaná operaci|
|/vaults/replicationJobs/Read|Číst všechny úlohy|
|/vaults/replicationJobs/Cancel/Action|Zrušení úlohy|
|/vaults/replicationJobs/restart/Action|Restartujte úlohu|
|/vaults/replicationJobs/Resume/Action|Obnovení úlohy|
|/ Trezory/backupPolicies/čtení|Vrátí všechny zásady ochrany|
|A trezory/backupPolicies/zápis|Vytvoří zásadu ochrany|
|Trezory, backupPolicies nebo odstranění|Odstranění zásady ochrany|
|/ Trezory/backupPolicies/operationResults/čtení|Načte výsledky operace zásad.|
|/ Trezory/backupPolicies/operationStatus/čtení|Načíst stav operace zásad.|
|/ Trezory/vaultTokens/čtení|Trezor tokenu operaci lze použít k získání tokenu trezoru pro operace trezoru úrovně back-end.|
|/ Trezory/monitoringConfigurations/notificationConfiguration/čtení|Získá konfigurace oznámení trezoru služby pro obnovení.|
|/ Trezory/backupJobs/čtení|Vrátí všechny objekty úlohy|
|/ Trezory/backupJobs nebo zrušit nebo akce|Zrušení úlohy|
|/ Trezory/backupJobs/operationResults/čtení|Vrátí výsledek operace úlohy.|
|/Locations/allocateStamp/Action|AllocateStamp je interní operace, které používá služba|
|/Locations/allocatedStamp/Read|GetAllocatedStamp je interní operace, kterou používá služba|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Operace | Popis |
|---|---|
|/ checkNamespaceAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje předplatné u poskytovatele prostředků Relay a povolí vytváření prostředků Relay.|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav Namespace jsou vlastnosti, které lze aktualizovat.|
|/namespaces/Read|Získá seznam popisů prostředku oboru názvů.|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec k oboru názvů|
|/namespaces/HybridConnections/Write|Vytvoření nebo aktualizace HybridConnection vlastnosti.|
|/namespaces/HybridConnections/Read|Získání seznamu popisů HybridConnection prostředků|
|/namespaces/HybridConnections/DELETE|Operace odstranění HybridConnection prostředků|
|/namespaces/HybridConnections/authorizationRules/Write|Vytvořte HybridConnection autorizační pravidla a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/HybridConnections/authorizationRules/DELETE|Operace odstranění HybridConnection autorizační pravidla|
|/namespaces/HybridConnections/authorizationRules/listkeys/Action|Získat připojovací řetězec k HybridConnection|
|/namespaces/WcfRelays/Write|Vytvoření nebo aktualizace WcfRelay vlastnosti.|
|/namespaces/WcfRelays/Read|Získání seznamu popisů WcfRelay prostředků|
|/namespaces/WcfRelays/DELETE|Operace odstranění WcfRelay prostředků|
|/namespaces/WcfRelays/authorizationRules/Write|Vytvořte WcfRelay autorizační pravidla a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/WcfRelays/authorizationRules/DELETE|Operace odstranění WcfRelay autorizační pravidla|
|/namespaces/WcfRelays/authorizationRules/listkeys/Action|Získat připojovací řetězec k WcfRelay|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Operace | Popis |
|---|---|
|/ AvailabilityStatuses/čtení|Získá stavy dostupnosti pro všechny prostředky v zadaném rozsahu.|
|/ AvailabilityStatuses/aktuální/čtení|Získá stav dostupnosti pro zadaný prostředek.|

## <a name="microsoftresources"></a>Microsoft.Resources

| Operace | Popis |
|---|---|
|/ checkResourceName nebo akce|Zkontroluje správnost názvu prostředku.|
|/providers/Read|Načte seznam zprostředkovatelů.|
|/subscriptions/Read|Získá výpis předplatných.|
|/subscriptions/operationresults/Read|Načte výsledky operací s předplatným.|
|/subscriptions/providers/Read|Načte nebo vypíše zprostředkovatele prostředků.|
|/subscriptions/tagNames/Read|Načte nebo vypíše značky předplatného.|
|/subscriptions/tagNames/Write|Přidá značku předplatného.|
|/subscriptions/tagNames/DELETE|Odstraní značku předplatného.|
|/subscriptions/tagNames/tagValues/Read|Načte nebo vypíše hodnoty značky předplatného.|
|/subscriptions/tagNames/tagValues/Write|Přidá hodnotu značky předplatného.|
|/subscriptions/tagNames/tagValues/DELETE|Odstraní hodnotu značky předplatného.|
|/subscriptions/Resources/Read|Získá prostředky předplatného.|
|/subscriptions/resourceGroups/Read|Načte nebo vypíše skupinu prostředků.|
|/subscriptions/resourceGroups/Write|Vytvoří nebo aktualizuje skupinu prostředků.|
|/subscriptions/resourceGroups/DELETE|Odstraní skupinu prostředků a všechny její prostředky.|
|/subscriptions/resourceGroups/moveResources/Action|Přesune prostředky z jedné skupiny prostředků do jiné.|
|/subscriptions/resourceGroups/validateMoveResources/Action|Ověří přesun prostředků z jedné skupiny prostředků do jiné.|
|/subscriptions/resourcegroups/Resources/Read|Načte prostředky pro skupinu prostředků.|
|/subscriptions/resourcegroups/Deployments/Read|Načte nebo vypíše nasazení.|
|/subscriptions/resourcegroups/Deployments/Write|Vytvoří nebo aktualizuje nasazení.|
|/subscriptions/resourcegroups/Deployments/operationstatuses/Read|Načte nebo vypíše stavy operace nasazení.|
|/subscriptions/resourcegroups/Deployments/Operations/Read|Načte nebo vypíše operace nasazení.|
|/subscriptions/Locations/Read|Získá výpis podporovaných umístění.|
|/Links/Read|Načte nebo vypíše odkazy na prostředek.|
|/ odkazy a zápis|Vytvoří nebo aktualizuje odkaz na prostředek.|
|/Links/DELETE|Odstraní odkaz na prostředek.|
|/tenants/Read|Získá výpis klientů.|
|/Resources/Read|Načte seznam prostředků na základě filtrů.|
|/Deployments/Read|Načte nebo vypíše nasazení.|
|/ nasazení a zápis|Vytvoří nebo aktualizuje nasazení.|
|/Deployments/DELETE|Odstraní nasazení.|
|/Deployments/Cancel/Action|Zruší nasazení.|
|/Deployments/Validate/Action|Ověří nasazení.|
|/Deployments/Operations/Read|Načte nebo vypíše operace nasazení.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Operace | Popis |
|---|---|
|/jobcollections/Read|Získání kolekce úloh|
|/ kolekce a zápis|Vytvoří nebo aktualizuje kolekci úloh.|
|/jobcollections/DELETE|Kolekce úloh odstranění.|
|/jobcollections/enable/Action|Kolekce úloh umožňuje.|
|/jobcollections/disable/Action|Zakáže kolekce úloh.|
|/jobcollections/Jobs/Read|Získá úlohy.|
|/jobcollections/Jobs/Write|Vytvoří nebo aktualizuje úlohu.|
|/jobcollections/Jobs/DELETE|Odstraní úlohu.|
|/jobcollections/Jobs/Run/Action|Úloha se spustí.|
|/jobcollections/Jobs/generateLogicAppDefinition/Action|Vygeneruje definici Aplikace logiky na základě úlohy Scheduleru.|
|/jobcollections/Jobs/jobhistories/Read|Získá historie úlohy.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro poskytovatele vyhledávání prostředků a umožňuje vytvoření služby vyhledávání.|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost názvu služby.|
|/ searchServices/zápisu|Vytvoří nebo aktualizuje službu vyhledávání.|
|/searchServices/Read|Načte službu vyhledávání.|
|/searchServices/DELETE|Odstraní službu vyhledávání.|
|/searchServices/Start/Action|Spustí službu vyhledávání.|
|/searchServices/stop/Action|Zastaví službu vyhledávání.|
|/searchServices/listAdminKeys/Action|Načte klíče správce.|
|/searchServices/regenerateAdminKey/Action|Znovu vygeneruje klíč správce.|
|/searchServices/createQueryKey/Action|Vytvoří klíč dotazu.|
|/searchServices/queryKey/Read|Načte klíče dotazu.|
|/searchServices/queryKey/DELETE|Odstraní klíč dotazu.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Operace | Popis |
|---|---|
|/jitNetworkAccessPolicies/Read|Získá zásad přístupu k síti za běhu|
|/ jitNetworkAccessPolicies/zápisu|Vytvoří nové zásady přístupu k síti za běhu nebo aktualizuje existující|
|/jitNetworkAccessPolicies/initiate/Action|Inicializuje zásady přístupu k síti za běhu|
|/securitySolutionsReferenceData/Read|Získá referenční data řešení zabezpečení|
|/securityStatuses/Read|Získá stavy stavu zabezpečení prostředků Azure|
|/webApplicationFirewalls/Read|Získá webové aplikace brány firewall|
|/ webApplicationFirewalls/zápisu|Vytvoří nové brány firewall webových aplikací nebo aktualizuje existující|
|/webApplicationFirewalls/DELETE|Odstraní brány firewall webových aplikací|
|/securitySolutions/Read|Získá řešení zabezpečení|
|/ securitySolutions/zápisu|Vytvoří nové řešení zabezpečení nebo aktualizuje existující|
|/securitySolutions/DELETE|Odstraní zabezpečení řešení|
|/Tasks/Read|Získá všechny dostupné zabezpečení doporučení|
|/Tasks/dismiss/Action|Zavření doporučení zabezpečení|
|/Tasks/Activate/Action|Aktivovat doporučení zabezpečení|
|/Policies/Read|Získá zásady zabezpečení|
|/ Zásady/zápisu|Aktualizace nastavení zásad zabezpečení|
|/applicationWhitelistings/Read|Získá whitelistings aplikace|
|/ applicationWhitelistings/zápisu|Vytvoří nové vytvoření seznamu povolených aplikací nebo aktualizuje existující|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Operace | Popis |
|---|---|
|/ subscriptions/zápisu|Vytvoří nebo aktualizuje předplatné|
|/ brány a zápis|Vytvoří nebo aktualizuje brány|
|/gateways/DELETE|Odstraní brány|
|/gateways/Read|Získá brány|
|/gateways/regenerateprofile/Action|Regeneruje profil brány|
|/gateways/upgradetolatest/Action|Upgraduje bránu na nejnovější verzi|
|/ uzly a zápis|Vytvoří nebo aktualizuje uzlu|
|/Nodes/DELETE|Odstraní uzel|
|/Nodes/Read|Získá uzel|
|/ relací a zápis|Vytvoří nebo aktualizuje relaci|
|/Sessions/Read|Získá relaci|
|/Sessions/DELETE|Odstraní relaci.|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Operace | Popis |
|---|---|
|/ checkNameAvailability nebo akce|Zkontroluje dostupnost oboru názvů v daném předplatném.|
|/ registrace nebo akce|Zaregistruje předplatné u poskytovatele prostředků ServiceBus a povolí vytváření prostředků ServiceBus.|
|/ obory názvů a zápis|Vytvořte prostředek Namespace a aktualizujte jeho vlastnosti. Značky a stav Namespace jsou vlastnosti, které lze aktualizovat.|
|/namespaces/Read|Získá seznam popisů prostředku oboru názvů.|
|/ obory názvů/odstranit|Odstranit prostředek Namespace|
|/namespaces/metricDefinitions/Read|Získání seznamu Namespace metrik popisy prostředků|
|/namespaces/authorizationRules/Write|Autorizační pravidla úrovni Namespace vytvořit a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/authorizationRules/Read|Získá seznam popisů autorizačních pravidel oboru názvů.|
|/namespaces/authorizationRules/DELETE|Odstraňte Namespace autorizační pravidlo. Výchozí Namespace autorizační pravidlo nelze odstranit. |
|/namespaces/authorizationRules/listkeys/Action|Získat připojovací řetězec k oboru názvů|
|/namespaces/authorizationRules/regenerateKeys/Action|Znovu vygeneruje primární nebo sekundární klíč k prostředku.|
|/namespaces/diagnosticSettings/Read|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/diagnosticSettings/Write|Získání seznamu popisů Namespace nastavení pro diagnostiku prostředků|
|/namespaces/Queues/Write|Vytvoření nebo aktualizace fronty vlastnosti.|
|/namespaces/Queues/Read|Získání seznamu popisů fronty prostředků|
|/namespaces/Queues/DELETE|Operace odstranění fronty prostředků|
|/namespaces/Queues/authorizationRules/Write|Vytvoření fronty autorizační pravidla a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/Queues/authorizationRules/Read| Získání seznamu fronty autorizační pravidla|
|/namespaces/Queues/authorizationRules/DELETE|Operace odstranění fronty autorizační pravidla|
|/namespaces/Queues/authorizationRules/listkeys/Action|Získat připojovací řetězec do fronty|
|/namespaces/Queues/authorizationRules/regenerateKeys/Action|Znovu vygeneruje primární nebo sekundární klíč k prostředku.|
|/namespaces/logDefinitions/Read|Získání seznamu protokolů Namespace popisy prostředků|
|/namespaces/topics/Write|Vytvoření nebo aktualizace tématu vlastnosti.|
|/namespaces/topics/Read|Získání seznamu popisů tématu prostředků|
|/namespaces/topics/DELETE|Operace odstranění prostředků tématu|
|/namespaces/topics/authorizationRules/Write|Vytvoření tématu autorizační pravidla a aktualizujte jeho vlastnosti. Autorizační pravidla přístupová práva, primární a sekundární klíče lze aktualizovat.|
|/namespaces/topics/authorizationRules/Read| Získání seznamu tématu autorizační pravidla|
|/namespaces/topics/authorizationRules/DELETE|Operace odstranění tématu autorizační pravidla|
|/namespaces/topics/authorizationRules/listkeys/Action|Získat připojovací řetězec k tématu|
|/namespaces/topics/authorizationRules/regenerateKeys/Action|Znovu vygeneruje primární nebo sekundární klíč k prostředku.|
|/namespaces/topics/Subscriptions/Write|Vytvoření nebo aktualizace TopicSubscription vlastnosti.|
|/namespaces/topics/Subscriptions/Read|Získání seznamu popisů TopicSubscription prostředků|
|/namespaces/topics/Subscriptions/DELETE|Operace odstranění TopicSubscription prostředků|
|/namespaces/topics/Subscriptions/Rules/Write|Vytvořit nebo aktualizovat pravidlo vlastnosti.|
|/namespaces/topics/Subscriptions/Rules/Read|Získání seznamu popisů pravidlo prostředků|
|/namespaces/topics/Subscriptions/Rules/DELETE|Operace se odstranit pravidlo prostředků|

## <a name="microsoftsql"></a>Microsoft.Sql

| Operace | Popis |
|---|---|
|/Servers/Read|Vrátí seznam serverů ve skupině prostředků na předplatné|
|/ servery a zápis|Vytvořit nový server nebo upravit vlastnosti stávajícího serveru ve skupině prostředků na předplatné|
|/Servers/DELETE|Odstraňte server a všechny obsažené databáze i elastické fondy|
|/Servers/import/Action|Vytvoření nové databáze na serveru a nasadit schéma a data z balíčku DacPac|
|/Servers/upgrade/Action|Nové funkce, které jsou k dispozici v nejnovější verzi serveru povolíte a zadáte databáze edice převod mapy|
|/Servers/VulnerabilityAssessmentScans/Action|Spuštění kontroly serveru assessment ohrožení zabezpečení|
|/Servers/operationResults/Read|Operace se používá ke sledování postup upgradu serveru z nižší verze vyšší|
|/Servers/operationResults/DELETE|Zrušení upgrade verze serveru v průběhu|
|/Servers/securityAlertPolicies/Read|Načíst podrobnosti o zásadě detekce hrozeb server nakonfigurovaný na daném serveru|
|/Servers/securityAlertPolicies/Write|Změnit detekce hrozeb serveru pro daný server|
|/Servers/securityAlertPolicies/operationResults/Read|Načíst výsledky zásad detekce hrozeb operace nastavení serveru|
|/Servers/Administrators/Read|Načíst podrobnosti o správce serveru|
|/Servers/Administrators/Write|Vytvořit nebo aktualizovat Správce serveru|
|/Servers/Administrators/DELETE|Odstranit správce serveru ze serveru|
|/Servers/recoverableDatabases/Read|Tato operace se používá pro zotavení po havárii živé databáze k obnovení databáze pro poslední známá funkční bodu zálohy. Vrací informace o poslední správné zálohy, ale nepodporuje obnovení ve skutečnosti databáze.|
|/Servers/serviceObjectives/Read|Načtení seznamu cílů na úrovni služby (také označované jako úrovně výkonu) k dispozici na daném serveru|
|/Servers/firewallRules/Read|Načíst podrobnosti o pravidlo brány firewall serveru|
|/Servers/firewallRules/Write|Vytvořit nebo aktualizovat pravidlo brány firewall serveru, který určuje rozsah IP adres povolené připojení k serveru|
|/Servers/firewallRules/DELETE|Odstranit pravidlo brány firewall ze serveru|
|/Servers/administratorOperationResults/Read|Načíst výsledky operace správce serveru|
|/Servers/recommendedElasticPools/Read|Načtení doporučení pro fondy elastické databáze pro snížení nákladů na nebo vylepšení výkonu na základě využití prostředků historica|
|/Servers/recommendedElasticPools/Metrics/Read|Načtení metriky pro fondy doporučené elastické databáze pro daný server|
|/Servers/recommendedElasticPools/Databases/Read|Načtení databází, které mají být přidány do doporučené fondy elastických databází pro daný server|
|/Servers/elasticPools/Read|Načíst podrobnosti o fondu elastické databáze na daném serveru|
|/Servers/elasticPools/Write|Vytvořit novou nebo změnit vlastnosti existující fond elastické databáze|
|/Servers/elasticPools/DELETE|Odstraňte existující fond elastické databáze|
|/Servers/elasticPools/operationResults/Read|Načíst podrobnosti o fondu operace dané elastické databáze|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>metricDefinitions/čtení|Návratové typy metriky, které jsou k dispozici pro fondy elastické databáze|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/čtení|Získá nastavení diagnostiky pro tento prostředek.|
|/Servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings a zápis|Vytvoří nebo aktualizuje nastavení diagnostiky pro tento prostředek.|
|/Servers/elasticPools/Metrics/Read|Vrátí metriky využití prostředků fondu elastické databáze|
|/Servers/elasticPools/elasticPoolDatabaseActivity/Read|Načtení činnosti a údaje na danou databázi, která je součástí fondu elastické databáze|
|/Servers/elasticPools/advisors/Read|Vrátí seznam poradci pro elastický fond k dispozici|
|/Servers/elasticPools/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni elastického fondu.|
|/Servers/elasticPools/advisors/recommendedActions/Read|Vrátí seznam doporučených akcí zadaný advisor pro elastický fond|
|/Servers/elasticPools/advisors/recommendedActions/Write|Použít akci doporučenou v elastického fondu|
|/Servers/elasticPools/elasticPoolActivity/Read|Načtení činnosti a údaje na danou databázi elastického fondu|
|/Servers/elasticPools/Databases/Read|Načtení seznamu a podrobnosti databáze, které jsou součástí fondu elastické databáze na daném serveru|
|/Servers/auditingPolicies/Read|Načíst podrobnosti o serveru tabulky výchozí zásady na daném serveru nakonfigurován auditování|
|/Servers/auditingPolicies/Write|Změňte výchozí server tabulky auditování pro daný server|
|/Servers/disasterRecoveryConfiguration/operationResults/Read|Získat výsledky operace konfigurace obnovení po havárii|
|/Servers/advisors/Read|Vrátí seznam poradci, které jsou k dispozici pro server|
|/Servers/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni serveru.|
|/Servers/advisors/recommendedActions/Read|Vrátí seznam doporučených akcí zadaný advisor pro server|
|/Servers/advisors/recommendedActions/Write|Použití doporučenou akci na server|
|/Servers/usages/Read|Vrátí kvóty DTU serveru a aktuální consuption DTU všemi databázemi v rámci serveru|
|/Servers/elasticPoolEstimates/Read|Vrátí seznam odhad elastického fondu vytvořeny již pro tento server|
|/Servers/elasticPoolEstimates/Write|Vytvoří nový odhad elastického fondu pro seznam databází poskytuje|
|/Servers/auditingSettings/Read|Načíst podrobnosti o zásadě auditování objektů blob serveru na daném serveru nakonfigurován|
|/Servers/auditingSettings/Write|Změnit nastavení auditování serveru objekt blob pro daný server|
|/Servers/auditingSettings/operationResults/Read|Načíst výsledek operace nastavení zásad auditování objektu blob serveru|
|/Servers/backupLongTermRetentionVaults/Read|Tato operace se používá k získání trezoru zálohování dlouhodobé uchovávání. Vrátí informace o úložišti registrované k tomuto serveru.|
|/Servers/backupLongTermRetentionVaults/Write|Zaregistrovat k trezoru zálohování dlouhodobé uchovávání|
|/Servers/restorableDroppedDatabases/Read|Načíst seznam databází, které byly vyřazeny na daném serveru, které jsou stále v rámci zásady uchovávání informací. Tato operace vrátí seznam databází a související metadata, jako je datum odstranění.|
|/Servers/Databases/Read|Vrátí seznam serverů ve skupině prostředků na předplatné|
|/Servers/Databases/Write|Vytvořit nový server nebo upravit vlastnosti stávajícího serveru ve skupině prostředků na předplatné|
|/Servers/Databases/DELETE|Odstraňte server a všechny obsažené databáze i elastické fondy|
|/Servers/Databases/export/Action|Vytvoření nové databáze na serveru a nasadit schéma a data z balíčku DacPac|
|/Servers/Databases/VulnerabilityAssessmentScans/Action|Spusťte kontrolu databáze assessment ohrožení zabezpečení.|
|/Servers/Databases/pause/Action|Pozastavení edici databáze datového skladu|
|/Servers/Databases/Resume/Action|Obnovit databázi datového skladu edition|
|/Servers/Databases/operationResults/Read|Operace se používá ke sledování průběhu dlouhotrvající operace databáze, jako je například škálování.|
|/Servers/Databases/replicationLinks/Read|Návratový podrobnosti o propojení replikace navázána pro konkrétní databázi|
|/Servers/Databases/replicationLinks/DELETE|Ukončení relace replikace vynuceně a potenciální ztráty dat.|
|/Servers/Databases/replicationLinks/Unlink/Action|Ukončení relace replikace vynuceně nebo po synchronizaci s partnera|
|/Servers/Databases/replicationLinks/Failover/Action|Převzetí služeb při selhání po synchronizaci všech změní z primární, provedení této databáze do primární vztah replikace a přijímání vzdálených primární do sekundárního|
|/Servers/Databases/replicationLinks/forceFailoverAllowDataLoss/Action|Převzetí služeb při selhání okamžitě potenciální ztrátě dat, provedení této databáze do primární vztah replikace a přijímání vzdálených primární do sekundárního|
|/Servers/Databases/replicationLinks/updateReplicationMode/Action|Režim aktualizace replikace pro odkaz na synchronní nebo asynchronní režim|
|/Servers/Databases/replicationLinks/operationResults/Read|Načíst stav operace dlouho běžící na odkazy replikace databáze|
|/Servers/Databases/dataMaskingPolicies/Read|Načtení podrobností maskování zásady nakonfigurované na danou databázi dat|
|/Servers/Databases/dataMaskingPolicies/Write|Změnit zásady pro danou databázi maskování dat.|
|/Servers/Databases/dataMaskingPolicies/Rules/Read|Načtení podrobností pravidla zásad, které jsou nakonfigurované na danou databázi maskování dat|
|/Servers/Databases/dataMaskingPolicies/Rules/Write|Změňte pravidla zásad pro danou databázi maskování dat.|
|/Servers/Databases/securityAlertPolicies/Read|Načíst podrobnosti o zásadě detekce hrozeb nakonfigurované na danou databázi|
|/Servers/Databases/securityAlertPolicies/Write|Změna zásad detekce hrozeb pro danou databázi|
|/Servers/Databases/providers/Microsoft.Insights/<br>metricDefinitions/čtení|Návratové typy metriky, které jsou k dispozici pro databáze|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings/čtení|Získá nastavení diagnostiky pro tento prostředek.|
|/Servers/Databases/providers/Microsoft.Insights/<br>diagnosticSettings a zápis|Vytvoří nebo aktualizuje nastavení diagnostiky pro tento prostředek.|
|/Servers/Databases/providers/Microsoft.Insights/<br>logDefinitions/čtení|Získá dostupné protokoly pro databáze|
|/Servers/Databases/topQueries/Read|Vrátí statistiku modulu runtime pro vybraný dotaz agregovat do vybrané časové období|
|/Servers/Databases/topQueries/queryText/Read|Vrátí text Transact-SQL pro ID vybraný dotaz|
|/Servers/Databases/topQueries/Statistics/Read|Vrátí statistiku modulu runtime pro vybraný dotaz agregovat do vybrané časové období|
|/Servers/Databases/connectionPolicies/Read|Načíst podrobnosti o zásadě připojení nakonfigurovaná na danou databázi|
|/Servers/Databases/connectionPolicies/Write|Změnit zásady připojení pro danou databázi|
|/Servers/Databases/Metrics/Read|Vrátí metriky využití prostředků databáze|
|/Servers/Databases/auditRecords/Read|Načtení záznamů auditu databáze objektů blob|
|/Servers/Databases/transparentDataEncryption/Read|Načíst stav a podrobnosti o funkce zabezpečení dat transparentní šifrování pro danou databázi|
|/Servers/Databases/transparentDataEncryption/Write|Povolit nebo zakázat transparentní šifrování dat pro danou databázi|
|/Servers/Databases/transparentDataEncryption/operationResults/Read|Načíst stav a podrobnosti o funkce zabezpečení dat transparentní šifrování pro danou databázi|
|/Servers/Databases/auditingPolicies/Read|Načtení podrobností tabulky auditování zásady, které jsou nakonfigurované na danou databázi|
|/Servers/Databases/auditingPolicies/Write|Změna zásad auditování tabulka pro danou databázi|
|/Servers/Databases/dataWarehouseQueries/Read|Vrátí distribuční dotazu informace o datovém skladu pro vybraný dotaz ID|
|/ servery/databáze/dataWarehouseQueries /<br>dataWarehouseQuerySteps/čtení|Vrátí informace o kroku distribuovaného dotazu dotazu datový sklad pro vybraný krok ID|
|/Servers/Databases/serviceTierAdvisors/Read|Vrátí návrhu o škálování databáze nahoru nebo dolů na základě statistik provádění dotazu na výkon nebo snížit náklady na|
|/Servers/Databases/advisors/Read|Vrátí seznam poradci, které jsou k dispozici pro databázi|
|/Servers/Databases/advisors/Write|Aktualizace automatického – spuštění stav advisor na úrovni databáze.|
|/Servers/Databases/advisors/recommendedActions/Read|Vrátí seznam doporučené akce zadané pro databázi Poradce pro|
|/Servers/Databases/advisors/recommendedActions/Write|Použít akci doporučenou v databázi|
|/Servers/Databases/usages/Read|Vrátí maximální velikost databáze, které lze přejít a aktuální velikost obsazena dat|
|/Servers/Databases/queryStore/Read|Vrátí aktuální hodnoty nastavení úložiště dotazů pro databázi|
|/Servers/Databases/queryStore/Write|Aktualizace nastavení úložiště dotazů pro databázi|
|/Servers/Databases/auditingSettings/Read|Načíst podrobnosti o objektu blob auditování zásady, které jsou nakonfigurované na danou databázi|
|/Servers/Databases/auditingSettings/Write|Změna zásad auditování objektů blob pro danou databázi|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Read|Načíst seznam doporučení indexu v databázi|
|/Servers/Databases/schemas/Tables/recommendedIndexes/Write|Použít doporučení indexu|
|/Servers/Databases/schemas/Tables/Columns/Read|Načíst seznam sloupců tabulky.|
|/Servers/Databases/missingindexes/Read|Vrátí návrhy o indexy databáze k vytvoření, úpravě nebo odstranění za účelem zlepšení výkonu dotazů|
|/Servers/Databases/missingindexes/Write|Použít databázi doporučení indexu v konkrétní databázi|
|/Servers/Databases/importExportOperationResults/Read|Vrátí podrobnosti o import databáze nebo operace exportu z DacPac umístěný v účtu úložiště|
|/Servers/importExportOperationResults/Read|Vrátí seznam s podrobnostmi o databázové operace importu z účtu úložiště na daném serveru|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Zaregistruje předplatné pro zprostředkovatele prostředku úložiště a povolí vytvoření účtů úložiště.|
|/checknameavailability/Read|Zkontroluje, že název účtu je platný a nepoužívá se.|
|/ storageAccounts/zápisu|Vytvoří účet úložiště se zadanými parametry, aktualizuje vlastnosti a značky nebo přidá vlastní doménu pro zadaný účet úložiště.|
|/storageAccounts/DELETE|Odstraní existující účet úložiště.|
|/storageAccounts/listkeys/Action|Vrátí přístupové klíče pro zadaný účet úložiště.|
|/storageAccounts/regeneratekey/Action|Obnoví přístupové klíče pro zadaný účet úložiště.|
|/storageAccounts/Read|Vrátí seznam účtů úložišť nebo načte vlastnosti zadaného účtu.|
|/storageAccounts/listAccountSas/Action|Vrátí token SAS účtu pro zadaný účet úložiště.|
|/storageAccounts/listServiceSas/Action|Token SAS služby úložiště|
|/storageAccounts/Services/diagnosticSettings/Write|Vytvořit nebo aktualizovat nastavení diagnostiky účet úložiště.|
|/skus/Read|Zobrazí seznam SKU nepodporuje Microsoft.Storage.|
|/usages/Read|Vrací limit a aktuální počet použití pro zdroje v zadaném odběru|
|/Operations/Read|Zadá dotaz na stav asynchronní operace.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Microsoft.Storage upozorní, že virtuální síť nebo podsíť, Probíhá odstranění|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Operace | Popis |
|---|---|
|/Managers/clearAlerts/Action|Vymažte všechny výstrahy spojené se Správce zařízení.|
|/Managers/getActivationKey/Action|Získáte aktivační klíč pro správce zařízení.|
|/Managers/regenerateActivationKey/Action|Znovu vygenerujte aktivační klíč pro správce zařízení.|
|/Managers/regenarateRegistationCertificate/Action|Znovu vygenerujte registrační certifikát pro správce zařízení.|
|/Managers/getEncryptionKey/Action|Získání šifrovacího klíče pro správce zařízení.|
|/Managers/Read|Uvádí nebo získá správce zařízení|
|/Managers/DELETE|Odstraní Správci zařízení|
|/ Správci a zápis|Vytvořit nebo aktualizovat Správci zařízení|
|/Managers/configureDevice/Action|Konfiguruje zařízení|
|/Managers/listActivationKey/Action|Získá aktivační klíč Správce zařízení StorSimple.|
|/Managers/listPublicEncryptionKey/Action|Seznam veřejný šifrovací klíče nástroje Správce zařízení StorSimple.|
|/Managers/listPrivateEncryptionKey/Action|Získá privátní šifrovací klíč pro správce zařízení StorSimple.|
|/Managers/provisionCloudAppliance/Action|Vytvořte nové zařízení cloudu.|
|A správci/zápis|Operace Vytvořit trezor vytvoří prostředek Azure typu trezor.|
|/ Správci/čtení|Operace získání trezoru získá objekt, který reprezentuje prostředků Azure typu 'trezoru.|
|Správci nebo odstranění|Operace odstranění trezoru Odstraní zadaný prostředek Azure typu 'trezoru.|
|/Managers/storageAccountCredentials/Write|Vytvořit nebo aktualizovat údaje k účtu úložiště|
|/Managers/storageAccountCredentials/Read|Uvádí nebo získá přihlašovací údaje účtu úložiště|
|/Managers/storageAccountCredentials/DELETE|Odstraní údaje k účtu úložiště|
|/Managers/storageAccountCredentials/listAccessKey/Action|Seznam přístupových klíčů přihlašovacích údajů účtu úložiště|
|/Managers/accessControlRecords/Read|Uvádí nebo získá záznamy řízení přístupu|
|/Managers/accessControlRecords/Write|Vytvořit nebo aktualizovat záznamy řízení přístupu|
|/Managers/accessControlRecords/DELETE|Odstraní záznamy řízení přístupu|
|/Managers/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/bandwidthSettings/Read|Seznam nastavení šířky pásma (pouze 8000 řada)|
|/Managers/bandwidthSettings/Write|Vytvoří novou nebo aktualizuje nastavení šířky pásma (pouze řady 8000)|
|/Managers/bandwidthSettings/DELETE|Odstraní existující nastavení šířky pásma (pouze řady 8000)|
|/ Správci/extendedInformation/čtení|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|A správci/extendedInformation/zápis|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|Správci, extendedInformation nebo odstranění|Operace Získat rozšířené informace získá rozšířené informace o objektu, který představuje prostředek Azure typu trezor.|
|/Managers/Alerts/Read|Uvádí nebo získá výstrahy|
|/Managers/storageDomains/Read|Uvádí nebo získá domén úložiště|
|/Managers/storageDomains/Write|Vytvořit nebo aktualizovat domén úložiště|
|/Managers/storageDomains/DELETE|Odstraní domén úložiště|
|/Managers/Devices/scanForUpdates/Action|Vyhledávání aktualizací v zařízení.|
|/Managers/Devices/download/Action|Stažení aktualizací pro zařízení.|
|/Managers/Devices/Install/Action|Instalaci aktualizací na zařízení.|
|/Managers/Devices/Read|Uvádí nebo získá zařízení|
|/Managers/Devices/Write|Vytvořit nebo aktualizovat zařízení|
|/Managers/Devices/DELETE|Odstraní zařízení|
|/Managers/Devices/Deactivate/Action|Deaktivuje zařízení.|
|/Managers/Devices/publishSupportPackage/Action|Publikujte balíček pro podporu zařízení pro řešení potíží s Microsoft Support.|
|/Managers/Devices/Failover/Action|Převzetí služeb při selhání zařízení.|
|/Managers/Devices/sendTestAlertEmail/Action|Odeslat testovací výstrahy e-mailu k příjemce nakonfigurované e-mailů.|
|/Managers/Devices/installUpdates/Action|Nainstaluje aktualizace zařízení|
|/Managers/Devices/listFailoverSets/Action|Zobrazí seznam sady převzetí služeb při selhání ze stávajících zařízení.|
|/Managers/Devices/listFailoverTargets/Action|Seznam cílů převzetí služeb při selhání zařízení|
|/Managers/Devices/publicEncryptionKey/Action|Seznam veřejný šifrovací klíč Správce zařízení|
|/ Správci/zařízení nebo hardwareComponentGroups /<br>Pro čtení|Seznam skupin součástí hardwaru|
|/ Správci/zařízení nebo hardwareComponentGroups /<br>changeControllerPowerState nebo akce|Změnit stav napájení řadiče hardwaru skupiny součástí|
|/Managers/Devices/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/Devices/chapSettings/Write|Vytvořit nebo aktualizovat nastavení Chap|
|/Managers/Devices/chapSettings/Read|Uvádí nebo získá nastavení Chap|
|/Managers/Devices/chapSettings/DELETE|Odstraní nastavení Chap|
|/Managers/Devices/backupScheduleGroups/Read|Uvádí nebo získá skupiny plánu zálohování|
|/Managers/Devices/backupScheduleGroups/Write|Vytvořit nebo aktualizovat skupiny plánu zálohování|
|/Managers/Devices/backupScheduleGroups/DELETE|Odstraní plán zálohování skupiny|
|/Managers/Devices/updateSummary/Read|Uvádí nebo získá souhrn aktualizace|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>Import nebo akce|Import konfigurace zdroje pro migraci.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>startMigrationEstimate nebo akce|Spustíte úlohu k odhad doby trvání procesu migrace.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>startMigration nebo akce|Spustit migraci pomocí zdroj konfigurace|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>confirmMigration nebo akce|Potvrzuje úspěšné migrace a potvrďte ho.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchMigrationEstimate nebo akce|Načtěte stav odhad úlohy migrace.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchMigrationStatus nebo akce|Načtěte stav migrace.|
|/ Správci/zařízení nebo migrationSourceConfigurations /<br>fetchConfirmMigrationStatus nebo akce|Načtěte stav potvrdit migrace.|
|/Managers/Devices/alertSettings/Read|Uvádí nebo získá nastavení výstrah|
|/Managers/Devices/alertSettings/Write|Vytvořit nebo aktualizovat nastavení výstrah|
|/Managers/Devices/networkSettings/Read|Uvádí nebo získá nastavení sítě|
|/Managers/Devices/networkSettings/Write|Vytvoří novou nebo aktualizovat nastavení sítě|
|/Managers/Devices/Jobs/Read|Uvádí nebo získá úlohy|
|/Managers/Devices/Jobs/Cancel/Action|Zrušení spuštěné úlohy|
|/Managers/Devices/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/Devices/volumeContainers/Write|Vytvoří novou nebo aktualizuje kontejnery svazků (pouze řady 8000)|
|/Managers/Devices/volumeContainers/Read|Seznam kontejnery svazků (pouze 8000 řada)|
|/Managers/Devices/volumeContainers/DELETE|Odstraní existující kontejnery svazků (pouze řady 8000)|
|/Managers/Devices/volumeContainers/listEncryptionKeys/Action|Seznam šifrování klíče kontejnery svazků|
|/Managers/Devices/volumeContainers/rolloverEncryptionKey/Action|Šifrování klíče výměny kontejnery svazků|
|/Managers/Devices/volumeContainers/Metrics/Read|Seznam metriky|
|/Managers/Devices/volumeContainers/Volumes/Read|Seznam svazků|
|/Managers/Devices/volumeContainers/Volumes/Write|Vytvoří novou nebo aktualizuje svazky|
|/Managers/Devices/volumeContainers/Volumes/DELETE|Odstraní existující svazky|
|/Managers/Devices/volumeContainers/Volumes/Metrics/Read|Seznam metriky|
|/Managers/Devices/volumeContainers/Volumes/metricsDefinitions/Read|Seznam definice metrik|
|/Managers/Devices/volumeContainers/metricsDefinitions/Read|Seznam definice metrik|
|/Managers/Devices/iscsiservers/Read|Uvádí nebo získá iSCSI servery|
|/Managers/Devices/iscsiservers/Write|Vytvořit nebo aktualizovat iSCSI servery|
|/Managers/Devices/iscsiservers/DELETE|Odstraní iSCSI servery|
|/Managers/Devices/iscsiservers/Backup/Action|Proveďte zálohování serveru iSCSI.|
|/Managers/Devices/iscsiservers/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/Devices/iscsiservers/Disks/Read|Uvádí nebo získá disky|
|/Managers/Devices/iscsiservers/Disks/Write|Vytvořit nebo aktualizovat disky|
|/Managers/Devices/iscsiservers/Disks/DELETE|Odstraní disky|
|/Managers/Devices/iscsiservers/Disks/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/Devices/iscsiservers/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/Devices/backups/Read|Uvádí nebo získá zálohovacího skladu|
|/Managers/Devices/backups/DELETE|Odstraní zálohovacího skladu|
|/Managers/Devices/backups/Restore/Action|Všechny svazky obnovte ze zálohovacího skladu.|
|/Managers/Devices/backups/Elements/clone/Action|Klonování sdílené složky nebo svazku pomocí zálohování elementu.|
|/Managers/Devices/backupPolicies/Write|Vytvoří novou nebo aktualizovat zásady zálohování (pouze řady 8000)|
|/Managers/Devices/backupPolicies/Read|Seznam zálohování zásady (pouze 8000 řada)|
|/Managers/Devices/backupPolicies/DELETE|Odstraní existující zásady zálohování (pouze řady 8000)|
|/Managers/Devices/backupPolicies/Backup/Action|Proveďte zálohu ručně vytvořit na vyžádání zálohování všech svazků chráněnými zásadami.|
|/Managers/Devices/backupPolicies/Schedules/Write|Vytvoří novou nebo aktualizovat plány|
|/Managers/Devices/backupPolicies/Schedules/Read|Seznam plány|
|/Managers/Devices/backupPolicies/Schedules/DELETE|Odstraní existující plán|
|/Managers/Devices/securitySettings/Update/Action|Aktualizujte nastavení zabezpečení.|
|/Managers/Devices/securitySettings/Read|Seznam nastavení zabezpečení|
|/ Správci/zařízení nebo securitySettings /<br>syncRemoteManagementCertificate nebo akce|Synchronizujte certifikát vzdálené správy pro zařízení.|
|/Managers/Devices/securitySettings/Write|Vytvoří novou nebo aktualizuje nastavení zabezpečení|
|/Managers/Devices/fileservers/Read|Uvádí nebo získá souborových serverů|
|/Managers/Devices/fileservers/Write|Vytvořit nebo aktualizovat souborových serverů|
|/Managers/Devices/fileservers/DELETE|Odstraní souborových serverů|
|/Managers/Devices/fileservers/Backup/Action|Proveďte zálohování souborového serveru.|
|/Managers/Devices/fileservers/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/Devices/fileservers/shares/Write|Vytvořit nebo aktualizovat sdílené složky|
|/Managers/Devices/fileservers/shares/Read|Uvádí nebo získá sdílené složky|
|/Managers/Devices/fileservers/shares/DELETE|Odstraní sdílené složky|
|/Managers/Devices/fileservers/shares/Metrics/Read|Uvádí nebo získá metriky|
|/Managers/Devices/fileservers/shares/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/Devices/fileservers/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/Devices/timeSettings/Read|Uvádí nebo získá nastavení času|
|/Managers/Devices/timeSettings/Write|Vytvoří novou nebo aktualizuje nastavení času|
|A správci/certifikátů/zápis|Operace aktualizace prostředek certifikátu aktualizuje prostředek nebo trezoru certifikát přihlašovacích údajů.|
|/Managers/cloudApplianceConfigurations/Read|Seznam zařízení cloudu podporované konfigurace|
|/Managers/metricsDefinitions/Read|Uvádí nebo získá definice metrik|
|/Managers/encryptionSettings/Read|Uvádí nebo získá nastavení šifrování|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Operace | Popis |
|---|---|
|/streamingjobs/Start/Action|Spuštění úlohy Stream Analytics|
|/streamingjobs/stop/Action|Zastavení úlohy Stream Analytics|
|/ streamingjobs/čtení|Úlohy Stream Analytics pro čtení|
|/ streamingjobs/zápisu|Zápis úlohy Stream Analytics|
|/ streamingjobs/odstranit|Odstranit úlohy Stream Analytics|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/Read|Načte dostupné metriky pro streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Read|Nastavení diagnostiky pro čtení.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Write|Zápis nastavení diagnostiky.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/Read|Získá dostupné protokoly pro streamingjobs|
|/streamingjobs/Transformations/Read|Číst Stream Analytics úlohy transformace|
|/streamingjobs/Transformations/Write|Zápis transformace úlohy Stream Analytics|
|/streamingjobs/Transformations/DELETE|Odstranit transformaci úlohy Stream Analytics|
|/streamingjobs/inputs/Read|Číst Stream Analytics úlohy vstup|
|/streamingjobs/inputs/Write|Zápis vstupu úlohy Stream Analytics|
|/streamingjobs/inputs/DELETE|Odstranit vstupu úlohy Stream Analytics|
|/streamingjobs/Outputs/Read|Výstup úlohy analýzy datového proudu pro čtení|
|/streamingjobs/Outputs/Write|Zapisovat výstup úlohy Stream Analytics|
|/streamingjobs/Outputs/DELETE|Odstranit výstup úlohy Stream Analytics|

## <a name="microsoftsupport"></a>Microsoft.Support

| Operace | Popis |
|---|---|
|/ registrace nebo akce|Registruje zprostředkovatele prostředků podpory.|
|/supportTickets/Read|Umožňuje získat podrobnosti lístku podpory (včetně stavu, závažnosti, podrobností kontaktu a komunikací) nebo seznam lístků podpory pro všechna předplatná.|
|/ supportTickets/zápisu|Vytvoří nebo aktualizuje lístek podpory. Můžete vytvořit lístek podpory pro Technical, fakturace, kvóty nebo správy předplatného související problémy. Můžete aktualizovat závažnost, podrobnosti kontaktu a komunikace pro existující lístky žádostí o podporu.|

## <a name="microsoftweb"></a>Microsoft.Web

| Operace | Popis |
|---|---|
|nebo zrušit registraci nebo akce|Zrušte registraci poskytovatele prostředků Microsoft.Web pro předplatné.|
|/ ověření nebo akce|Ověření.|
|/ registrace nebo akce|Registrace poskytovatele prostředků Microsoft.Web pro předplatné.|
|/ hostingEnvironments/čtení|Umožňuje získat vlastnosti služby App Service Environment|
|/ hostingEnvironments/zápisu|Vytvoření nové služby App Service Environment nebo aktualizovat existující|
|/ hostingEnvironments/odstranit|Odstranění služby App Service Environment|
|/hostingEnvironments/reboot/Action|Restartujte všechny počítače ve službě App Service Environment|
|/hostingenvironments/Resume/Action|Obnovte hostitelská prostředí.|
|/hostingenvironments/suspend/Action|Pozastavte hostitelská prostředí.|
|/hostingenvironments/metricdefinitions/Read|Získat hostování prostředí definice metrik.|
|/hostingEnvironments/workerPools/Read|Umožňuje získat vlastnosti fondu pracovních procesů ve službě App Service Environment|
|/hostingEnvironments/workerPools/Write|Vytvořit nový fond pracovních procesů ve službě App Service Environment nebo aktualizovat stávající|
|/hostingenvironments/workerpools/metricdefinitions/Read|Získat hostování prostředí Workerpools metrika definice.|
|/hostingenvironments/workerpools/Metrics/Read|Získat hostování prostředí Workerpools metriky.|
|/hostingenvironments/workerpools/skus/Read|Získat hostování prostředí Workerpools SKU.|
|/hostingenvironments/workerpools/usages/Read|Získat hostování prostředí Workerpools použití.|
|/hostingenvironments/Sites/Read|Získáte hostování prostředí webové aplikace.|
|/hostingenvironments/serverfarms/Read|Získáte hostování prostředí plány služby App Service.|
|/hostingenvironments/usages/Read|Získat hostování použití prostředí.|
|/hostingenvironments/capacities/Read|Získat hostování prostředí kapacity.|
|/hostingenvironments/Operations/Read|Získat hostování prostředí operace.|
|/hostingEnvironments/multiRolePools/Read|Umožňuje získat vlastnosti fondu front-endu ve službě App Service Environment|
|/hostingEnvironments/multiRolePools/Write|Vytvořit nový fond front-endové služby App Service Environment, nebo aktualizaci existující|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Získat hostování definice metrik u fondy prostředí.|
|/hostingenvironments/multirolepools/Metrics/Read|Získat hostování prostředí u fondy metriky.|
|/hostingenvironments/multirolepools/skus/Read|Získat hostování prostředí u fondy SKU.|
|/hostingenvironments/multirolepools/usages/Read|Získat hostování prostředí u fondy použití.|
|/hostingenvironments/Diagnostics/Read|Získat hostování prostředí diagnostiky.|
|/publishingusers/Read|Uživatelé získat publikování.|
|/ publishingusers/zápisu|Aktualizace publikování uživatelů.|
|/checknameavailability/Read|Zkontrolujte, zda název prostředku je k dispozici.|
|/ geoRegions/čtení|Získejte seznam geografických oblastech.|
|/ weby/čtení|Umožňuje získat vlastnosti webové aplikace|
|/ weby/zápisu|Vytvořit novou webovou aplikaci nebo aktualizovat stávající|
|/ weby/odstranit|Odstranit stávající webovou aplikaci|
|/Sites/Backup/Action|Vytvoří novou zálohu webové aplikace|
|/Sites/publishxml/Action|Získat publikování profil xml pro webovou aplikaci|
|/Sites/Publish/Action|Publikování webové aplikace|
|/Sites/restart/Action|Restartování webové aplikace|
|/Sites/Start/Action|Spustí webovou aplikaci|
|/Sites/stop/Action|Zastavení webové aplikace|
|/Sites/slotsswap/Action|Prohodit sloty nasazení webové aplikace|
|/Sites/slotsdiffs/Action|Získat rozdíly v konfiguraci mezi sloty a webové aplikace|
|/Sites/applySlotConfig/Action|Použít konfiguraci webové aplikace slotu z cílového slotu na aktuální webové aplikace|
|/Sites/resetSlotConfig/Action|Resetovat konfiguraci webové aplikace|
|/Sites/Functions/Action|Funkce Web Apps.|
|/Sites/listsyncfunctiontriggerstatus/Action|Seznam synchronizace funkce aktivační událost stavu webových aplikací.|
|/Sites/networktrace/Action|Webové aplikace, síťové trasování.|
|/Sites/newpassword/Action|Nové_heslo webové aplikace.|
|/Sites/Sync/Action|Synchronizace webové aplikace.|
|/Sites/operationresults/Read|Získáte výsledky operace webové aplikace.|
|/Sites/webjobs/Read|Načtení úloh Webjob webové aplikace.|
|/ weby/zálohování/čtení|Získáte zálohování webové aplikace.|
|/Sites/Backup/Write|Aktualizace webové aplikace zálohování.|
|/Sites/metricdefinitions/Read|Získáte webové aplikace metrika definice.|
|/Sites/Metrics/Read|Získáte metriky webové aplikace.|
|/Sites/continuouswebjobs/DELETE|Odstranění webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/Read|Získáte webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/Start/Action|Spuštění webové aplikace nepřetržité webové úlohy.|
|/Sites/continuouswebjobs/stop/Action|Zastavení webové aplikace nepřetržité webové úlohy.|
|/Sites/domainownershipidentifiers/Read|Získání identifikátorů vlastnictví webové aplikace domény.|
|/Sites/domainownershipidentifiers/Write|Aktualizace webové aplikace domény vlastnictví identifikátory.|
|/Sites/premieraddons/DELETE|Odstraňte rozšíření Premier webové aplikace.|
|/Sites/premieraddons/Read|Získáte rozšíření Premier webové aplikace.|
|/Sites/premieraddons/Write|Aktualizace doplňků Premier webové aplikace.|
|/Sites/triggeredwebjobs/DELETE|Odstraňte spouštěná WebJobs webové aplikace.|
|/Sites/triggeredwebjobs/Read|Získáte spouštěná WebJobs webové aplikace.|
|/Sites/triggeredwebjobs/Run/Action|Spuštění webové aplikace aktivované webové úlohy.|
|/Sites/hostnamebindings/DELETE|Odstranění webové aplikace Hostname vazby.|
|/Sites/hostnamebindings/Read|Získáte webové aplikace Hostname vazby.|
|/Sites/hostnamebindings/Write|Aktualizujte vazby názvů hostitelů aplikací Web.|
|/Sites/virtualnetworkconnections/DELETE|Odstraňte připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/Read|Získání připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/Write|Aktualizujte připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/gateways/Read|Získáte brány připojení virtuální sítě webové aplikace.|
|/Sites/virtualnetworkconnections/gateways/Write|Aktualizace brány připojení virtuální sítě webové aplikace.|
|/Sites/publishxml/Read|Získat publikování webové aplikace XML.|
|/Sites/hybridconnectionrelays/Read|Získáte webové aplikace hybridní připojení předávání.|
|/Sites/perfcounters/Read|Získáte čítače výkonu webové aplikace.|
|/Sites/usages/Read|Získáte použití webové aplikace.|
|/Sites/slots/Write|Vytvořit nový Slot webové aplikace nebo aktualizovat stávající|
|/Sites/slots/DELETE|Odstraňte existující Slot webové aplikace|
|/Sites/slots/Backup/Action|Vytvoří novou zálohu Slot webové aplikace.|
|/Sites/slots/publishxml/Action|Získat publikování profil xml pro Slot webové aplikace|
|/Sites/slots/Publish/Action|Publikování Slot webové aplikace|
|/Sites/slots/restart/Action|Restartujte Slot webové aplikace|
|/Sites/slots/Start/Action|Spustit Slot webové aplikace|
|/Sites/slots/stop/Action|Zastavit Slot webové aplikace|
|/Sites/slots/slotsswap/Action|Prohodit sloty nasazení webové aplikace|
|/Sites/slots/slotsdiffs/Action|Získat rozdíly v konfiguraci mezi sloty a webové aplikace|
|/Sites/slots/applySlotConfig/Action|Použít konfiguraci webové aplikace slotu z cílového slotu na aktuální pozici.|
|/Sites/slots/resetSlotConfig/Action|Resetovat konfiguraci slot webové aplikace|
|/Sites/slots/Read|Umožňuje získat vlastnosti nasazovací slot webové aplikace|
|/Sites/slots/newpassword/Action|Sloty nové_heslo webové aplikace.|
|/Sites/slots/Sync/Action|Sloty webové aplikace synchronizace.|
|/Sites/slots/operationresults/Read|Získáte výsledky operace sloty webové aplikace.|
|/Sites/slots/webjobs/Read|Načtení úloh Webjob sloty webové aplikace.|
|/Sites/slots/Backup/Write|Aktualizujte zálohování sloty webové aplikace.|
|/Sites/slots/metricdefinitions/Read|Získáte definice metrik sloty webové aplikace.|
|/Sites/slots/Metrics/Read|Získáte metriky sloty webové aplikace.|
|/Sites/slots/continuouswebjobs/DELETE|Odstranění webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/Read|Získáte webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/Start/Action|Spuštění webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/continuouswebjobs/stop/Action|Zastavení webové aplikace sloty nepřetržité webové úlohy.|
|/Sites/slots/premieraddons/DELETE|Odstranění webové aplikace sloty Premier rozšíření.|
|/Sites/slots/premieraddons/Read|Získáte webové aplikace sloty Premier rozšíření.|
|/Sites/slots/premieraddons/Write|Aktualizace webové aplikace sloty Premier doplňků.|
|/Sites/slots/triggeredwebjobs/DELETE|Odstraňte spouštěná WebJobs sloty webové aplikace.|
|/Sites/slots/triggeredwebjobs/Read|Získáte spouštěná WebJobs sloty webové aplikace.|
|/Sites/slots/triggeredwebjobs/Run/Action|Sloty spuštění webové aplikace aktivované webové úlohy.|
|/Sites/slots/hostnamebindings/DELETE|Odstraňte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/hostnamebindings/Read|Získáte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/hostnamebindings/Write|Aktualizujte vazby názvů hostitelů sloty webové aplikace.|
|/Sites/slots/phplogging/Read|Získáte Phplogging sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/DELETE|Odstraňte připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/Read|Získání připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/Write|Aktualizujte připojení virtuální sítě sloty webové aplikace.|
|/Sites/slots/virtualnetworkconnections/gateways/Write|Aktualizace webové aplikace sloty virtuální síť připojení brány.|
|/Sites/slots/usages/Read|Získáte použití sloty webové aplikace.|
|/Sites/slots/hybridconnection/DELETE|Odstranění webové aplikace sloty hybridní připojení.|
|/Sites/slots/hybridconnection/Read|Získáte webové aplikace sloty hybridní připojení.|
|/Sites/slots/hybridconnection/Write|Aktualizace webové aplikace sloty hybridní připojení.|
|/Sites/slots/config/Read|Získat nastavení konfigurace Slot webové aplikace|
|/Sites/slots/config/list/Action|Seznam Slot webové aplikace citlivé nastavení zabezpečení, jako je například publikování přihlašovací údaje, nastavení aplikace a připojovacích řetězců|
|/Sites/slots/config/Write|Aktualizovat nastavení konfigurace Slot webové aplikace|
|/Sites/slots/config/DELETE|Odstraňte konfigurační sloty webové aplikace.|
|/Sites/slots/instances/Read|Získá instance sloty webové aplikace.|
|/Sites/slots/instances/Processes/Read|Získáte webové aplikace sloty instancí procesy.|
|/Sites/slots/instances/Deployments/Read|Získá webové aplikace sloty instancí nasazení.|
|/Sites/slots/sourcecontrols/Read|Získat nastavení konfigurace Slot webové aplikace zdrojového kódu|
|/Sites/slots/sourcecontrols/Write|Aktualizovat nastavení konfigurace správy zdrojů Slot webové aplikace|
|/Sites/slots/sourcecontrols/DELETE|Odstranění nastavení konfigurace správy zdrojů Slot webové aplikace|
|/Sites/slots/Restore/Read|Získáte obnovení sloty webové aplikace.|
|/Sites/slots/analyzecustomhostname/Read|Získat webové aplikace sloty analyzovat vlastní název hostitele.|
|/Sites/slots/backups/Read|Umožňuje získat vlastnosti zálohování se sloty webové aplikace|
|/Sites/slots/backups/list/Action|Seznam webových aplikací sloty zálohování.|
|/Sites/slots/backups/Restore/Action|Obnovte zálohy sloty webové aplikace.|
|/Sites/slots/Deployments/DELETE|Odstraňte nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/Read|Získá nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/Write|Aktualizace nasazení sloty webové aplikace.|
|/Sites/slots/Deployments/log/Read|Získáte webové aplikace sloty nasazení protokolu.|
|/Sites/hybridconnection/DELETE|Odstranění webové aplikace hybridní připojení.|
|/Sites/hybridconnection/Read|Získáte webové aplikace hybridní připojení.|
|/Sites/hybridconnection/Write|Aktualizace webové aplikace hybridní připojení.|
|/Sites/recommendationhistory/Read|Zobrazit historii doporučení webové aplikace.|
|/Sites/Recommendations/Read|Získejte seznam doporučení pro webovou aplikaci.|
|/Sites/Recommendations/disable/Action|Zakážete doporučení webové aplikace.|
|/Sites/config/Read|Získat nastavení konfigurace webové aplikace|
|/Sites/config/list/Action|Seznam webové aplikace citlivé nastavení zabezpečení, jako je například publikování přihlašovací údaje, nastavení aplikace a připojovacích řetězců|
|/Sites/config/Write|Aktualizovat nastavení konfigurace webové aplikace|
|/Sites/config/DELETE|Odstraňte konfigurační webové aplikace.|
|/Sites/instances/Read|Získá instance webové aplikace.|
|/Sites/instances/Processes/DELETE|Odstraňte procesy instance webové aplikace.|
|/Sites/instances/Processes/Read|Získáte procesy instance webové aplikace.|
|/Sites/instances/Deployments/Read|Získá nasazení instance webové aplikace.|
|/Sites/sourcecontrols/Read|Získat nastavení konfigurace zdrojového kódu webové aplikace|
|/Sites/sourcecontrols/Write|Aktualizovat nastavení konfigurace správy zdrojů webové aplikace|
|/Sites/sourcecontrols/DELETE|Odstranění nastavení konfigurace správy zdrojů webové aplikace|
|/Sites/Restore/Read|Získáte obnovení webové aplikace.|
|/Sites/analyzecustomhostname/Read|Analýza vlastní název hostitele.|
|/Sites/backups/Read|Umožňuje získat vlastnosti zálohování webové aplikace|
|/Sites/backups/list/Action|Seznam webových aplikací zálohování.|
|/Sites/backups/Restore/Action|Obnovte zálohy webové aplikace.|
|/Sites/snapshots/Read|Získáte snímky webové aplikace.|
|/Sites/Functions/DELETE|Odstraňte funkce webové aplikace.|
|/Sites/Functions/listsecrets/Action|Funkce, webové aplikace seznamu tajných klíčů.|
|/Sites/Functions/Read|Získáte funkce Web Apps.|
|/Sites/Functions/Write|Aktualizace funkcí webové aplikace.|
|/Sites/Deployments/DELETE|Odstraňte nasazení webové aplikace.|
|/Sites/Deployments/Read|Získá nasazení webové aplikace.|
|/Sites/Deployments/Write|Aktualizace nasazení webové aplikace.|
|/Sites/Deployments/log/Read|Získání protokolu nasazení webové aplikace.|
|/Sites/Diagnostics/Read|Získání diagnostiky webové aplikace.|
|/Sites/Diagnostics/workerprocessrecycle/Read|Získáte recyklace procesu Worker webové aplikace diagnostiky.|
|/Sites/Diagnostics/workeravailability/Read|Získáte Workeravailability webové aplikace diagnostiky.|
|/Sites/Diagnostics/runtimeavailability/Read|Získáte dostupnosti webové aplikace diagnostiky modulu Runtime.|
|/Sites/Diagnostics/cpuanalysis/Read|Získáte Cpuanalysis webové aplikace diagnostiky.|
|/Sites/Diagnostics/servicehealth/Read|Získání stavu služby webových aplikací diagnostiky.|
|/Sites/Diagnostics/frebanalysis/Read|Získáte webové analýzy FREB diagnostiky aplikace.|
|/availablestacks/Read|Získáte zásobníky k dispozici.|
|/isusernameavailable/Read|Zkontrolujte, zda uživatelské jméno je k dispozici.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/čtení|Získejte seznam rozhraní API.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a zápis|Přidání nového rozhraní Api nebo aktualizovat existující.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo odstranění|Odstranění existujícího rozhraní Api.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo připojení/čtení|Získejte seznam připojení.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a připojení/zápis|Uložení nové připojení nebo aktualizuje stávající.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, připojení nebo odstranění|Odstraňte existující připojení.|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API nebo připojení/connectionAcls nebo pro čtení|ConnectionAcls pro čtení|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a připojení nebo connectionAcls/zápis|Přidat nebo aktualizovat ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, připojení, connectionAcls nebo odstranění|Odstranit ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/connectionAcls/čtení|Čtení ConnectionAcls pro rozhraní Api|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API/apiAcls/čtení|ConnectionAcls pro čtení|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API a apiAcls/zápis|Vytvořit nebo aktualizovat seznamy ACL rozhraní Api|
|/Microsoft.Web/apiManagementAccounts/<br>rozhraní API, apiAcls nebo odstranění|Odstranit rozhraní Api seznamy řízení přístupu|
|/ serverfarms/čtení|Umožňuje získat vlastnosti na plán služby App Service|
|/ serverfarms/zápisu|Nový plán služby App Service umožňuje vytvořit nebo aktualizovat stávající|
|/ serverfarms/odstranit|Odstranit existující plán služby App Service|
|/serverfarms/restartSites/Action|Restartujte všechny webové aplikace v plán služby App Service|
|/serverfarms/operationresults/Read|Získáte výsledky operace plány služby App Service.|
|/serverfarms/Capabilities/Read|Získáte možnosti plány služby App Service.|
|/serverfarms/metricdefinitions/Read|Získáte definice metrik plány služby App Service.|
|/serverfarms/Metrics/Read|Získáte metriky plány služby App Service.|
|/serverfarms/hybridconnectionplanlimits/Read|Získáte limity plán služby App Service plány hybridní připojení.|
|/serverfarms/virtualnetworkconnections/Read|Získání připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/DELETE|Odstraňte směrování připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/Read|Získáte trasy připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/Routes/Write|Aktualizace tras připojení virtuální sítě plány služby App Service.|
|/serverfarms/virtualnetworkconnections/gateways/Write|Aktualizace brány připojení virtuální sítě plány služby App Service.|
|/serverfarms/firstpartyapps/Settings/DELETE|Odstraňte první strany aplikace nastavení plánů služby App Service.|
|/serverfarms/firstpartyapps/Settings/Read|Získáte nastavení první strany aplikací plány služby App Service.|
|/serverfarms/firstpartyapps/Settings/Write|Aktualizujte nastavení první strany aplikací plány služby App Service.|
|/serverfarms/Sites/Read|Získáte webové aplikace plány aplikační služby.|
|/serverfarms/workers/reboot/Action|Restartování pracovních procesů plány služby App Service.|
|/serverfarms/hybridconnectionrelays/Read|Získáte předávací připojení hybridní plány služby App Service.|
|/serverfarms/skus/Read|Získáte SKU plány služby App Service.|
|/serverfarms/usages/Read|Získáte použití plány služby App Service.|
|/serverfarms/hybridconnectionnamespaces/relays/Sites/Read|Získáte plány služby App Service hybridní připojení obory názvů předávací webové aplikace.|
|/ishostnameavailable/Read|Zkontrolujte, zda název hostitele je k dispozici.|
|/ connectionGateways/čtení|Získejte seznam připojení brány.|
|/ connectionGateways/zápisu|Vytvoří nebo aktualizuje připojení brány.|
|/ connectionGateways/odstranit|Odstraní připojení brány.|
|/connectionGateways/JOIN/Action|Spojí připojení brány.|
|/classicmobileservices/Read|Získáte Classic mobilních služeb.|
|/skus/Read|Získáte SKU.|
|/ certifikátů/čtení|Získáte seznam certifikátů.|
|/ certifikátů/zápisu|Přidat nový certifikát nebo aktualizovat existující.|
|/ certifikátů/odstranit|Odstraňte existující certifikát.|
|/Operations/Read|Získejte operace.|
|/ doporučení/čtení|Získejte seznam doporučení pro předplatné.|
|/ishostingenvironmentnameavailable/Read|Získáte, pokud je k dispozici hostování název prostředí.|
|/ apiManagementAccounts/čtení|Získejte seznam ApiManagementAccounts.|
|/ apiManagementAccounts/zápisu|Přidat nové ApiManagementAccount nebo aktualizovat stávající|
|/ apiManagementAccounts/odstranit|Odstraňte existující ApiManagementAccount|
|/apiManagementAccounts/connectionAcls/Read|Získání seznamu ACL připojení.|
|/apiManagementAccounts/apiAcls/Read|ConnectionAcls pro čtení|
|/ připojení/čtení|Získejte seznam připojení.|
|/ připojení/zápisu|Vytvoří nebo aktualizuje připojení.|
|/ připojení/odstranit|Odstraní připojení.|
|/Connections/JOIN/Action|Spojí připojení.|
|/Connections/confirmconsentcode/Action|Potvrďte souhlas kód připojení.|
|/Connections/listconsentlinks/Action|Seznam odkazů souhlasu pro připojení.|
|/deploymentlocations/Read|Získáte umístění nasazení.|
|/sourcecontrols/Read|Získáte ovládací prvky zdroje.|
|/ sourcecontrols/zápisu|Aktualizace ovládacích prvků zdroje.|
|/managedhostingenvironments/Read|Získat spravované hostitelská prostředí.|
|/managedhostingenvironments/Sites/Read|Získat spravované hostování prostředí webové aplikace.|
|/managedhostingenvironments/serverfarms/Read|Získat spravované hostování prostředí plány služby App Service.|
|/Locations/managedapis/Read|Získáte umístění spravovaná rozhraní API.|
|/Locations/apioperations/Read|Získejte operace umístění rozhraní API.|
|/Locations/connectiongatewayinstallations/Read|Získáte umístění připojení brány instalace.|
|/ listSitesAssignedToHostName/čtení|Získáte názvy lokalit, které jsou přiřazeny k hostitele.|

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [vytvořit vlastní roli](role-based-access-control-custom-roles.md).

- Zkontrolujte [integrovaným rolím RBAC](role-based-access-built-in-roles.md).

- Zjistěte, jak spravovat přístup k přiřazení [uživatelem](role-based-access-control-manage-assignments.md) nebo [prostředkem](role-based-access-control-configure.md) 
