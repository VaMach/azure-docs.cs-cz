---
title: Azure App Service pro webové a mobilní aplikace | Microsoft Docs
description: Zjistěte, jak vám může Azure App Service pomoci s vývojem, nasazením a správou webových a mobilních aplikací.
keywords: app service, azure app service, cena app service, škálování, škálovatelné, nasazení aplikace, nasazení azure app, paas, platforma jako služba
services: app-service
documentationcenter: ''
author: omarkmsft
manager: dwrede
editor: jimbe

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2016
ms.author: omark

---
# Co je Azure App Service?
*App Service* je služba Microsoft Azure typu [platforma jako služba](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS). Webové a mobilní aplikace můžete vytvářet pro libovolnou platformu nebo zařízení. Své aplikace můžete integrovat s řešeními SaaS a propojovat s lokálními aplikacemi. A lze také automatizovat firemní procesy. Azure spouští vaše aplikace na plně spravovaných virtuálních počítačích (VM) se sdílenými prostředky, které si sami zvolíte, nebo na vyhrazených virtuálních počítačích. 

App Service zahrnuje webové a mobilní funkce, které jsme dříve nabízeli samostatně jako Weby Azure a Azure Mobile Services.  Obsahuje také nové možnosti pro automatizaci obchodních procesů a hostování cloudových rozhraní API. Jako jediná integrovaná služba vám App Service umožňuje vytvářet různé komponenty – weby, back-endy mobilních aplikací, rozhraní RESTful API a firemní procesy – v jediném řešení.

Následující 4minutové video stručně shrnuje, v čem se App Service podobá dřívějším nabídkám Azure a co nabízí nového.

+[AZURE.VIDEO app-service-history-lesson] 

## Proč používat App Service?
Toto jsou některé klíčové funkce a možnosti služby App Service: 

* **Více jazyků a rozhraní:** App Service zahrnuje prvotřídní podporu pro ASP.NET, Node.js, Javu, PHP a Python. Na virtuálních počítačích App Service můžete také spouštět [Windows PowerShell a další skripty nebo spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md).
* **Optimalizace DevOps:** Můžete nastavit [průběžnou integraci a nasazení](../app-service-web/app-service-continuous-deployment.md) pomocí služeb Visual Studio Team Services, GitHub nebo BitBucket. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](../app-service-web/web-sites-staged-publishing.md). Máte možnost [testování A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Spravujte aplikace ve službě App Service pomocí prostředí [Azure PowerShell](../powershell-install-configure.md) nebo [multiplatformního rozhraní příkazového řádku (CLI)](../xplat-cli-install.md).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](../app-service-web/web-sites-scale.md) nebo [horizontálního](../azure-portal/insights-how-to-scale.md) navýšení kapacity. Aplikace je možné hostovat kdekoli v globální infrastruktuře datacenter Microsoftu a smlouva [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) pro App Service zaručuje vysokou dostupnost.
* **Připojení k platformám SaaS a lokálním datům:** Můžete si vybírat z více než 50 [konektorů](../connectors/apis-list.md) pro firemní systémy (například SAP, Siebel nebo Oracle), služby SaaS (například služby Salesforce nebo Office 365) a internetové služby (například Facebook nebo Twitter). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [Azure Virtual Networks](../app-service-web/web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/).
* **Šablony aplikací**: Vyberte si z rozsáhlého seznamu šablon aplikací na [Azure Marketplace](https://azure.microsoft.com/marketplace/), které vám umožní používat průvodce k instalaci oblíbeného open-source softwaru, jako je WordPress, Joomla nebo Drupal.
* **Integrace nástroje Visual Studio**: Vyhrazené nástroje v nástroji Visual Studio zjednodušují práci při vytváření, nasazování a ladění.

## Typy aplikací v App Service
App Service nabízí několik *typů aplikací*, z nichž každá je určena k hostování konkrétního typu úlohy:

* [**Webové aplikace**](../app-service-web/app-service-web-overview.md) – k hostování webů a webových aplikací
* [**Mobilní aplikace**](../app-service-mobile/app-service-mobile-value-prop.md) – k hostování back-endu mobilních aplikací
* [**Aplikace API**](../app-service-api/app-service-api-apps-why-best-platform.md) – k hostování cloudových rozhraní API 
* [**Aplikace logiky**](../app-service-logic/app-service-logic-what-are-logic-apps.md) – k automatizaci přístupu k datům a jejich používání napříč cloudy bez nutnosti psaní kódu

Slovem *aplikace* se zde označují prostředky hostování vyhrazené pro spouštění určité úlohy nebo procesu. Pokud zde například zmiňujeme „webovou aplikaci“, pravděpodobně jste zvyklí chápat webovou aplikaci jako výpočetní prostředky i kód aplikace, které společně poskytují v prohlížeči příslušné funkce. V App Service ale *webová aplikace* představuje výpočetní prostředky, které Azure poskytuje k hostování kódu aplikace. Pokud se vaše aplikace skládá z webového front-endu a back-endu RESTful API, můžete obě komponenty nasadit do webové aplikace, anebo můžete kód front-endu nasadit do webové aplikace a kód back-endu do aplikace API. Vaše aplikace se může skládat z více aplikací App Service různého druhu.

## Plány služby App Service
[Plány služby App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) určují druh výpočetních prostředků, na kterých vaše aplikace běží. Pokud neočekáváte velké přenosové zatížení, můžete použít sdílené virtuální počítače (VM). V případě většího zatížení si můžete vybrat z několika velikostí vyhrazených virtuálních počítačů. Jeden plán může sdílet více aplikací App Service, které je možné společně s plánem škálovat nahoru a dolů.

Pokud potřebujete větší škálovatelnost a izolaci sítě, můžete aplikace spustit ve službě [App Service Environment](../app-service-web/app-service-app-service-environment-intro.md). 

## Ceny
Informace o cenách služby App Service najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/). 

## Začínáme se službou App Service
[Můžete si vytvořit zdarma a rychle dočasnou webovou aplikaci, mobilní aplikaci nebo aplikaci logiky](http://go.microsoft.com/fwlink/?LinkId=523751), a to bez použití kreditní karty, bez závazků a bez problémů.

Nebo si můžete otevřít [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/) a vyzkoušet některý z našich úvodních kurzů:

* [Kurz: Vytvoření webové aplikace](../app-service-web/app-service-web-get-started.md)
* [Kurz: Vytvoření mobilní aplikace](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Kurz: Vytvoření aplikace API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Kurz: Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!--HONumber=Sep16_HO3-->


