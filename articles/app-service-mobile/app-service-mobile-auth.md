---
title: "Ověřování a autorizaci v Azure Mobile Apps | Microsoft Docs"
description: "Reference konceptu a Přehled ověřování / autorizace funkcí pro Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: cfowler
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 90c11b09351f019c45f5f1b025d67947b69b3b0a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Ověřování a autorizace pro Azure Mobile Apps
## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / autorizace?
> [!NOTE]
> Toto téma se budou migrovat do konsolidované [aplikace služby ověřování / autorizace](../app-service/app-service-authentication-overview.md) téma, které obsahuje webové, mobilní a aplikace API.
> 
> 

Aplikace služby ověřování / autorizace je funkce, které umožní aplikaci k přihlášení uživatelé beze změn kódu na back-end aplikace vyžaduje. Poskytuje snadný způsob, jak chránit vaše aplikace a pracovat s daty na uživatele.

Služby App Service používá federovaných identit, ve kterém 3. stran **zprostředkovatele identity** ("IDP") ukládá účty a ověřuje uživatele a místo vlastní aplikace používá tuto identitu. App Service podporuje pět poskytovatelů identit ihned: *Azure Active Directory*, *Facebook*, *Google*, *Account Microsoft* , a *Twitter*. Tato podpora pro vaše aplikace můžete také rozšířit integrací jiný zprostředkovatel identity nebo řešení vlastní identitu.

Aplikace můžete využít libovolný počet těchto poskytovatelů identit vaši koncoví uživatelé můžou poskytovat s možnostmi pro jak se přihlásit.

Pokud chcete začít hned, naleznete v jednom z následujících kurzů:

* [Přidání ověřování do vaší aplikace pro iOS]
* [Přidání ověřování do aplikace Xamarin.iOS]
* [Přidání ověřování do aplikace Xamarin.Android]
* [Přidání ověřování do aplikace pro Windows]

## <a name="how-authentication-works"></a>Jak funguje ověřování
Aby bylo možné ověřit pomocí jednoho z poskytovatelů identity, musíte nejprve konfigurace poskytovatele identit vědět o vaší aplikaci. Poskytovatel identity vám potom poskytne ID a tajné klíče, které poskytují zpět do aplikace. To dokončí vztahu důvěryhodnosti a umožňuje aplikaci služby za účelem ověření identity, který mu je poskytnut.

Tyto kroky jsou podrobně popsané v následujících tématech:

* [Postup konfigurace aplikace použít Azure Active Directory přihlášení]
* [Postup konfigurace aplikace používat Facebook přihlášení]
* [Postup konfigurace aplikace používat Google přihlášení]
* [Postup konfigurace aplikace používat Account Microsoft přihlášení]
* [Postup konfigurace aplikace používat přihlášení služby Twitter.]

Jakmile je všechno nastavené na back-end, můžete upravit vašeho klienta k přihlášení. Existují dva přístupy tady:

* Použijete jeden řádek kódu, umožní Mobile Apps klienta SDK přihlášení uživatele.
* Využívejte sadu SDK publikovaná poskytovatelem danou identitu k vytvoření identity a přístup do služby App Service.

> [!TIP]
> Většinu aplikací měli použít poskytovatele sady SDK, chcete-li získat přihlašování nativní pocit a využít podporu aktualizace a jiné výhody specifický pro zprostředkovatele.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Jak funguje ověřování bez poskytovatele sady SDK
Pokud nechcete nastavit poskytovatele sady SDK, můžete povolit Mobile Apps provádět přihlášení. Mobilní aplikace klienta SDK se jim otevře webové zobrazení k poskytovateli dle vašeho výběru a dokončení přihlášení. Čas od času na blogy a fóra, které uvidíte, že tento označuje jako "serveru pohybu" nebo "směrované serveru pohybu" od serveru spravuje přihlašovací údaje a klient SDK nikdy obdrží token zprostředkovatele.

Kód potřebné ke spuštění tento tok je popsaná v tomto kurzu ověřování pro každou platformu. Na konci tok tokenu služby App Service má klient SDK a token se automaticky přiloží k všechny požadavky na back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Jak funguje ověřování u poskytovatele sady SDK
Práce s poskytovatele sady SDK umožňuje zkušenosti přihlásit k více úzce interakci s platformou OS aplikace běží na. To také umožňuje token zprostředkovatele a některé informace o uživateli na straně klienta, takže je mnohem jednodušší graf rozhraní API přehledná a přizpůsobit činnost koncového uživatele. Čas od času na blogy a fóra uvidíte tento označuje jako "tok klienta" nebo "přesměruje klienta pohybu" od kódu na straně klienta je zpracování přihlášení a kód klienta má přístup k tokenu zprostředkovatele.

Po získání tokenu zprostředkovatele musí být odeslány do služby App Service pro ověření. Na konci tok tokenu služby App Service má klient SDK a token se automaticky přiloží k všechny požadavky na back-end. Vývojář můžete také ponechat odkaz na token zprostředkovatele, pokud se tak rozhodne.

## <a name="how-authorization-works"></a>Jak funguje autorizace
Aplikace služby ověřování / autorizace zpřístupňuje několik možností **akci provést, když požadavek nebude ověřený**. Než kódu obdrží daného požadavku, může mít zkontrolujte služby App Service, pokud je požadavek ověřen a pokud ne, odmítněte ho a pokusit se uživatel přihlásit než to zkusíte znovu.

Jednou z možností je do mají neověřené přesměrovat požadavky do jednoho z poskytovatelů identit. Ve webovém prohlížeči to by ve skutečnosti trvat uživatele na novou stránku. Však tímto způsobem nelze přesměrovat mobilního klienta a obdrží neověřené odpovědi HTTP *401 – Neověřeno* odpovědi. Zadána to první požadavek díky vašeho klienta by mělo být vždy ke koncovému bodu přihlášení a proveďte volání pro jiná rozhraní API. Když zkusíte pro volání rozhraní API jiné než přihlašování, vašeho klienta dojde k chybě.

Pokud chcete mít podrobnější řízení, přes které koncové body vyžadují ověřování, můžete také vybrat "žádná akce (povolit žádosti o)" pro neověřené požadavky. Všechna rozhodnutí ověřování jsou v tomto případě odložení do kódu aplikace. To také umožňuje povolit přístup ke konkrétní uživatele na základě pravidel autorizace.

## <a name="documentation"></a>Dokumentace
Následující kurzy ukazují, jak přidat ověřování do vaší mobilních klientů pomocí služby App Service:

* [Přidání ověřování do vaší aplikace pro iOS]
* [Přidání ověřování do aplikace Xamarin.iOS]
* [Přidání ověřování do aplikace Xamarin.Android]
* [Přidání ověřování do aplikace pro Windows]

Následující kurzy ukazují, jak nakonfigurovat App Service využít zprostředkovatele různých ověřování:

* [Postup konfigurace aplikace použít Azure Active Directory přihlášení]
* [Postup konfigurace aplikace používat Facebook přihlášení]
* [Postup konfigurace aplikace používat Google přihlášení]
* [Postup konfigurace aplikace používat Account Microsoft přihlášení]
* [Postup konfigurace aplikace používat přihlášení služby Twitter.]

Pokud chcete použít systém identit než těm, které jsou zadané tady, můžete využít i [náhled podpora vlastní ověřování na serveru .NET SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Přidání ověřování do vaší aplikace pro iOS]: app-service-mobile-ios-get-started-users.md
[Přidání ověřování do aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Přidání ověřování do aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Přidání ověřování do aplikace pro Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Postup konfigurace aplikace použít Azure Active Directory přihlášení]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Postup konfigurace aplikace používat Facebook přihlášení]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Postup konfigurace aplikace používat Google přihlášení]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Postup konfigurace aplikace používat Account Microsoft přihlášení]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Postup konfigurace aplikace používat přihlášení služby Twitter.]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
