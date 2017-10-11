---
title: "Azure AD v2 rozhraní ASP.NET Web Server získávání spuštěno – testování | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
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
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Otestujte svůj kód

Stiskněte klávesu `F5` spustit projekt v sadě Visual Studio. Prohlížeč a přesměruje na *http://localhost: {port}* kde se zobrazí *přihlásit pomocí Microsoft* tlačítko. Pokračujte a klikněte na něj pro přihlášení.

Až budete připraveni k testování, použijte pracovní nebo školní (Azure Active Directory) nebo osobní (live.com, outlook.com) účet pro přihlášení. 

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Očekávané výsledky
Po přihlášení se uživatel přesměruje na domovskou stránku vašeho webu, což je adresa URL HTTPS zadaná v informace o registraci aplikace v portálu pro registraci aplikace společnosti Microsoft. Tato stránka se teď zobrazuje *Hello {uživatele}* a odkaz na odhlášení a na odkaz zobrazíte deklaracích identity uživatele –, což je odkaz na kontroler autorizovat vytvořili dříve.

### <a name="see-users-claims"></a>V tématu deklarací identity uživatele
Vyberte hypertextový odkaz zobrazíte deklaracích identity uživatele. To vás řadiče a zobrazení, která je k dispozici pro uživatele, kteří se ověřují.

#### <a name="expected-results"></a>Očekávané výsledky
 Měli byste vidět tabulku obsahující základní vlastnosti pro přihlášeného uživatele:

| Vlastnost | Hodnota | Popis|
|---|---|---|
| Name (Název) | {Úplné uživatelské jméno} | Jméno a příjmení uživatele
|Uživatelské jméno | <span>user@domain.com</span>| Uživatelské jméno sloužící k identifikaci pro přihlášeného uživatele
| Předmět| {Subjektu}|Řetězec k jednoznačné identifikaci přihlášení uživatele na webu|
| ID tenanta| {Guid}| A *guid* jednoznačně představující uživatele organizaci Azure Active Directory.|

Kromě toho se zobrazí tabulku včetně všech deklarací identity uživatele zahrnuta v žádosti o ověření. Seznam všech deklarací identity do Id tokenu a vysvětlení najdete v tématu to [článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "seznam deklarací identity v Id tokenu").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test přístupu k metodě, která má *[Authorize]* atribut (volitelné)
V tomto kroku budete testovat přístup k řadiči ověřený jako anonymní uživatel:<br/>
Vyberte propojení na odhlášení uživatele a dokončete proces přihlášení.<br/>
Teď v prohlížeči zadejte http://localhost: {port} / ověřený přístup k řadiči, která je chráněná pomocí `[Authorize]` atribut

#### <a name="expected-results"></a>Očekávané výsledky
Měli byste obdržet řádku by bylo potřeba ověření zobrazíte zobrazení.

## <a name="additional-information"></a>Další informace

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Chránit celý web
Chcete-li chránit celý web, přidejte `AuthorizeAttribute` k `GlobalFilters` v `Global.asax` `Application_Start` metoda:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Jak chcete zabránit uživatelům ve pouze jedna organizace k přihlášení do aplikace**

> Ve výchozím nastavení osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má integrované s Azure Active Directory můžete přihlášení do aplikace. 

> Pokud chcete, aby aplikace tak, aby přijímal přihlášení z pouze jedna organizace Azure Active Directory, nahraďte `Tenant` parametr v *web.config* z `Common` k názvu klienta organizace – příklad, *contoso.onmicrosoft.com*. Potom změnit `ValidateIssuer` argument ve vaší *třídy pro spuštění OWIN* k `true`.

> Chcete-li povolit uživatelům jenom seznamu konkrétní organizací, nastavte `ValidateIssuer` na hodnotu true a použít `ValidIssuers` parametr pro zadání seznamu organizací.

> Další možností je implementovat vlastní metodu k ověření pomocí parametru IssuerValidator vystavitele. Další informace o `TokenValidationParameters`, najdete v tématu [to](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "článku na webu MSDN parametry tokenvalidationparameters") článku na webu MSDN.

