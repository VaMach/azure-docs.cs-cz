---
title: Principy Manifest aplikace Azure Active Directory | Microsoft Docs
description: "Podrobné pokrytí manifestu aplikace Azure Active Directory, která představuje konfiguraci identity aplikace v klientovi služby Azure AD a se používá k zajištění OAuth autorizace, souhlasu prostředí a další."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: d5e18f41d6eb69ccb7eafaa4de2646c4c38df5e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-azure-active-directory-application-manifest"></a>Principy manifest aplikace Azure Active Directory
Aplikace, které se integrují s Azure Active Directory (AD) musí být zaregistrován u klienta služby Azure AD, poskytuje konfiguraci trvalé identity pro aplikaci. Tato konfigurace je konzultaci za běhu, povolení scénáře, které umožňují, aby aplikace a externí zprostředkovatel ověřování/autorizace služby prostřednictvím služby Azure AD. Další informace o modelu aplikace Azure AD najdete v tématu [přidání, aktualizace a odebrání aplikace] [ ADD-UPD-RMV-APP] článku.

## <a name="updating-an-applications-identity-configuration"></a>Aktualizuje se konfigurace identity aplikace
Ve skutečnosti více možností pro nejsou k dispozici aktualizace vlastností konfigurace identity aplikace, které se liší v možnosti a stupňů potíže, včetně následujících:

* **[Portál Azure] [ AZURE-PORTAL] webové uživatelské rozhraní** umožňuje aktualizovat nejběžnější vlastností aplikace. To je chyba nejrychlejší a minimálně náchylné k chybám způsob aktualizace vlastnosti aplikace, ale není poskytnuta úplný přístup na všechny vlastnosti, jako jsou následující dvě metody.
* Pro pokročilejší scénáře, kde je potřeba aktualizovat vlastnosti, které nejsou přístupné na portálu Azure classic, můžete upravit **manifest aplikace**. Toto je fokus v tomto článku a je podrobněji popsána spouštění v další části.
* Je také možné **zápisu aplikace, která používá [rozhraní Graph API] [ GRAPH-API]**  aktualizovat aplikace, které vyžaduje nejvíce úsilí. To může být atraktivní možnosti i když, pokud jsou zápis software pro správu, nebo muset aktualizovat vlastnosti aplikace v pravidelných intervalech automatizovaně.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Aktualizace konfigurace identity aplikace pomocí manifest aplikace
Prostřednictvím [portál Azure][AZURE-PORTAL], konfigurace identity aplikace můžete spravovat aktualizace pomocí editoru manifestu vložené manifest aplikace. Můžete také stáhnout a nahrát manifest aplikace jako soubor ve formátu JSON. Žádný skutečný soubor je uložen v adresáři. Manifest aplikace je jenom metody GET protokolu HTTP operace u entity aplikace Azure AD Graph API a nahrávání je operace HTTP PATCH u entity aplikací.

Výsledkem je, aby bylo možné porozumět formátu a vlastnosti manifestu aplikace, budete muset odkazovat na rozhraní Graph API [aplikace entity] [ APPLICATION-ENTITY] dokumentaci. Příklady aktualizace, které lze provést, i když nahrávání manifest aplikace:

* **Deklarovat obory oprávnění (oauth2Permissions)** vystavené webového rozhraní API. V tématu "Vystavení webové rozhraní API pro další aplikace" v [integrace aplikací s Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informace o implementaci vystupovat jako jiný uživatel pomocí oauth2Permissions delegovaná oprávnění oboru. Jak je uvedeno nahoře, vlastností entity aplikací jsou popsané v rozhraní Graph API [Entity a komplexní typ] [ APPLICATION-ENTITY] článku, včetně oauth2Permissions vlastnost, která je kolekce z typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarovat aplikační role (appRoles) vystavené aplikace**. Vlastnost entity aplikace appRoles je kolekce typu [aplikační role][APPLICATION-ENTITY-APP-ROLE]. Najdete v článku [řízení přístupu v cloudových aplikacích pomocí služby Azure AD na základě Role] [ RBAC-CLOUD-APPS-AZUREAD] článku příklad implementace.
* **Deklarovat známé klienta aplikace (knownClientApplications)**, které umožňují logicky tie souhlasu zadaného klienta aplikace na prostředek nebo webové rozhraní API.
* **Žádosti o službě Azure AD vydat deklaraci členství ve skupině** pro přihlášeného uživatele (groupMembershipClaims).  Můžete nakonfigurovat také vystavovat deklarace identity o členství role uživatele adresáře. Najdete v článku [autorizace v cloudových aplikací pomocí skupiny AD] [ AAD-GROUPS-FOR-AUTHORIZATION] článku příklad implementace.
* **Aby vaše aplikace podporují OAuth 2.0 implicitní grant** toků (oauth2AllowImplicitFlow). Tento typ udělení toku se používá s embedded webové stránky JavaScript nebo jedné stránky aplikace (SPA). Další informace o poskytnutí implicitní autorizace najdete v tématu [pochopení OAuth2 implicitní tok v Azure Active Directory poskytování][IMPLICIT-GRANT].
* **Povolit použití X509 certifikáty jako tajný klíč** (keyCredentials). Najdete v článku [vytvářet aplikace, služby a démon v Office 365] [ O365-SERVICE-DAEMON-APPS] a [– Příručka vývojáře pro ověřování s rozhraním API pro Azure Resource Manager] [ DEV-GUIDE-TO-AUTH-WITH-ARM] články Příklady implementace.
* **Přidat nový identifikátor ID URI aplikace** pro vaši aplikaci (identifierURIs[]). Identifikátory URI ID aplikace používají k jedinečné identifikaci aplikace v rámci jeho klienta Azure AD (nebo napříč více klientů Azure AD, pro víc klientů scénáře při kvalifikovaný prostřednictvím ověřené vlastní domény). Používají se při vyžadování oprávnění na prostředek aplikace nebo získání tokenu přístupu pro aplikaci prostředků. Při aktualizaci tento prvek se stejnou aktualizaci přišla odpovídající instanční objekt servicePrincipalNames [] – kolekce, které je umístěn v domácí klienta aplikace.

Manifest aplikace také poskytuje vhodný způsob, jak sledovat stav registrace vaší aplikace. Protože je k dispozici ve formátu JSON, lze do zdrojového kódu, společně s zdrojový kód aplikace zkontrolovat reprezentace souboru.

## <a name="step-by-step-example"></a>Krok příklad krok
Umožňuje teď provede kroky potřebné k aktualizaci konfigurace identity aplikace prostřednictvím manifest aplikace. Zvýrazníme jeden z předchozích ukázkách znázorňující na prostředek aplikace deklarovat nový obor oprávnění:

1. Přihlaste se na web [Azure Portal][AZURE-PORTAL].
2. Po došlo k ověření, vyberte vyberete v pravém horním rohu stránky klientovi Azure AD.
3. Vyberte **Azure Active Directory** rozšíření z levého navigačního panelu a klikněte na **registrace aplikace**.
4. Vyhledávání aplikace, které chcete aktualizovat v seznamu a klikněte na něj.
5. Na stránce aplikace klikněte na tlačítko **Manifest** otevřete editor pro vložené manifestu. 
6. Manifest pomocí tohoto editoru lze přímo upravit. Všimněte si, že manifest dodržuje schéma pro [aplikace entity] [ APPLICATION-ENTITY] jak jsme už zmínili dřív: například za předpokladu, že chceme implementace/zveřejněte nové oprávnění názvem "Employees.Read.All" na našem prostředků aplikace (API), jednoduše přidejte element nové za sekundu kolekce oauth2Permissions ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Položka musí být jedinečný, a proto musíte vygenerovat nový globálně jedinečné ID (GUID) pro `"id"` vlastnost. V takovém případě vzhledem k tomu, že jsme zadali `"type": "User"`, toto oprávnění může být souhlas pomocí libovolného účtu Azure AD ověřit klienta v aplikaci prostředků nebo rozhraní API je zaregistrován. Tím získají klienta aplikace oprávnění k přístupu jménem účtu. Popis a zobrazovaný název řetězce se používají během souhlasu a pro zobrazení na portálu Azure.
6. Po dokončení aktualizace manifest, klikněte na tlačítko **Uložit** uložit manifest.  
   
Teď, když je uložen v manifestu, můžete udělit registrovaný klientské aplikaci přístup k nové oprávnění jsme přidali výše. Tentokrát webového uživatelského rozhraní portálu Azure můžete použít neupravujte manifest aplikace klienta:  

1. Přejděte do okna **nastavení** klientské aplikace, do které chcete přidat přístup k novým rozhraním API, klikněte na **požadovaných oprávnění** a zvolte **vybrat rozhraní API**.
2. Pak se zobrazí se seznam registrovaných prostředků aplikace (API) v klientovi. Klikněte na aplikaci prostředků, vyberte nebo zadejte název aplikace do vyhledávacího pole. Když jste najde aplikaci, klikněte na tlačítko **vyberte**.  
3. Bude to trvat, abyste **vyberte oprávnění** stránku, která se zobrazí v seznamu oprávnění aplikací a delegovaná oprávnění k dispozici pro aplikaci prostředků. Vyberte nové oprávnění, aby bylo možné přidat do seznamu klienta požadovaná oprávnění. Toto nové oprávnění se uloží v konfiguraci identity klientské aplikace, ve vlastnosti kolekce "requiredResourceAccess".


A je to. Teď vaše aplikace bude spuštěna s použitím jejich novou konfiguraci identity.

## <a name="next-steps"></a>Další kroky
* Další informace o vztah mezi objekty aplikace a instanční objekt aplikace v [aplikace a služby hlavní objekty ve službě Azure AD][AAD-APP-OBJECTS].
* Najdete v článku [Glosář vývojáře Azure AD] [ AAD-DEVELOPER-GLOSSARY] definice některých koncepty pro vývojáře Azure Active Directory (AD) jádra.

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

