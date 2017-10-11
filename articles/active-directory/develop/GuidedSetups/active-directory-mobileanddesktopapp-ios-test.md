---
title: "Azure AD v2 iOS Začínáme - Test | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 4a88096d2b0a23708acdbc1798eac528599b4f71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Test dotazování Microsoft Graph API z vaší aplikace iOS

Stiskněte klávesu `Command`  +  `R` spustí kód v simulátoru.

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Až budete připraveni k testování, klepněte na *'volání Microsoft Graph API,* a zobrazí se výzva k zadání uživatelského jména a hesla.

### <a name="consent"></a>Vyjádřit souhlas.
Při prvním přihlášení do aplikace, zobrazí obrazovka podobná souhlasu s níže, kde je potřeba explicitně přijmout:

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Očekávané výsledky
Informace o profilu uživatele vrácený Microsoft Graph API volat v byste měli vidět *protokolování* části.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele. Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilent` volání. Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->



