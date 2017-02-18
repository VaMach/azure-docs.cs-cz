---
title: "Azure App Service pro webové a mobilní aplikace a aplikace API | Dokumentace Microsoftu"
description: "Zjistěte, jak vám může Azure App Service pomoci s vývojem, nasazením a správou webových a mobilních aplikací."
keywords: "app service, azure app service, cena app service, škálování, škálovatelné, nasazení aplikace, nasazení aplikace azure, paas, platforma jako služba, web, azure mobile"
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
translationtype: Human Translation
ms.sourcegitcommit: 40dd75832302d7d88e852e2ea93821750675607e
ms.openlocfilehash: 4deb60c25bf13d1f31b58f002a7edea0672eca25


---
# <a name="what-is-azure-app-service"></a>Co je Azure App Service?
*App Service* je služba Microsoft Azure typu [platforma jako služba](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS). Webové a mobilní aplikace můžete vytvářet pro libovolnou platformu nebo zařízení. Své aplikace můžete integrovat s řešeními SaaS a propojovat s lokálními aplikacemi. A lze také automatizovat firemní procesy. Azure spouští vaše aplikace na plně spravovaných virtuálních počítačích (VM) se sdílenými prostředky, které si sami zvolíte, nebo na vyhrazených virtuálních počítačích.

App Service zahrnuje webové a mobilní funkce, které jsme dříve nabízeli samostatně jako Weby Azure a Azure Mobile Services. Obsahuje také nové možnosti pro automatizaci obchodních procesů a hostování cloudových rozhraní API. Jako jediná integrovaná služba vám App Service umožňuje vytvářet různé komponenty – weby, back-endy mobilních aplikací, rozhraní RESTful API a firemní procesy – v jediném řešení.

Následující 4minutové video stručně shrnuje, v čem se App Service podobá dřívějším nabídkám Azure a co nabízí nového.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Proč používat App Service?
Toto jsou některé klíčové funkce a možnosti služby App Service:

* **Více jazyků a rozhraní:** App Service zahrnuje prvotřídní podporu pro ASP.NET, Node.js, Javu, PHP a Python. Na virtuálních počítačích App Service můžete také spouštět [Windows PowerShell a další skripty nebo spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md).
* **Optimalizace DevOps:** Můžete nastavit [průběžnou integraci a nasazení](../app-service-web/app-service-continuous-deployment.md) pomocí služeb Visual Studio Team Services, GitHub nebo BitBucket. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](../app-service-web/web-sites-staged-publishing.md). Máte možnost [testování A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Spravujte aplikace ve službě App Service pomocí prostředí [Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo [multiplatformního rozhraní příkazového řádku (CLI)](../xplat-cli-install.md).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](../app-service-web/web-sites-scale.md) nebo [horizontálního](../monitoring-and-diagnostics/insights-how-to-scale.md) navýšení kapacity. Aplikace je možné hostovat kdekoli v globální infrastruktuře datacenter Microsoftu a smlouva [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) pro App Service zaručuje vysokou dostupnost.
* **Připojení k platformám SaaS a lokálním datům:** Můžete si vybírat z více než 50 [konektorů](../connectors/apis-list.md) pro firemní systémy (například SAP, Siebel nebo Oracle), služby SaaS (například služby Salesforce nebo Office 365) a internetové služby (například Facebook nebo Twitter). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](../app-service-web/web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/).
* **Šablony aplikací**– Vyberte si z rozsáhlého seznamu šablon na webu [Azure Marketplace](https://azure.microsoft.com/marketplace/), které vám umožní používat průvodce k instalaci oblíbeného open source softwaru, jako je WordPress, Joomla nebo Drupal.
* **Integrace sady Visual Studio** – Vyhrazené nástroje v sadě Visual Studio zjednodušují práci při vytváření, nasazování a ladění.

## <a name="app-types-in-app-service"></a>Typy aplikací v App Service
App Service nabízí několik *typů aplikací*, z nichž každá je určena k hostování konkrétního typu úlohy:

* [**Webové aplikace**](../app-service-web/app-service-web-overview.md) – k hostování webů a webových aplikací
* [**Mobilní aplikace**](../app-service-mobile/app-service-mobile-value-prop.md) – k hostování back-endu mobilních aplikací
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) – k hostování rozhraní RESTful API.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) – k automatizaci obchodních procesů a integraci systémů a dat napříč cloudy bez psaní kódu.

Slovem *aplikace* se zde označují prostředky hostování vyhrazené pro spouštění určité úlohy nebo procesu. Pokud zde například zmiňujeme „webovou aplikaci“, pravděpodobně jste zvyklí chápat webovou aplikaci jako výpočetní prostředky i kód aplikace, které společně poskytují v prohlížeči příslušné funkce. V App Service ale *webová aplikace* představuje výpočetní prostředky, které Azure poskytuje k hostování kódu aplikace. 

Vaše aplikace se může skládat z více aplikací App Service různého druhu. Například pokud se vaše aplikace skládá z webového front-endu a back-endu RESTful API, můžete:

- Nasadit obojí (front-end i rozhraní API) do jedné webové aplikace.  
- Nasadit kód front-endu do webové aplikace a kód back-endu do aplikace API. 



## <a name="app-service-plans"></a>Plány služby App Service
[Plány služby App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) představují kolekci fyzických prostředků použitých k hostování vašich aplikací.

Plány služby App Service definují:

- **Oblast** (Západní USA, Východní USA atd.)
- **Počet škálování** (jedna, dvě, tři instance atd.)
- **Velikost instance** (Malá, Střední, Velká)
- **SKU** (Free, Shared, Basic, Standard, Premium)

Všechny aplikace přiřazené k **plánu služby App Service** sdílí službou definované prostředky a tak umožňují snížení nákladů při hostování více aplikací.

Váš **plán služby App Service** se může škálovat od skladových jednotek (SKU) úrovní **Free** a **Shared** po SKU úrovní **Basic**, **Standard** a **Premium** a vy při tom získáte přístup k dalším prostředkům a funkcím. Když bude váš plán služby App Service nastaven na úroveň **Basic** nebo vyšší, budete také moci řídit **velikost** a škálovat počet virtuálních počítačů.

**SKU** a **Škálování** plánu služby App Service určují cenu, ne počet aplikací hostovaných ve službě. 

Pokud potřebujete větší škálovatelnost a izolaci sítě, můžete aplikace spustit ve službě [App Service Environment](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Ceny
Informace o cenách služby App Service najdete v tématu [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Vyzkoušejte si App Service
[Vytvořte si ukázkovou webovou aplikaci, mobilní aplikaci nebo aplikaci logiky](https://azure.microsoft.com/try/app-service/), hodinu si s ní hrajte, a to bez platební karty, bez závazků a bez starostí.

Nebo si můžete otevřít [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/) a vyzkoušet některý z našich úvodních kurzů:

* [Kurz: Vytvoření webové aplikace](../app-service-web/app-service-web-get-started.md)
* [Kurz: Vytvoření mobilní aplikace](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Kurz: Vytvoření aplikace API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Kurz: Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)




<!--HONumber=Feb17_HO1-->


