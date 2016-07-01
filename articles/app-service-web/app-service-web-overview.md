<properties
    pageTitle="Přehled Web Apps | Microsoft Azure"
    description="Zjistěte, jak služba Azure App Service pomáhá nasazovat, hostovat a používat webové aplikace"
    services="app-service\web"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="tdykstra"/>

# Přehled Web Apps

*App Service Web Apps* je plně spravovaná výpočetní platforma, která je optimalizována pro hostování webů a webových aplikací. Tato nabídka typu [platforma jako služba](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) Microsoft Azure umožňuje zaměřit se na obchodní logiku, zatímco Azure zajistí infrastrukturu pro spouštění a škálování aplikací.

Následující 5minutové video představuje Azure App Service Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

## Co je webová aplikace ve službě App Service?

Ve službě App Service se *webovou aplikací* rozumí výpočetní prostředky, které Azure poskytuje k hostování webu či webové aplikace.  

V závislosti na zvolené cenové úrovni se výpočetní prostředky mohou nacházet ve sdílených nebo vyhrazených virtuálních počítačích (VM). Kód aplikace běží ve spravovaném virtuálním počítači, který je izolován od ostatních zákazníků.

Kód může být v libovolném jazyce nebo rozhraní podporovaném službou [Azure App Service](../app-service/app-service-value-prop-what-is.md), jako je ASP.NET, Node.js, Java, PHP nebo Python. Můžete také spouštět skripty, které ve webové aplikaci používají [PowerShell či jiné skriptovací jazyky](web-sites-create-web-jobs.md#acceptablefiles).

Příklady typických aplikačních scénářů, k nimž můžete použít Web Apps, naleznete v částech [Scénáře webových aplikací](https://azure.microsoft.com/documentation/scenarios/web-app/) a **Scénáře a doporučení** v tématu [Srovnání Azure App Service, Virtual Machines, Service Fabric a Cloud Services](choose-web-site-cloud-service-vm.md#scenarios).

## Proč používat službu Web Apps?

Toto jsou některé klíčové funkce služby App Service, které platí pro službu Web Apps:

- **Více jazyků a rozhraní** – Služba App Service obsahuje prvotřídní podporu pro ASP.NET, Node.js, Javu, PHP a Python. Ve virtuálních počítačích App Service můžete také spouštět [PowerShell a další skripty či spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md).

- **Optimalizace DevOps** – Nastavte [průběžnou integraci a nasazení](../app-service-web/app-service-continous-deployment.md) pomocí řešení Visual Studio Team Services, GitHub nebo BitBucket. Podporujte aktualizace prostřednictvím [testovacího a přípravného prostředí](../app-service-web/web-sites-staged-publishing.md). Provádějte [testování A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Spravujte aplikace ve službě App Service pomocí prostředí [Azure PowerShell](../powershell-install-configure.md) nebo [multiplatformního rozhraní příkazového řádku (CLI)](../xplat-cli-install.md).
 
- **Globální škálování s vysokou dostupností** – [Vertikálně](../app-service/app-service-scale.md) či [horizontálně](../azure-portal/insights-how-to-scale.md) navyšujte kapacitu, a to ručně i automaticky. Hostujte aplikace kdekoli v globální infrastruktuře datacenter společnosti Microsoft. Smlouva [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) služby App Service zaručuje vysokou dostupnost.

- **Připojení k platformám SaaS a místním datům** – Vyberte si z více než 50 [konektorů](../connectors/apis-list.md) pro podnikové systémy (například SAP, Siebel nebo Oracle), služeb SaaS (například služby Salesforce nebo Office 365) a internetových služeb (například Facebook nebo Twitter). Získejte přístup k místním datům pomocí [hybridních připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuálních sítí Azure](../app-service-web/web-sites-integrate-with-vnet.md).

- **Zabezpečení a dodržování předpisů** – Služba App Service je [kompatibilní se standardy ISO, SOC a PCI](https://www.microsoft.com/TrustCenter/).

- **Šablony aplikací** – Vyberte si z rozsáhlého seznamu šablon aplikací v [Azure Marketplace](https://azure.microsoft.com/marketplace/), které vám umožní používat průvodce k instalaci oblíbeného Open Source softwaru, jako je WordPress, Joomla či Drupal.

- **Integrace sady Visual Studio** – Vyhrazené nástroje v sadě Visual Studio zjednodušují práci při vytváření, nasazování a ladění.

Kromě toho může webová aplikace využívat funkce služeb [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (např. podpora CORS) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (např. nabízená oznámení). Další informace o typech aplikací ve službě App Service naleznete v tématu [Přehled služby Azure App Service](../app-service/app-service-value-prop-what-is.md).

Kromě Web Apps ve službě App Service nabízí Azure další služby, které lze použít k hostování webů a webových aplikací. Pro většinu scénářů je nejlepší volbou služba Web Apps.  V případě architektury mikroslužby zvažte službu [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric), a pokud potřebujete větší kontrolu nad virtuálními počítači, v nichž je kód spuštěn, zvažte službu [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Příklady typických aplikačních scénářů, k nimž můžete použít službu Web Apps, naleznete v tématu [Srovnání Azure App Service, Virtual Machines, Service Fabric a Cloud Services](choose-web-site-cloud-service-vm.md).

## Začínáme

Chcete-li začít nasazením ukázkového kódu do nové webové aplikace ve službě App Service, postupujte podle kurzu [Nasazení první webové aplikace do Azure během 5 minut](app-service-web-get-started.md). Budete potřebovat bezplatný účet Azure.

Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Není vyžadována platební karta a nevzniká žádný závazek.



<!--HONumber=Jun16_HO2-->


