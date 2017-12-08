---
title: "Azure AD v2 JS SPA na základě nastavení – Test | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: e10e5bbb035878eb62d9295b91263602f8128230
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

> ### <a name="testing-with-visual-studio"></a>Testování v sadě Visual Studio
> Pokud používáte Visual Studio, stiskněte klávesu `F5` ke spuštění projektu: Otevře se prohlížeč a přesměruje vám *http://localhost: {port}* kde uvidíte *volání Microsoft Graph API* tlačítko.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testování v Pythonu nebo jiný webový server
> Pokud nepoužíváte Visual Studio, ujistěte se, váš webový server je spuštěna a je nakonfigurován pro naslouchání na portu TCP založené na složku, která obsahuje vaše *index.html* souboru. Pro jazyk Python, můžete spustit naslouchání portu spuštěním v příkazu výzvy a Terminálový, ze složky aplikace:
> 
> ```bash
> python -m http.server 8080
> ```
>  Potom otevřete prohlížeč a zadejte *adrese http://localhost: 8080* nebo *http://localhost: {port}* – kde *port* odpovídá port, který váš webový server naslouchá. Měli byste vidět obsah vaší stránka index.html se *volání Microsoft Graph API* tlačítko.

## <a name="test-your-application"></a>Testování vaší aplikace

Po prohlížeč načte vaše *index.html*, klikněte *volání Microsoft Graph API* tlačítko. Pokud je poprvé, prohlížeč vás přesměruje na koncový bod v2 Microsoft Azure Active Directory, kde se zobrazí výzva k přihlášení.
 
![Snímek obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Vyjádřit souhlas.
Velmi při prvním přihlášení do aplikace, se zobrazí obrazovky souhlasu podobná následující příkaz, kde je nutné přijmout:

 ![Souhlas obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Očekávané výsledky
Měli byste vidět informace o profilu uživatele vrácený odpovědi volání Microsoft Graph API.
 
 ![Výsledky](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Můžete také zobrazit základní informace o tokenu získali v *tokenu přístupu* a *ID tokenu deklarací* polí.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována. Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory. Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele. Chcete-li získat přístup k kalendáře uživatele v rámci aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilent` volání. Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.

Pokud rozhraní API back-end nevyžaduje obor (nedoporučuje se), můžete použít `clientId` jako obor v `acquireTokenSilent` nebo `acquireTokenRedirect` volání.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]
