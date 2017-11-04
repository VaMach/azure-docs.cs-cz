---
title: "Přehled Web Apps | Dokumentace Microsoftu"
description: "Zjistěte, jak služba Azure App Service pomáhá nasazovat, hostovat a používat webové aplikace"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Přehled Web Apps

*Azure App Service Web Apps* (nebo jenom webové aplikace) je služba pro hostování webových aplikací, rozhraní REST API, a končí mobile zpět. Můžete vyvíjet v svůj oblíbený jazyk, je-li jej .NET, .NET Core, Javy, Ruby, Node.js, PHP nebo Python. Můžete spustit a škálování aplikací se usnadnění v systému Windows nebo Linux virtuální počítače (najdete v části [služby App Service v systému Linux](containers/app-service-linux-intro.md)). 

Webové aplikace nejen přidá power ve službě Microsoft Azure k vaší aplikaci, například zabezpečení, Vyrovnávání zatížení, automatické škálování a automatizovat správu. Můžete také využít její možnosti DevOps, jako je například průběžné nasazování ze služby VSTS, Githubu, úložiště Docker Hub a další zdroje, správy balíčků pracovní prostředí, vlastní domény a certifikáty SSL. 

Službou App Service platíte za výpočetní prostředky, které používáte. Je dáno výpočetní prostředky, můžete použít _plán služby App Service_ spuštění webové aplikace. Další informace najdete v tématu [plánů služby App Service ve službě Azure Web Apps](azure-web-sites-web-hosting-plans-in-depth-overview.md).

Následující 5minutové video představuje Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Proč používat službu Web Apps?
Zde jsou některé klíčové funkce App Service Web Apps:

* **Více jazyků a rozhraní** -webové aplikace obsahuje prvotřídní podporu pro ASP.NET, ASP.NET Core, Javy, Ruby, Node.js, PHP nebo Python. Můžete také spouštět [prostředí PowerShell a další skripty či spustitelné soubory](web-sites-create-web-jobs.md) stejně jako na pozadí služby.
* **Optimalizace DevOps** -nastavit [průběžnou integraci a nasazení](app-service-continuous-deployment.md) s Visual Studio Team Services, GitHub, BitBucket, úložiště Docker Hub nebo Azure Container Service. Aktualizace lze podporovat prostřednictvím [testovacího a přípravného prostředí](web-sites-staged-publishing.md). Spravujte aplikace ve webové aplikace pomocí [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo [multiplatformního rozhraní příkazového řádku (CLI)](/cli/azure/install-azure-cli).
* **Globální škálování s vysokou dostupností:** Můžete ručně i automaticky škálovat pro účely [vertikálního](web-sites-scale.md) nebo [horizontálního](../monitoring-and-diagnostics/insights-how-to-scale.md) navýšení kapacity. Aplikace je možné hostovat kdekoli v globální infrastruktuře datacenter Microsoftu a smlouva [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) pro App Service zaručuje vysokou dostupnost.
* **Připojení k datům SaaS platformy a místní** – zvolte z více než 50 [konektory](../connectors/apis-list.md) pro firemní systémy (například SAP), služby SaaS (například Salesforce) a internetové služby (například Facebook). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](web-sites-integrate-with-vnet.md).
* **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/). Ověřuje uživatele pomocí [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) nebo s přihlášení prostřednictvím sociální sítě ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md), a [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Vytvoření [omezení podle IP adresy](app-service-ip-restrictions.md) a [Správa identit služby](app-service-managed-service-identity.md).
* **Šablony aplikací** – zvolte z rozsáhlého seznamu šablon aplikací [Azure Marketplace](https://azure.microsoft.com/marketplace/), jako je WordPress, Joomla nebo Drupal.
* **Integrace sady Visual Studio** – Vyhrazené nástroje v sadě Visual Studio zjednodušují práci při vytváření, nasazování a ladění.
* **Rozhraní API a mobilní funkce** – Web Apps poskytuje podporu CORS klíč pro scénáře rozhraní RESTful API a zjednodušuje tím, že povolení ověřování, synchronizaci dat offline, nabízená oznámení a další scénáře mobilní aplikace.
* **Bez serveru kódu** – spustit fragmentu kódu nebo skriptu na vyžádání bez nutnosti explicitně zřizovat nebo spravovat infrastrukturu a platit pouze pro dobu výpočtů kódu ve skutečnosti používá (viz [Azure Functions](/azure/azure-functions/)).

Kromě Web Apps ve službě App Service nabízí Azure další služby, které lze použít k hostování webů a webových aplikací. Pro většinu scénářů je nejlepší volbou služba Web Apps.  V případě architektury mikroslužby zvažte [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric). Pokud potřebujete větší kontrolu nad virtuálními počítači, které kód spuštěn, zvažte [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Příklady typických aplikačních scénářů, k nimž můžete použít službu Web Apps, naleznete v tématu [Srovnání Azure App Service, Virtual Machines, Service Fabric a Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Další kroky

Vytvoření první webové aplikace.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

