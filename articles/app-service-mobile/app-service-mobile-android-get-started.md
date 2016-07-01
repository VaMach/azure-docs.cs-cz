<properties
    pageTitle="Vytvoření aplikace pro Android v Azure App Service Mobile Apps | Microsoft Azure"
    description="V tomto kurzu začnete používat back-endy mobilní aplikace Azure pro vývoj na platformě Android."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="yuaxu;ricksal"/>

#Vytvoření aplikace pro Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## Přehled

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Android pomocí back-endu mobilní aplikace Azure.  Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci pro Android, která bude představovat _seznam úkolů_ a ukládat data do Azure.

Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Android.

## Předpoklady

Pro absolvování tohoto kurzu potřebujete:

* [Android Developer Tools](https://developer.android.com/sdk/index.html), což zahrnuje integrované vývojové prostředí Android Studio a nejnovější platformu Android
* Sadu SDK Azure Mobile pro Android, na kterou se automaticky odkazuje jako na součást projektu pro rychlý start, který si stáhnete
* Počítač s nástrojem [Visual Studio Community 2013] nebo novějším &mdash; není potřeba pro back-end Node.js
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)

## Vytvoření nového back-endu mobilní aplikace Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## Konfigurace serverového projektu

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Stažení a spuštění aplikace pro Android

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]


<!-- Images. -->

<!-- URLs -->
[Portál Azure]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!--HONumber=Jun16_HO2-->


