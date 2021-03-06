---
title: "Kurz použití Azure Active Directory B2C k ochraně webového rozhraní API ASP.NET"
description: "Kurz popisující použití Active Directory B2C k ochraně webového rozhraní API ASP.NET a jeho volání z webové aplikace ASP.NET."
services: active-directory-b2c
author: PatAltimore
ms.author: patricka
ms.reviewer: saraford
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 0e9f324cec0d242c013a461d8580abd4faa97c8d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-use-azure-active-directory-b2c-to-protect-an-aspnet-web-api"></a>Kurz: Použití Azure Active Directory B2C k ochraně webového rozhraní API ASP.NET

V tomto kurzu se dozvíte, jak volat prostředek webového rozhraní API chráněný službou Azure Active Directory (Azure AD) B2C z webové aplikace ASP.NET.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Registrace webového rozhraní API v tenantovi Azure AD B2C
> * Definice a konfigurace oborů pro webové rozhraní API
> * Udělení oprávnění k webovému rozhraní API aplikaci
> * Aktualizace vzorového kódu tak, aby pomocí Azure AD B2C chránil webové rozhraní API

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

* Dokončete [kurz Použití Azure Active Directory B2C k ověřování uživatelů ve webové aplikaci ASP.NET](active-directory-b2c-tutorials-web-app.md).
* Nainstalujte sadu [Visual Studio 2017](https://www.visualstudio.com/downloads/) se sadou funkcí **Vývoj pro ASP.NET a web**.

## <a name="register-web-api"></a>Registrace webového rozhraní API

Prostředky webového rozhraní API je nejprve potřeba zaregistrovat ve vašem tenantovi, a až pak můžou přijímat a reagovat na [požadavky na chráněné prostředky](../active-directory/develop/active-directory-dev-glossary.md#resource-server) od [klientských aplikací](../active-directory/develop/active-directory-dev-glossary.md#client-application), které obsahují [přístupový token](../active-directory/develop/active-directory-dev-glossary.md#access-token) ze služby Azure Active Directory. Registrací se ve vašem tenantovi vytvoří [objekt aplikace a instanční objekt](../active-directory/develop/active-directory-dev-glossary.md#application-object). 

Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce vašeho tenanta Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Ze seznamu služeb na webu Azure Portal vyberte **Azure AD B2C**.

2. V nastavení B2C klikněte na **Aplikace** a potom klikněte na **+ Přidat**.

    K registraci ukázkového webového rozhraní API ve vašem tenantovi použijte následující nastavení.
    
    ![Přidání nového rozhraní API](media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Název** | My Sample Web API | Zadejte **Název**, který popíše vaše webové rozhraní API pro vývojáře. |
    | **Zahrnout webovou aplikaci nebo webové rozhraní API** | Ano | Pro webové rozhraní API vyberte **Ano**. |
    | **Povolit implicitní tok** | Ano | Vyberte **Ano**, protože rozhraní API používá [Přihlášení OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Adresa URL odpovědi** | `https://localhost:44332` | Adresy URL odpovědí jsou koncové body, kam Azure AD B2C vrací všechny tokeny, které vaše rozhraní API požaduje. V tomto kurzu se ukázkové webové rozhraní API spouští místně (localhost) a naslouchá na portu 44332. |
    | **Identifikátor URI ID aplikace** | myAPISample | Tento identifikátor URI jednoznačně identifikuje rozhraní API v tenantovi. Díky tomu můžete v jednom tenantovi registrovat více rozhraní API. [Obory](../active-directory/develop/active-directory-dev-glossary.md#scopes) řídí přístup k chráněnému prostředku rozhraní API a definují se pro jednotlivé identifikátory URI ID aplikace. |
    | **Nativní klient** | Ne | Vzhledem k tomu, že se jedná o webové rozhraní API, a ne nativního klienta, vyberte Ne. |
    
3. Kliknutím na **Vytvořit** své rozhraní API zaregistrujte.

Zaregistrovaná rozhraní API se zobrazí v seznamu aplikací pro příslušného tenanta Azure AD B2C. Vyberte ze seznamu vaše webové rozhraní API. Zobrazí se podokno vlastností webového rozhraní API.

![Vlastnosti webového rozhraní API](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Poznamenejte si **ID klienta aplikace**. Toto ID jednoznačně identifikuje rozhraní API a je potřeba při konfiguraci rozhraní API později v tomto kurzu.

Registrací webového rozhraní API v Azure AD B2C se nadefinuje vztah důvěryhodnosti. Vzhledem k tomu, že je rozhraní API zaregistrované v B2C, může teď důvěřovat přístupovým tokenům B2C přijímaným z jiných aplikací.

## <a name="define-and-configure-scopes"></a>Definice a konfigurace oborů

[Obory](../active-directory/develop/active-directory-dev-glossary.md#scopes) poskytují způsob řízení přístupu k chráněným prostředkům. Webové rozhraní API používá obory k implementaci řízení přístupu na základě oboru. Někteří uživatelé například můžou mít oprávnění ke čtení i zápisu, zatímco jiní uživatelé můžou mít oprávnění jen pro čtení. V tomto kurzu nadefinujete pro webové rozhraní API oprávnění ke čtení i zápisu.

### <a name="define-scopes-for-the-web-api"></a>Definice oborů pro webové rozhraní API

Zaregistrovaná rozhraní API se zobrazí v seznamu aplikací pro příslušného tenanta Azure AD B2C. Vyberte ze seznamu vaše webové rozhraní API. Zobrazí se podokno vlastností webového rozhraní API.

Klikněte na **Publikované obory (Preview)**.

Nakonfigurujte obory pro rozhraní API přidáním následujících záznamů. 

![obory definované ve webovém rozhraní API](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Nastavení      | Navrhovaná hodnota  | Popis                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Rozsah** | Hello.Read | Oprávnění ke čtení pro hello |
| **Rozsah** | Hello.Write | Oprávnění k zápisu pro hello |

Publikované obory je možné použít k udělení oprávnění k webovému rozhraní API klientské aplikaci.

### <a name="grant-app-permissions-to-web-api"></a>Udělení oprávnění k webovému rozhraní API aplikaci

Pokud chcete volat chráněné webové rozhraní API z aplikace, musíte aplikaci udělit oprávnění k rozhraní API. 

1. Na webu Azure Portal vyberte ze seznamu služeb **Azure AD B2C** a kliknutím na **Aplikace** zobrazte seznam zaregistrovaných aplikací.

2. Ze seznamu aplikací vyberte **My Sample Web App**, klikněte na **Přístup přes rozhraní API (Preview)** a pak na **Přidat**.

3. V rozevíracím seznamu **Vybrat rozhraní API** vyberte své zaregistrované webové rozhraní API **My Sample Web API**.

4. V rozevíracím seznamu **Vybrat obory** vyberte obory, které jste definovali v registraci webového rozhraní API.

    ![výběr oborů pro aplikaci](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Klikněte na **OK**.

Vaše aplikace **My Sample Web App** je zaregistrovaná a může volat rozhraní **My Sample Web API**. Uživatel se [ověří](../active-directory/develop/active-directory-dev-glossary.md#authentication) v Azure AD B2C, aby mohl webovou aplikaci použít. Webová aplikace získá z Azure AD B2C [udělení autorizace](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) pro přístup k chráněnému webovému rozhraní API.

## <a name="update-web-api-code"></a>Aktualizace kódu webového rozhraní API

Když je teď webové rozhraní API zaregistrované a máte definované obory, je potřeba nakonfigurovat kód webového rozhraní API tak, aby používalo vašeho tenanta Azure AD B2C. V tomto kurzu nakonfigurujete ukázkové webové rozhraní API. 

Ukázkové webové rozhraní API je součástí projektu, který jste stáhli v požadovaném kurzu [Použití Azure Active Directory B2C k ověřování uživatelů ve webové aplikaci ASP.NET](active-directory-b2c-tutorials-web-app.md). Pokud jste požadovaný kurz nedokončili, dokončete ho, než budete pokračovat.

Ukázkové řešení obsahuje dva projekty:

**Ukázková webová aplikace (TaskWebApp):** Webová aplikace umožňující vytvoření a úpravy seznamu úkolů. Tato webová aplikace používá k registraci nebo přihlašování uživatelů pomocí e-mailové adresy zásady **registrace nebo přihlašování**.

**Ukázková aplikace webového rozhraní API (TaskService):** Webové rozhraní API, které podporuje funkce vytvoření, čtení, aktualizace a odstranění seznamu úkolů. Toto webové rozhraní API je zabezpečené službou Azure AD B2C a volané webovou aplikací.

Ukázková webová aplikace a webové rozhraní API definují hodnoty konfigurace jako nastavení aplikace v souboru Web.config každého projektu.

Otevřete řešení **B2C-WebAPI-DotNet** v sadě Visual Studio.

### <a name="configure-the-web-app"></a>Konfigurace webové aplikace

1. Otevřete soubor **Web.config** v projektu **TaskWebApp**.

2. Pokud chcete rozhraní API spustit místně, jako **api:TaskServiceUrl** použijte nastavení pro localhost. Změňte soubor Web.config následujícím způsobem: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Nakonfigurujte identifikátor URI rozhraní API. Toto je identifikátor URI, pomocí kterého webová aplikace provede požadavek na rozhraní API. Nakonfigurujte také požadovaná oprávnění.

```C#
<add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
<add key="api:ReadScope" value="Hello.Read" />
<add key="api:WriteScope" value="Hello.Write" />
```

### <a name="configure-the-web-api"></a>Konfigurace webového rozhraní API

1. Otevřete soubor **Web.config** v projektu **TaskService**.

2. Nakonfigurujte rozhraní API tak, aby používalo vašeho tenanta.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Nastavte ID klienta na ID aplikace zaregistrované pro vaše rozhraní API.

    ```C#
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. Aktualizujte nastavení zásad s použitím názvu vygenerovaného při vytváření zásad registrace a přihlášení.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    ```

5. Nakonfigurujte nastavení oborů tak, aby odpovídaly oborům, které jste vytvořili na portálu.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample-web-app-and-web-api"></a>Spuštění ukázkové webové aplikace a webového rozhraní API

Je potřeba spustit jak projekt **TaskWebApp**, tak i projekt **TaskService**. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **Nastavit projekty po spuštění**. 
2. Vyberte přepínač **Více projektů po spuštění**.
3. U obou projektů změňte hodnotu **Akce** na **Spustit**.
4. Kliknutím na OK uložte konfiguraci.
5. Stisknutím klávesy **F5** spusťte obě aplikace. Obě aplikace se otevřou na samostatné kartě prohlížeče. Na adrese `https://localhost:44316/` je webová aplikace.
    Na adrese `https://localhost:44332/` je webové rozhraní API.

6. Ve webové aplikaci klikněte na odkaz pro registraci nebo přihlášení v banneru nabídky a zaregistrujte se do webové aplikace. Použijte účet, který jste vytvořili v [kurzu k webové aplikaci](active-directory-b2c-tutorials-web-app.md). 
7. Po přihlášení klikněte na odkaz **To-do list** (Seznam úkolů) a vytvořte položku seznamu úkolů.

Když vytvoříte položku seznamu úkolů, webová aplikace odešle do webového rozhraní API požadavek na vygenerování položky seznamu úkolů. Vaše chráněná webová aplikace volá chráněné webové rozhraní API ve vašem tenantovi Azure AD B2C.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Svého tenanta Azure AD B2C můžete použít i k vyzkoušení dalších kurzů k Azure AD B2C. Jakmile už ho nebudete potřebovat, můžete [svého tenanta Azure AD B2C odstranit](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Další kroky

Tento článek vás provedl ochranou webového rozhraní API ASP.NET prostřednictvím registrace a definice oborů v Azure AD B2C. Další podrobnosti o vývoji tohoto scénáře, včetně podrobného vysvětlení kódů, najdete v dalším kurzu.

> [!div class="nextstepaction"]
> [Vytvoření webové aplikace ASP.NET umožňující registraci, přihlášení, úpravy profilu a resetování hesla s využitím Azure Active Directory B2C](active-directory-b2c-devquickstarts-web-dotnet-susi.md)