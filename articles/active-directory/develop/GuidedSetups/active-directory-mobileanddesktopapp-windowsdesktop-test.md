---
title: "Azure AD v2 Windows Desktop získávání spuštěno – testování | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 972cc48057c13271d725b0c973c3ccf651ad27c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

Chcete-li otestovat aplikaci, stiskněte klávesu `F5` spustit projekt v sadě Visual Studio. Hlavní okno by měl zobrazit:

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Až budete připraveni k testování, klikněte na tlačítko *volání Microsoft Graph API* a přihlaste se pomocí Microsoft Azure Active Directory (účet organizace) nebo účet Account Microsoft (live.com, outlook.com). Ho je poprvé, zobrazí se okno s výzvou, uživatel k přihlášení:

![Přihlášení](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Vyjádřit souhlas.
Při prvním přihlášení do aplikace, zobrazí obrazovka podobná souhlasu s níže, kde je potřeba explicitně přijmout:

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět informace o profilu uživatele vrácený Microsoft Graph API volání na obrazovce výsledky volání rozhraní API.

Měli byste taky vidět základní informace o tokenu získaných prostřednictvím `AcquireTokenAsync` nebo `AcquireTokenSilentAsync` informace o tokenu pole:

|Vlastnost  |Formát  |Popis |
|---------|---------|---------|
|Name (Název) | {Úplné uživatelské jméno} |Jméno a příjmení uživatele|
|Uživatelské jméno |<span>user@domain.com</span> |Uživatelské jméno používá k identifikaci uživatele|
|Platnost tokenu vyprší |{{Data a času}         |Čas, kdy vyprší platnost tokenu. MSAL bude prodloužit datum vypršení platnosti pro vás obnovením token, pokud je to nezbytné|
|Přístupový token |{Řetězec}         |Řetězec tokenu odeslány, budou odeslány na požadavky HTTP, které vyžadují hlavičku autorizace|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění
Rozhraní Graph API vyžaduje `user.read` obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé jiné rozhraní Graph API a také vlastní rozhraní API pro back-end serveru vyžadovat další obory. Například pro graf `Calendars.Read` je vyžadován ke kalendářům uživatele seznamu. Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, budete muset přidat `Calendars.Read` delegovat informace o registraci aplikace a pak přidejte `Calendars.Read` k `AcquireTokenAsync` volání. Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->



