---
title: "Zabezpečení vlastních konektorů s Azure AD - Azure Logic Apps | Microsoft Docs"
description: "Přidat ověřování a zabezpečení rozhraní API a konektor s Azure Active Directory (Azure AD)"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: bcee842cb880002daaa3ae9d9110ee1734941b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Zvýšit zabezpečení rozhraní API a konektor s Azure Active Directory (Azure AD)

K zabezpečení volání mezi rozhraní API a vlastní konektor, přidejte do vašeho webového rozhraní API a vaše konektor ověřování Azure AD. V tomto scénáři vytvoříte dvě aplikace Azure Active Directory (Azure AD) – jednu aplikaci Azure AD zabezpečuje webové rozhraní API, zatímco jiné aplikace Azure AD zabezpečuje registrace konektoru a přidá Delegovaný přístup. 

Tento kurz používá jako příklad rozhraní API služby Azure Resource Manager. Azure Resource Manager můžete spravovat součásti pro řešení, které jste vytvořili v Azure, jako je například databáze, virtuální počítače a webové aplikace. Vlastní konektor pro Azure Resource Manager může být užitečné, pokud chcete ke správě prostředků Azure z vaše pracovní postupy. Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Pokud nemáte předplatné, můžete začít s [bezplatný účet Azure](https://azure.microsoft.com/free/). Jinak, zaregistrujte si [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* V tomto kurzu [OpenAPI ukázkový soubor pro Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Ukázkový soubor OpenAPI nedefinuje všechny operace Azure Resource Manager a aktuálně obsahuje pouze operace pro [seznam všech odběrů](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Můžete upravit tento OpenAPI nebo vytvořte jinou OpenAPI soubor pomocí [online OpenAPI editor](http://editor.swagger.io/).
  >
  > V tomto kurzu můžete použít pro všechny rozhraní RESTful API, kde chcete použít ověřování Azure AD.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Vytvoření první aplikace Azure AD pro zabezpečení vašeho webového rozhraní API

Pokud vlastní konektor volání rozhraní API, což je rozhraní API služby Azure Resource Manager v tomto příkladu, provede první aplikace Azure AD ověřování.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). Pokud máte více než jednoho klienta Azure Active Directory, zkontrolujte, zda jste přihlášeni k adresáři správné kontrolou adresáře v rámci své uživatelské jméno. 

   ![Potvrďte adresáře](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Změňte adresář, vyberte své uživatelské jméno a potom vyberte adresář, který chcete.

2. V hlavní nabídce Azure, zvolte **Azure Active Directory** , můžete zobrazit aktuální adresář.

   ![Zvolte "Azure Active Directory"](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Pokud se nezobrazí v hlavní nabídce Azure **Azure Active Directory**, zvolte **další služby**. V **filtru** pole, zadejte "Azure Active Directory" jako filtr a potom zvolte **Azure Active Directory**.

3. V adresáři nabídce v části **spravovat**, zvolte **registrace aplikace**. V seznamu registrovaných aplikací vyberte **+ nové registrace aplikace**.

   ![Zvolte "Registrace aplikace", "+ nové registrace aplikace.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. V části **vytvořit**, zadejte podrobnosti pro vaši aplikaci Azure AD, jak je popsáno v tabulce a pak zvolte **vytvořit**. 

   ![Vytvořit aplikaci Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *webové rozhraní api aplikace názvu* | Název aplikace Azure AD Web API | 
   | **Typ aplikace** | **Webovou aplikaci nebo rozhraní API** | Typ vaší aplikace | 
   | **Adresa URL přihlašování** | `https://login.windows.net` | | 
   |||| 

5. Když se vrátíte do vašeho adresáře **registrace aplikace** seznamu, vyberte svou aplikaci Azure AD.

   ![Vyberte svou aplikaci Azure AD ze seznamu](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Jakmile se zobrazí stránku podrobností aplikace, ujistěte se, že jste **zkopírujte a uložte aplikace *ID aplikace* bezpečné místo**. Musíte toto ID pro pozdější použití.

   !["ID aplikace" uložit pro pozdější použití](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Nyní poskytují adresa URL odpovědi pro vaši aplikaci Azure AD. V dané aplikaci **nastavení** nabídce zvolte **adresy URL odpovědí**. Zadat tuto adresu URL, a potom vyberte **Uložit**.

   ![Adresy URL odpovědí](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Adresy URL odpovědí** | Pro vaše vlastní rozhraní API zadejte tuto adresu URL: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Přidělená oprávnění** | {není vyžadováno} | | 
   | **Klíč klienta** | {není vyžadováno} | | 
   |||| 

   > [!TIP]
   > Pokud **nastavení** nabídky nebyla dříve nezobrazí, zvolte **nastavení** tady:
   >
   > ![Zvolte "Nastavení"](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Vytvořte druhý aplikace Azure AD pro vaše vlastní konektor

Druhý aplikaci Azure AD zabezpečuje registrace vlastní konektor a přidá Delegovaný přístup k rozhraní Web API chráněn první aplikace Azure AD. 

> [!IMPORTANT]
> Ujistěte se, že vaše aplikace Azure AD existují ve stejném adresáři.

1. Vraťte se do **registrace aplikace** seznam a vyberte **+ nové registrace aplikace** znovu.

   ![Zvolte "Registrace aplikace", "+ nové registrace aplikace.](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. V části **vytvořit**, zadejte podrobnosti pro druhou aplikaci Azure AD, jak je popsáno v tabulce a pak zvolte **vytvořit**. 

   ![Vytvořit aplikaci Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *váš název konektoru* | Název aplikace Azure AD vaší konektoru | 
   | **Typ aplikace** | **Webovou aplikaci nebo rozhraní API** | Typ vaší aplikace | 
   | **Adresa URL přihlašování** | `https://login.windows.net` | | 
   |||| 

3. Když se vrátíte do vašeho adresáře **registrace aplikace** vyberte druhý aplikace Azure AD.

   ![Ze seznamu vyberte druhou aplikaci Azure AD](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Jakmile se zobrazí stránku podrobností aplikace, ujistěte se, že jste také **zkopírujte a uložte tuto aplikaci *ID aplikace* bezpečné místo příliš**. Musíte toto ID pro pozdější použití.

   !["ID aplikace" uložit pro pozdější použití](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Nyní poskytují adresa URL odpovědi pro vaši aplikaci Azure AD. V dané aplikaci **nastavení** nabídce zvolte **adresy URL odpovědí**. Zadat tuto adresu URL, a potom vyberte **Uložit**.

   | Nastavení | Navrhovaná hodnota | 
   | ------- | --------------- | 
   | **Adresy URL odpovědí** | Pro vlastní konektor Azure Resource Manager zadejte tuto adresu URL:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Pokud **nastavení** nabídky nebyla dříve nezobrazí, zvolte **nastavení** tady:
   >
   > ![Zvolte "Nastavení"](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Zpět v **nastavení** nabídce zvolte **požadovaná oprávnění** > **přidat**.

   ![Požadovaná oprávnění > Přidat](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Když **přidat rozhraní API pro přístup** otevře se nabídka, zvolte **vybrat rozhraní API** > 
**Windows Azure Service Management API** > **vyberte **.

   ![Vyberte rozhraní API.](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Na **přidat rozhraní API pro přístup** nabídky, zvolte **vyberte oprávnění**. V části **delegovaná oprávnění**, zvolte **přístupu Azure Service Management jako uživatele organizaci** > **vyberte**.

   ![Zvolte "Delegovaní oprávnění" > "přístup k Azure Services Management jako uživatele organizaci"](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Jinak pro další možnosti:

   | Možnost | Navrhovaná hodnota | Popis | 
   | ------ | --------------- | ----------- | 
   | **Přidělená oprávnění** | | Vyberte oprávnění k Delegovaný přístup k vaší webové rozhraní API | 
   |||| 

9. Nyní na **přístup přidat rozhraní API** nabídce zvolte **provádí**.

   ![Nabídka "Přidat přístup pomocí rozhraní API" > "Hotovo"](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Nyní generovat *klíč klienta*, nebo "tajný", pro aplikaci Azure AD vaší konektor. 

    1. Zpět na **nastavení** nabídce zvolte **klíče**. 
    Zadejte název pro klíč s 16 nebo méně znaků, vyberte dobu platnosti a pak zvolte **Uložit**.

       ![Vytvoření klíče klienta](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Jakmile se zobrazí generovaný klíč, **Ujistěte se, zkopírujte a uložte tento klíč bezpečné místo** před odchodem **klíče** stránky.
    
       ![Ručně zkopírujte a uložte klíč](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Po uložení klíče, můžete bezpečně zavřít **nastavení** nabídky.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Přidat ověřování Azure AD do aplikace webového rozhraní API

Nyní zapněte ověřování pro webové rozhraní API pomocí vaší první aplikace Azure AD. Další informace najdete další [postup konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. V [portál Azure](https://portal.azure.com), najít aplikace webového rozhraní API, kterou jste dříve publikovali v [vytvořit vlastní konektory z rozhraní Web API](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. V části **nastavení**, zvolte **ověřování / autorizace**. 

   1. V části **ověřování služby aplikace**, zvolte **na**.
   2. Pro **akci provést, když požadavek nebude ověřený**, vyberte **přihlásit se přes Azure Active Directory**.
   3. Pro **zprostředkovatele ověřování**, vyberte **Azure Active Directory**. 

   ![Zvolte "ověřování / autorizace"](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Na **nastavení Azure Active Directory** stránky:

   1. V části **režim správy**, zvolte **Express**.

   2. Nyní vyberte aplikaci Azure AD, která vaše aplikace webového rozhraní API používá pro ověřování. 
   Zvolte **vybrat existující aplikaci AD** > **aplikaci Azure AD**.

   3. V části **aplikací Azure AD**, vyberte aplikaci Azure AD, kterou jste vytvořili dříve pro aplikaci webového rozhraní API. 
   
   4. Zvolte **OK** až se vrátíte do **ověřování / autorizace** stránky.

   Například:

   ![Vyberte aplikaci Azure AD, který představuje aplikace webového rozhraní API](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Na **ověřování / autorizace** vyberte **Uložit**.

   ![Uložit nastavení ověřování](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Teď můžete použít aplikace webového rozhraní API služby Azure AD pro ověřování.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Nastavení ověřování Azure AD v souboru OpenAPI Web API

Otevřete soubor OpenAPI pro vaši aplikaci webového rozhraní API, abyste mohli přidat `securityDefintions` objekt a ověřování Azure AD v části `host` vlastnost. Tady je příklad, který ukazuje `host` vlastnost a `securityDefinitions` objektu:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
Nyní můžete vytvořit a registrovat vlastní konektor.

## <a name="next-steps"></a>Další kroky

* [Registrace vašeho konektoru](../logic-apps/logic-apps-custom-connector-register.md)
* [Vlastní konektor – nejčastější dotazy](../logic-apps/custom-connector-faq.md)
