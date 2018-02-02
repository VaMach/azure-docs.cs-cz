---
title: "Autorizovat vývojářským účtům pomocí Azure Active Directory – Azure API Management | Microsoft Docs"
description: "Naučte se autorizace uživatelů pomocí služby Azure Active Directory ve službě API Management."
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: c9d99d6f7c2777c8e68f5db50b402280377d88e9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory v Azure API Management

Tento průvodce vám ukáže, jak povolit přístup k portálu pro vývojáře pro uživatele ze služby Azure Active Directory. Tento průvodce také ukazuje, jak spravovat skupiny uživatelů Azure Active Directory tak, že přidáte externí skupiny, které obsahují uživatele, služby Azure Active Directory.

> [!WARNING]
> Integrace se službou Azure Active Directory je k dispozici v [Developer, Standard a Premium](https://azure.microsoft.com/pricing/details/api-management/) pouze úrovně.

## <a name="prerequisites"></a>Požadavky

- Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
- Import a publikujte instance služby API Management. Další informace najdete v tématu [importu a publikování](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). 
2. Vyberte ![šipka](./media/api-management-howto-aad/arrow.png).
3. Do vyhledávacího pole zadejte "api".
4. Klikněte na tlačítko **služby API Management**.
5. Vyberte instanci služby APIM.
6. V části **zabezpečení**, vyberte **identity**.

    ![Externí identity](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Klikněte na tlačítko **+ přidat** shora.

    **Zprostředkovatele identity přidat** okno se zobrazí na pravé straně.
8. V části **typ zprostředkovatele**, vyberte **Azure Active Directory**.

    Ovládací prvky, které vám umožní zadat další potřebné informace se zobrazí v okně. Ovládací prvky zahrnují: **ID klienta**, **tajný klíč klienta** (získáte informace později v tomto kurzu).
9. Poznamenejte si **adresy URL pro přesměrování**.  
10. V prohlížeči otevřete na jiné kartě. 
11. Otevřete [Azure Portal](https://portal.azure.com).
12. Vyberte ![šipka](./media/api-management-howto-aad/arrow.png).
13. Typ "aktivní" **Azure Active Directory** se zobrazí.
14. Vyberte **Azure Active Directory**.
15. V části **SPRAVOVAT**, vyberte **registrace aplikací**.

    ![Registrace aplikace](./media/api-management-howto-aad/api-management-with-aad002.png)
16. Klikněte na **Registrace nové aplikace**.

    **Vytvořit** okno se zobrazí na pravé straně. To je, kde zadáte informace relavent AAD aplikace.
17. Eneter název aplikace.
18. Typ aplikace, vyberte **webové aplikace nebo rozhraní API**.
19. Přihlašovací adresa URL zadejte adresu URL přihlašování portálu pro vývojáře. V tomto příkladu je adresa URL přihlašování: https://apimwithaad.portal.azure-api.net/signin.
20. Klikněte na tlačítko **vytvořit** k vytvoření aplikace.
21. Chcete-li najít aplikaci, vyberte **registrace aplikace** a hledat podle názvu.

    ![Registrace aplikace](./media/api-management-howto-aad/find-your-app.png)
22. Po registraci aplikace, přejděte na **adresa URL odpovědi** a zajistěte, aby "Přesměrování URL" je nastavena na hodnotu jste získali v kroku 9. 
23. Pokud chcete nakonfigurovat aplikace (například změnit **URL ID aplikace**) vyberte **vlastnosti**.

    ![Registrace aplikace](./media/api-management-howto-aad/api-management-with-aad004.png)

    Pokud se má použít pro tuto aplikaci více Azure Active Directory, klikněte na tlačítko **Ano** pro **aplikace je víceklientské**. Výchozí hodnota je **ne**.
24. Nastavte oprávnění aplikací tak, že vyberete **požadovaná oprávnění**.
25. Vyberte aplikace a zaškrtněte **čtení dat adresáře** a **přihlášení a čtení profilu uživatele**.

    ![Registrace aplikace](./media/api-management-howto-aad/api-management-with-aad005.png)

    Další informace o aplikaci a přidělená oprávnění najdete v tématu [přístup k rozhraní Graph API][Accessing the Graph API].
26. V levém okně zkopírovat **ID aplikace** hodnotu.

    ![Registrace aplikace](./media/api-management-howto-aad/application-id.png)
27. Přepnout zpět do aplikace API Management. **Zprostředkovatele identity přidat** měla by se zobrazit okno. <br/>Vložení **ID aplikace** hodnotu **Id klienta** pole.
28. Přepněte zpět na konfiguraci služby Azure Active Directory a klikněte na **klíče**.
29. Vytvořte nový klíč speicifying název a doba trvání. 
30. Klikněte na **Uložit**. Získá vygenerovat klíč.

    Klíč zkopírujte do schránky.

    ![Registrace aplikace](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Tento klíč si poznamenejte. Po zavření okna konfigurace Azure Active Directory, klíč nelze zobrazit znovu.
    > 
    > 
31. Přepnout zpět do aplikace API Management. <br/>V **zprostředkovatele identity přidat** okno, vložte klíč do **tajný klíč klienta** textové pole.
32. **Zprostředkovatele identity přidat** okno, obsahuje **klientům povoleno** textového pole, ve které zadejte adresáře, které mají přístup k rozhraní API v instanci služby API Management. <br/>Zadejte domény instancí Azure Active Directory, ke kterým chcete udělit přístup. Více domén můžete oddělit vložení znaků newline, mezery nebo čárkami.

    V lze zadat více domén **klientům povoleno** části. Než každý uživatel může přihlásit z jiné doméně než původní domény, kde byla aplikace registrovaná, globální správce jinou doménu, musí udělit oprávnění pro aplikaci pro přístup k datům adresáře. Udělit oprávnění, má globální správce, přejděte k `https://<URL of your developer portal>/aadadminconsent` (například https://contoso.portal.azure-api.net/aadadminconsent), zadejte název domény klienta služby Active Directory chtějí poskytnout přístup a klikněte na tlačítko Odeslat. V následujícím příkladu, globální správce `miaoaad.onmicrosoft.com` pokouší udělit oprávnění k tomuto portálu konkrétní developer. 

33. Jakmile je zadána požadované konfigurace, klikněte na možnost **přidat**.

    ![Registrace aplikace](./media/api-management-howto-aad/api-management-with-aad007.png)

Po změny jsou uloženy, uživatelé v zadané Azure Active Directory můžete přihlásit na portál pro vývojáře podle kroků v [Přihlaste se k portálu pro vývojáře pomocí účtu Azure Active Directory](#log_in_to_dev_portal).

![Oprávnění](./media/api-management-howto-aad/api-management-aad-consent.png)

Na další obrazovce se výzva k potvrzení, udělíte oprávnění globálního správce. 

![Oprávnění](./media/api-management-howto-aad/api-management-permissions-form.png)

Pokud není globální správce pokusí o přihlášení, než oprávnění globální správce, pokus o přihlášení selže a se zobrazí chybové obrazovce.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Postup přidání externí Azure skupině služby Active Directory

Když povolíte přístup pro uživatele v Azure Active Directory, můžete přidat skupiny Azure Active Directory do rozhraní API správy snadnější správu přidružení požadované produkty vývojářů ve skupině.

Ke konfiguraci externích skupin Azure Active Directory, Azure Active Directory musí nejdřív nakonfigurovat na kartě identit pomocí postupu v předchozím oddílu. 

Externí skupiny Azure Active Directory se přidají **skupiny** kartě instanci služby API Management.

![Skupiny](./media/api-management-howto-aad/api-management-with-aad008.png)

1. Vyberte kartu **Skupiny**.
2. Klikněte **skupiny přidat AAD** tlačítko.
3. Vyberte skupinu, kterou chcete přidat.
4. Stiskněte klávesu **vyberte** tlačítko.

Po vytvoření skupiny služby Azure Active Directory, můžete zkontrolovat a konfigurovat vlastnosti pro externí skupiny po byly přidány, klikněte na název skupiny z **skupiny** kartě.

Zde můžete upravit **název** a **popis** skupiny.
 
Uživatelé z nakonfigurovaných Azure Active Directory můžete přihlásit k portálu pro vývojáře a zobrazení a přihlášení k odběru do žádné skupiny, ke kterým mají viditelnost podle pokynů uvedených v následující části.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>Jak se přihlásit na portál pro vývojáře pomocí účtu Azure Active Directory

Přihlaste se pomocí účtu Azure Active Directory nakonfigurované v předchozích částech portálu pro vývojáře, otevřete nové okno prohlížeče pomocí **přihlašovací adresa URL** z konfigurace aplikace služby Active Directory a klikněte na tlačítko **Azure Active Directory**.

![Portál pro vývojáře][api-management-dev-portal-signin]

Zadejte přihlašovací údaje pro jeden z uživatelů ve vašem Azure Active Directory a klikněte na tlačítko **přihlášení**.

![Přihlášení][api-management-aad-signin]

S registračním formuláři můžete být vyzváni, pokud je vyžadováno žádné další údaje. Dokončení registrace a klikněte na tlačítko **zaregistrovat**.

![Registrace][api-management-complete-registration]

Uživatel je nyní přihlášeni portál pro vývojáře pro instanci služby API Management.

![Registrace je dokončena.][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
