---
title: "Zabezpečení vlastních konektorů s Azure AD - Azure Logic Apps | Microsoft Docs"
description: "Přidání zabezpečení a ověřování do rozhraní API a konektoru pomocí Azure Active Directory (Azure AD)"
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
ms.openlocfilehash: dae76a4230a1cfbe8970bbb1015041fde7765f49
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2018
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Zvýšit zabezpečení rozhraní API a konektor s Azure Active Directory (Azure AD)

Pokud chcete zabezpečit volání mezi vaším rozhraním API a vlastním konektorem, přidejte do svého webového rozhraní API a konektoru ověřování Azure AD. Pro tento scénář vytvoříte dvě aplikace Azure Active Directory (Azure AD) – jedna aplikace Azure AD zabezpečí vaše webové rozhraní API, zatímco druhá aplikace Azure AD zabezpečí registraci vašeho konektoru a přidá delegovaný přístup. 

Tento kurz jako příklad používá rozhraní API Azure Resource Manageru. Azure Resource Manager pomáhá spravovat komponenty pro řešení vytvořené v Azure, jako jsou databáze, virtuální počítače a webové aplikace. Pokud chcete spravovat prostředky Azure ze svých pracovních postupů, může být užitečný vlastní konektor pro Azure Resource Manager. Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Pokud předplatné nemáte, můžete začít s [bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete zaregistrovat [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Pro účely tohoto kurzu [ukázkový soubor OpenAPI pro Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

  > [!NOTE] 
  > Ukázkový soubor OpenAPI nedefinuje všechny operace Azure Resource Manageru a aktuálně obsahuje pouze operaci pro [Výpis všech předplatných](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). Pomocí [online editoru OpenAPI](http://editor.swagger.io/) můžete tento soubor OpenAPI upravit nebo vytvořit jiný.
  >
  > Postupy v tomto kurzu můžete použít u jakéhokoli rozhraní RESTful API, kde chcete využít ověřování Azure AD.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Vytvoření první aplikace Azure AD pro zabezpečení webového rozhraní API

Vaše první aplikace Azure AD provádí ověřování, když váš vlastní konektor volá rozhraní API, což je v tomto příkladu rozhraní API Azure Resource Manageru.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). Pokud máte více než jednoho tenanta Azure Active Directory, zkontrolujte adresář pod vaším uživatelským jménem a potvrďte, že jste přihlášeni ke správnému adresáři. 

   ![Potvrzení adresáře](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Pokud chcete změnit adresář, zvolte své uživatelské jméno a pak vyberte požadovaný adresář.

2. V hlavní nabídce Azure zvolte **Azure Active Directory** a zobrazte váš aktuální adresář.

   ![Volba možnosti Azure Active Directory](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Pokud se nezobrazí v hlavní nabídce Azure **Azure Active Directory**, zvolte **všechny služby**. Do pole **Filtr** zadejte jako filtr „Azure Active Directory“ a pak zvolte **Azure Active Directory**.

3. V nabídce adresáře v části **Správa** zvolte **Registrace aplikací**. V seznamu registrovaných aplikací zvolte **+ Registrace nové aplikace**.

   ![Volba možností Registrace aplikací a + Registrace nové aplikace](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. V části **Vytvořit** zadejte podrobnosti o vaší aplikaci Azure AD, jak je popsáno v tabulce, a pak zvolte **Vytvořit**. 

   ![Vytvoření aplikace Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *název_aplikace_webového_rozhraní_API* | Název aplikace Azure AD vašeho webového rozhraní API | 
   | **Typ aplikace** | **Webová aplikace / webové rozhraní API** | Typ vaší aplikace | 
   | **Přihlašovací adresa URL** | `https://login.windows.net` | | 
   |||| 

5. Po návratu k seznamu **Registrace aplikací** vašeho adresáře vyberte vaši aplikaci Azure AD.

   ![Výběr vaší aplikace Azure AD ze seznamu](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Jakmile se zobrazí stránka s podrobnostmi o aplikaci, nezapomeňte **zkopírovat *ID aplikace* a uložit ho na bezpečném místě**. Toto ID budete potřebovat později.

   ![Uložení ID aplikace pro pozdější použití](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Teď pro svou aplikaci Azure AD zadejte adresu URL odpovědi. V nabídce **Nastavení** aplikace zvolte **Adresy URL odpovědí**. Zadejte tuto adresu URL a zvolte **Uložit**.

   ![Adresy URL odpovědí](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Adresy URL odpovědí** | Pro vaše vlastní rozhraní API zadejte tuto adresu URL: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Delegovaná oprávnění** | {není potřeba} | | 
   | **Klíč klienta** | {není potřeba} | | 
   |||| 

   > [!TIP]
   > Pokud se dříve nezobrazila nabídka **Nastavení**, zvolte **Nastavení** tady:
   >
   > ![Volba možnosti Nastavení](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Vytvoření druhé aplikace Azure AD pro vlastní konektor

Vaše druhá aplikace Azure AD zabezpečí registraci vašeho vlastního konektoru a přidá delegovaný přístup k webovému rozhraní API chráněnému první aplikací Azure AD. 

> [!IMPORTANT]
> Ujistěte se, že obě aplikace Azure AD existují ve stejném adresáři.

1. Vraťte se k seznamu **Registrace aplikací** a znovu zvolte **+ Registrace nové aplikace**.

   ![Volba možností Registrace aplikací a + Registrace nové aplikace](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. V části **Vytvořit** zadejte podrobnosti o vaší druhé aplikaci Azure AD, jak je popsáno v tabulce, a pak zvolte **Vytvořit**. 

   ![Vytvoření aplikace Azure AD](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *název_vašeho_konektoru* | Název aplikace Azure AD vašeho konektoru | 
   | **Typ aplikace** | **Webová aplikace / webové rozhraní API** | Typ vaší aplikace | 
   | **Přihlašovací adresa URL** | `https://login.windows.net` | | 
   |||| 

3. Po návratu k seznamu **Registrace aplikací** vašeho adresáře vyberte vaši druhou aplikaci Azure AD.

   ![Výběr vaší druhé aplikace Azure AD ze seznamu](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Jakmile se zobrazí stránka s podrobnostmi o aplikaci, opět nezapomeňte **zkopírovat *ID aplikace* a uložit ho na bezpečném místě**. Toto ID budete potřebovat později.

   ![Uložení ID aplikace pro pozdější použití](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Teď pro svou aplikaci Azure AD zadejte adresu URL odpovědi. V nabídce **Nastavení** aplikace zvolte **Adresy URL odpovědí**. Zadejte tuto adresu URL a zvolte **Uložit**.

   | Nastavení | Navrhovaná hodnota | 
   | ------- | --------------- | 
   | **Adresy URL odpovědí** | Pro vlastní konektor Azure Resource Manageru zadejte tuto adresu URL: `https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Pokud se dříve nezobrazila nabídka **Nastavení**, zvolte **Nastavení** tady:
   >
   > ![Volba možnosti Nastavení](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Zpět v nabídce **Nastavení** zvolte **Požadovaná oprávnění** > **Přidat**.

   ![Požadovaná oprávnění > Přidat](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Po otevření nabídky **Přidat přístup přes rozhraní API** zvolte **Vybrat rozhraní API** > 
**Rozhraní API pro správu služeb Windows Azure** > **Vybrat**.

   ![Výběr rozhraní API](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. V nabídce **Přidat přístup přes rozhraní API** zvolte **Vybrat oprávnění**. V části **Delegovaná oprávnění** zvolte **Přístup ke správě služeb Azure jako uživatelé organizace** > **Vybrat**.

   ![Volba možností Delegovaná oprávnění > Přístup ke správě služeb Azure jako uživatelé organizace](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Případně pro jiné možnosti:

   | Možnost | Navrhovaná hodnota | Popis | 
   | ------ | --------------- | ----------- | 
   | **Delegovaná oprávnění** | | Vyberte oprávnění pro delegovaný přístup k vašemu webovému rozhraní API. | 
   |||| 

9. Teď v nabídce **Přidat přístup přes rozhraní API** zvolte **Hotovo**.

   ![Nabídka Přidat přístup přes rozhraní API > Hotovo](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Teď pro aplikaci Azure AD vašeho konektoru vygenerujte *klíč klienta* neboli tajný klíč. 

    1. Zpět v nabídce **Nastavení** zvolte **Klíče**. 
    Zadejte název vašeho klíče o délce maximálně 16 znaků, vyberte dobu platnosti a pak zvolte **Uložit**.

       ![Vytvoření klíče klienta](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Jakmile se zobrazí vygenerovaný klíč, **nezapomeňte tento klíč zkopírovat a uložit na bezpečném místě** před tím, než stránku **Klíče** opustíte.
    
       ![Ruční zkopírování a uložení klíče](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Po uložení klíče můžete nabídku **Nastavení** bezpečně zavřít.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Přidání ověřování Azure AD do aplikace webového rozhraní API

Teď pro své webové rozhraní API zapnete ověřování pomocí vaší první aplikace Azure AD. Další informace najdete další [postup konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. V [portál Azure](https://portal.azure.com), najít aplikace webového rozhraní API, kterou jste dříve publikovali v [vytvořit vlastní konektory z rozhraní Web API](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. V části **Nastavení** zvolte **Ověřování / autorizace**. 

   1. V části **Ověřování pomocí služby App Service** zvolte **Zapnuto**.
   2. V části **Akce, která se má provést, když požadavek nebude ověřený** vyberte **Přihlásit se přes Azure Active Directory**.
   3. V části **Zprostředkovatelé ověřování** vyberte **Azure Active Directory**. 

   ![Volba možnosti Ověřování / autorizace](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Na stránce **Nastavení služby Azure Active Directory**:

   1. V části **Režim správy** zvolte **Expresní**.

   2. Teď vyberte aplikaci Azure AD, kterou vaše aplikace webového rozhraní API používá k ověřování. 
   Zvolte **Vybrat existující aplikaci AD** > **Aplikace Azure AD**.

   3. V části **Aplikace Azure AD** vyberte aplikaci Azure AD, kterou jste vytvořili dříve pro svou aplikaci webového rozhraní API. 
   
   4. Volte možnost **OK**, dokud se nevrátíte na stránku **Ověřování / autorizace**.

   Příklad:

   ![Volba aplikace Azure AD, která představuje vaši aplikaci webového rozhraní API](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Na stránce **Ověřování / autorizace** zvolte **Uložit**.

   ![Uložení nastavení ověřování](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Vaše aplikace webového rozhraní API teď může k ověřování využívat Azure AD.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Nastavení ověřování Azure AD v souboru OpenAPI webového rozhraní API

Otevřete soubor OpenAPI pro vaši aplikaci webového rozhraní API, abyste pod vlastnost `host` mohli přidat objekt `securityDefintions` a ověřování Azure AD. Tady je příklad ukazující vlastnost `host` a objekt `securityDefinitions`:

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
Teď jste připraveni vytvořit a zaregistrovat vlastní konektor.

## <a name="next-steps"></a>Další postup

* [Registrace vašeho konektoru](../logic-apps/logic-apps-custom-connector-register.md)
* [Nejčastější dotazy k vlastním konektorům](../logic-apps/custom-connector-faq.md)
