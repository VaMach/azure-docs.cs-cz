---
title: "Azure AD v2 Android získávání spuštěno – testování | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

1. Nasazení kódu do vašeho zařízení nebo emulátor.
2. Až budete připraveni k testování, používáte pro přihlášení Microsoft Azure Active Directory (účet organizace) nebo účet Account Microsoft (live.com, outlook.com). 

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Přihlášení](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Vyjádřit souhlas.
Při prvním přihlášení uživatele k vaší aplikaci, se zobrazí obrazovka podobná souhlasu s níže, kde se musí explicitně přijmout: 

![Vyjádřit souhlas.](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět výsledky volání Microsoft Graph API, mi na použije k získání uživatelský profil - https://graph.microsoft.com/v1.0/me koncový bod. Seznam běžných Microsoft Graph koncové body, najdete v tématu to [článku](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele. Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilentAsync` volání. Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.

<!--end-collapse-->
