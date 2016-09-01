<properties
    pageTitle="Vytvoření aplikace Cordova v Azure App Service Mobile Apps | Microsoft Azure"
    description="V tomto kurzu začnete používat back-endy mobilních aplikací Azure pro vývoj na platformě Apache Cordova."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript, mobilní, klient" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="glenga"/>

#Vytvoření aplikace na platformě Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## Přehled

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu k mobilní aplikaci Apache Cordova pomocí back-endu mobilní aplikace Azure.  Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Apache Cordova, která bude představovat _seznam úkolů_ a ukládat data do Azure.

Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Apache Cordova.

## Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Počítač s nástrojem [Visual Studio Community 2015] nebo novějším
* [Visual Studio Tools for Apache Cordova]
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)

Visual Studio je možné obejít a používat přímo příkazový řádek platformy Apache Cordova.  To je užitečné v případě, že kurz budete absolvovat na počítači Mac.  Tento kurz se nezabývá kompilací klientských aplikací Apache Cordova pomocí příkazového řádku.

## Vytvoření nového back-endu mobilní aplikace Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Podívejte se na video zobrazující podobný postup.](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## Konfigurace serverového projektu

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Stažení a spuštění aplikace Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## Další kroky

Teď když jste dokončili tento kurz, jak rychle začít, přejděte k jednomu z následujících kurzů: 

* [Přidání ověřování] do aplikace Apache Cordova
* [Přidání nabízených oznámení] do aplikace Apache Cordova

Další informace o klíčových konceptech Azure App Service

* [Authentication]
* [Nabízená oznámení]

Zjistěte, jak používat sady SDK.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[portál Azure]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Přidání ověřování]: app-service-mobile-cordova-get-started-users.md
[Přidání nabízených oznámení]: app-service-mobile-cordova-get-started-push.md
[Authentication]: app-service-mobile-auth.md
[Nabízená oznámení]: ../notification-hubs/notification-hubs-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!----HONumber=Aug16_HO4-->


