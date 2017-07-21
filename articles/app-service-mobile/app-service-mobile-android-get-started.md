---
title: "Vytvoření aplikace pro Android v Azure App Service Mobile Apps | Dokumentace Microsoftu"
description: "V tomto kurzu začnete používat back-endy mobilní aplikace Azure pro vývoj na platformě Android."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 7752cd091d7952a905001f14414eba84b90f0d51
ms.contentlocale: cs-cz
ms.lasthandoff: 03/09/2017

---
# <a name="create-an-android-app"></a>Vytvoření aplikace pro Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Android pomocí back-endu mobilní aplikace Azure.  Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci pro Android, která bude představovat *seznam úkolů* a ukládat data do Azure.

Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Android.

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu potřebujete:

* [Android Developer Tools](https://developer.android.com/sdk/index.html), což zahrnuje integrované vývojové prostředí Android Studio a nejnovější platformu Android
* Sadu SDK Azure Mobile pro Android, na kterou se automaticky odkazuje jako na součást projektu pro rychlý start, který si stáhnete
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>Vytvoření nového back-endu mobilní aplikace Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurace serverového projektu
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Stažení a spuštění aplikace pro Android
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

