---
title: "Kurz použití Azure Active Directory B2C k ověřování uživatelů ve webové aplikaci ASP.NET"
description: "Kurz týkající se použití Azure Active Directory B2C k registraci a přihlašování uživatelů ve webové aplikaci ASP.NET."
services: active-directory-b2c
author: PatAltimore
ms.author: patricka
ms.reviewer: saraford
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: ee006476f9e40e9d1a6e7213cb1881ca46ea75c2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-authenticate-users-with-azure-active-directory-b2c-in-an-aspnet-web-app"></a>Kurz: Ověřování uživatelů pomocí Azure Active Directory B2C ve webové aplikaci ASP.NET

V tomto kurzu se dozvíte, jak použít Azure Active Directory (Azure AD) B2C k registraci a přihlašování uživatelů ve webové aplikaci ASP.NET. Azure AD B2C umožňuje aplikacím provádět ověřování účtů na sociálních sítích, podnikových účtů a účtů Azure Active Directory s využitím protokolů s otevřenými standardy.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zaregistrovat ukázkovou webovou aplikaci ASP.NET ve svém tenantovi Azure AD B2C.
> * Vytvořit zásady pro registraci a přihlašování uživatelů, úpravy profilu a resetování hesla.
> * Nakonfigurovat ukázkovou webovou aplikaci pro použití vašeho tenanta Azure AD B2C. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

* Vytvořte si vlastního [tenanta Azure AD B2C](active-directory-b2c-get-started.md).
* Nainstalujte sadu [Visual Studio 2017](https://www.visualstudio.com/downloads/) se sadou funkcí **Vývoj pro ASP.NET a web**.

## <a name="register-web-app"></a>Registrace webové aplikace

Aby aplikace mohly přijímat [přístupové tokeny](../active-directory/develop/active-directory-dev-glossary.md#access-token) z Azure Active Directory, musí být [zaregistrované](../active-directory/develop/active-directory-dev-glossary.md#application-registration) ve vašem tenantovi. Registrací se pro aplikaci vytvoří [ID aplikace](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) ve vašem tenantovi. 

Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce vašeho tenanta Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Ze seznamu služeb na webu Azure Portal vyberte **Azure AD B2C**.

2. V nastavení B2C klikněte na **Aplikace** a pak klikněte na **Přidat**.

    K registraci ukázkové webové aplikace ve vašem tenantovi použijte následující nastavení.

    ![Přidání nové aplikace](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Název** | My Sample Web App | Zadejte **Název**, který popíše vaši aplikaci pro zákazníky. | 
    | **Zahrnout webovou aplikaci nebo webové rozhraní API** | Ano | Pro webovou aplikaci vyberte **Ano**. |
    | **Povolit implicitní tok** | Ano | Vyberte **Ano**, protože aplikace používá [Přihlášení OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **Adresa URL odpovědi** | `https://localhost:44316` | Adresy URL odpovědí jsou koncové body, kam Azure AD B2C vrací všechny tokeny, které vaše aplikace požaduje. V tomto kurzu se ukázka spouští místně (localhost) a naslouchá na portu 44316. |
    | **Nativní klient** | Ne | Vzhledem k tomu, že se jedná o webovou aplikaci, a ne nativního klienta, vyberte Ne. |

3. Kliknutím na **Vytvořit** svou aplikaci zaregistrujte.

Zaregistrované aplikace se zobrazí v seznamu aplikací pro příslušného tenanta Azure AD B2C. Vyberte ze seznamu vaši webovou aplikaci. Zobrazí se podokno vlastností webové aplikace.

![Vlastnosti webové aplikace](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Poznamenejte si **ID klienta aplikace**. Toto ID jednoznačně identifikuje aplikaci a je potřeba při konfiguraci aplikace později v tomto kurzu.

### <a name="create-a-client-password"></a>Vytvoření hesla klienta

Azure AD B2C pro [klientské aplikace](../active-directory/develop/active-directory-dev-glossary.md#client-application) používá autorizaci OAuth2. Webové aplikace jsou [důvěrní klienti](../active-directory/develop/active-directory-dev-glossary.md#web-client) a vyžadují tajný klíč klienta (heslo). ID klienta aplikace a tajný klíč klienta se používají při ověřování webové aplikace v Azure Active Directory. 

1. Vyberte stránku Klíče pro zaregistrovanou webovou aplikaci a klikněte na **Vygenerovat klíč**.

2. Kliknutím na **Uložit** zobrazte klíč.

    ![stránka aplikace pro generování klíčů](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

Klíč se na portálu zobrazí pouze jednou. Je důležité hodnotu klíče zkopírovat a uložit. Tuto hodnotu budete potřebovat ke konfiguraci vaší aplikace. Uložte klíč na bezpečném místě. Klíč veřejně nesdílejte.

## <a name="create-policies"></a>Vytvoření zásad

Zásada Azure AD B2C definuje pracovní postupy uživatelů. Běžnými pracovními postupy jsou například registrace, přihlášení, změna hesla a úpravy profilu.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Vytvoření zásady registrace nebo přihlašování

Pro registraci uživatelů, která jim umožní přístup k přihlášení k webové aplikaci, vytvořte **zásadu registrace nebo přihlašování**.

1. Na stránce portálu Azure AD B2C vyberte **Zásady registrace nebo přihlašování** a klikněte na **Přidat**.

    Ke konfiguraci zásady použijte následující nastavení:

    ![Přidání zásady registrace nebo přihlašování](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Název** | SiUpIn | Zadejte **Název** zásady. K názvu zásady se přidá předpona **b2c_1_**. Úplný název zásady **b2c_1_SiUpIn** použijete ve vzorovém kódu. | 
    | **Zprostředkovatel identity** | E-mailová registrace | Zprostředkovatel identity sloužící k jednoznačné identifikaci uživatele. |
    | **Atributy registrace** | Zobrazované jméno a PSČ | Vyberte atributy, které se při registraci shromáždí od uživatele. |
    | **Deklarace identity aplikace** | Zobrazované jméno, PSČ, Uživatel je nový, ID objektu uživatele | Vyberte [deklarace identity](../active-directory/develop/active-directory-dev-glossary.md#claim), které chcete zahrnout do [přístupového tokenu](../active-directory/develop/active-directory-dev-glossary.md#access-token). |

2. Kliknutím na **Vytvořit** vytvořte zásadu. 

### <a name="create-a-profile-editing-policy"></a>Vytvoření zásady upravování profilu

Pokud chcete uživatelům umožnit resetovat informace o svém profilu uživatele, vytvořte **zásadu upravování profilu**.

1. Na stránce portálu Azure AD B2C vyberte **Zásady upravování profilu** a klikněte na **Přidat**.

    Ke konfiguraci zásady použijte následující nastavení:

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Název** | SiPe | Zadejte **Název** zásady. K názvu zásady se přidá předpona **b2c_1_**. Úplný název zásady **b2c_1_SiPe** použijete ve vzorovém kódu. | 
    | **Zprostředkovatel identity** | Registrace místního účtu | Zprostředkovatel identity sloužící k jednoznačné identifikaci uživatele. |
    | **Atributy profilu** | Zobrazované jméno a PSČ | Vyberte atributy, které můžou uživatelé při úpravě profilu změnit. |
    | **Deklarace identity aplikace** | Zobrazované jméno, PSČ, Uživatel je nový, ID objektu uživatele | Vyberte [deklarace identity](../active-directory/develop/active-directory-dev-glossary.md#claim), které chcete zahrnout do [přístupového tokenu](../active-directory/develop/active-directory-dev-glossary.md#access-token) po úspěšné úpravě profilu. |

2. Kliknutím na **Vytvořit** vytvořte zásadu. 

### <a name="create-a-password-reset-policy"></a>Vytvoření zásady resetování hesla

Pokud chcete ve své aplikaci povolit resetování hesla, musíte vytvořit **zásadu resetování hesla**. Tato zásada popisuje uživatelské prostředí pro resetování hesla a obsah tokenů, které bude aplikace přijímat po úspěšném dokončení.

1. Na stránce portálu Azure AD B2C vyberte **Zásady resetování hesla** a klikněte na **Přidat**.

    Ke konfiguraci zásady použijte následující nastavení.

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Název** | SSPR | Zadejte **Název** zásady. K názvu zásady se přidá předpona **b2c_1_**. Úplný název zásady **b2c_1_SSPR** použijete ve vzorovém kódu. | 
    | **Zprostředkovatel identity** | Resetování hesla s použitím e-mailové adresy | Toto je zprostředkovatel identity sloužící k jednoznačné identifikaci uživatele. |
    | **Deklarace identity aplikace** | ID objektu uživatele | Vyberte [deklarace identity](../active-directory/develop/active-directory-dev-glossary.md#claim), které chcete zahrnout do [přístupového tokenu](../active-directory/develop/active-directory-dev-glossary.md#access-token) po úspěšném resetování hesla. |

2. Kliknutím na **Vytvořit** vytvořte zásadu. 

## <a name="update-web-app-code"></a>Aktualizace kódu webové aplikace

Když teď máte zaregistrovanou webovou aplikaci a vytvořené zásady, musíte svou aplikaci nakonfigurovat tak, aby používala vašeho tenanta Azure AD B2C. V tomto kurzu nakonfigurujete ukázkovou webovou aplikaci. 

[Stáhněte soubor .zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) nebo naklonujte ukázkovou webovou aplikaci z GitHubu.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Ukázková webová aplikace ASP.NET je jednoduchá aplikace seznamu úkolů umožňující vytvoření a aktualizaci seznamu úkolů. Aplikace umožňuje uživatelům zaregistrovat se k používání aplikace ve vašem tenantovi Azure AD B2C s využitím [komponent middlewaru Microsoft OWIN](https://docs.microsoft.com/en-us/aspnet/aspnet/overview/owin-and-katana/). Díky vytvoření zásady Azure AD B2C můžou uživatelé jako svou identitu pro přístup k aplikaci použít účet na sociální síti nebo si můžou vytvořit účet. 

Ukázkové řešení obsahuje dva projekty:

**Ukázková webová aplikace (TaskWebApp):** Webová aplikace umožňující vytvoření a úpravy seznamu úkolů. Tato webová aplikace používá k registraci nebo přihlašování uživatelů pomocí e-mailové adresy zásady **registrace nebo přihlašování**.

**Ukázková aplikace webového rozhraní API (TaskService):** Webové rozhraní API, které podporuje funkce vytvoření, čtení, aktualizace a odstranění seznamu úkolů. Toto webové rozhraní API je zabezpečené službou Azure AD B2C a volané webovou aplikací.

Aplikaci musíte změnit tak, aby používala registraci aplikace ve vašem tenantovi. Musíte také nakonfigurovat zásady, které jste vytvořili. Ukázková webová aplikace definuje hodnoty konfigurace jako nastavení aplikace v souboru Web.config. Nastavení aplikace můžete změnit následujícím způsobem:

1. Otevřete řešení **B2C-WebAPI-DotNet** v sadě Visual Studio.

2. V projektu webové aplikace **TaskWebApp** otevřete soubor **Web.config** a proveďte následující aktualizace:

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    
    <add key="ida:ClientId" value="The Application ID for your web app registered in your tenant" />
    
    <add key="ida:ClientSecret" value="Client password (client secret)" />
    ```
3. Aktualizujte nastavení zásad s použitím názvů vygenerovaných při vytváření zásad.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    <add key="ida:EditProfilePolicyId" value="b2c_1_SiPe" />
    <add key="ida:ResetPasswordPolicyId" value="b2c_1_SSPR" />
    ```

## <a name="run-the-sample-web-app"></a>Spuštění ukázkové webové aplikace

V Průzkumníku řešení klikněte pravým tlačítkem na projekt **TaskWebApp** a klikněte na **Nastavit jako spouštěný projekt**.

Stisknutím klávesy **F5** spusťte webovou aplikaci. Spustí se výchozí prohlížeč a otevře se adresa místního webu `https://localhost:44316/`. 

Ukázková aplikace podporuje registraci, přihlašování, úpravy profilu a resetování hesla. Následuje příklad registrace uživatele k používání aplikace pomocí e-mailové adresy. Jiné scénáře si můžete zkusit sami.

### <a name="sign-up-using-an-email-address"></a>Registrace pomocí e-mailové adresy

1. Kliknutím na odkaz **Sign up / Sign in** (Registrace / Přihlášení) v horním banneru se zaregistrujte jako uživatel webové aplikace. Tady se používá zásada **b2c_1_SiUpIn**, kterou jste definovali v předchozím kroku.

2. Azure AD B2C zobrazí přihlašovací stránku s odkazem na registraci. Protože ještě nemáte účet, klikněte na odkaz **Sign up now** (Zaregistrovat se). 

3. Pracovní postup registrace zobrazí stránku pro shromáždění a ověření identity uživatele pomocí e-mailové adresy. Pracovní postup registrace shromažďuje také heslo uživatele a požadované atributy definované v zásadě.

    Použijte platnou e-mailovou adresu a proveďte ověření pomocí ověřovacího kódu. Nastavte heslo. Zadejte hodnoty požadovaných atributů. 

    ![Pracovní postup registrace](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Kliknutím na **Create** (Vytvořit) vytvořte místní účet v tenantovi Azure AD B2C.

Teď se uživatel může přihlásit pomocí své e-mailové adresy a používat webovou aplikaci.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Svého tenanta Azure AD B2C můžete použít i k vyzkoušení dalších kurzů k Azure AD B2C. Jakmile už ho nebudete potřebovat, můžete [svého tenanta Azure AD B2C odstranit](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak vytvořit tenanta Azure AD B2C, vytvořit zásady a aktualizovat ukázkovou webovou aplikaci tak, aby používala vašeho tenanta Azure AD B2C. V dalším kurzu zjistíte, jak zaregistrovat, nakonfigurovat a volat webové rozhraní API ASP.NET chráněné vaším tenantem Azure AD B2C.

> [!div class="nextstepaction"]
> [Použití Azure Active Directory B2C k ochraně webového rozhraní API ASP.NET](active-directory-b2c-tutorials-web-api.md)
