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
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 3faa6c1867808436a66a2b33ea1a9d79ede2c8fb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
> [!WARNING]
> Integrace se službou Azure Active Directory je k dispozici v [Developer a Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) pouze úrovně.

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory v Azure API Management
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak povolit přístup k portálu pro vývojáře pro uživatele ze služby Azure Active Directory. Tento průvodce také ukazuje, jak spravovat skupiny uživatelů Azure Active Directory tak, že přidáte externí skupiny, které obsahují uživatele, služby Azure Active Directory.

> Pokud chcete provést kroky v této příručce musí mít nejprve Azure Active Directory, do kterého chcete vytvořit aplikaci.
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory
Chcete-li začít, klikněte na tlačítko **portál vydavatele** na portálu Azure pro služby API Management. Tím přejdete na portál vydavatele služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

Klikněte na tlačítko **zabezpečení** z **API Management** nabídky na levé straně a klikněte na **externí identity**.

![Externí identity][api-management-security-external-identities]

Klikněte na tlačítko **Azure Active Directory**. Poznamenejte si **adresy URL pro přesměrování** a přejít do služby Azure Active Directory na portálu Azure Classic.

![Externí identity][api-management-security-aad-new]

Klikněte **přidat** tlačítko Vytvořit novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.

![Přidejte novou aplikaci Azure Active Directory][api-management-new-aad-application-menu]

Zadejte název aplikace, vyberte možnost **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko Další.

![Novou aplikaci Azure Active Directory][api-management-new-aad-application-1]

Pro **přihlašovací adresa URL**, zadejte adresu URL přihlašování portálu pro vývojáře. V tomto příkladu **přihlašovací adresa URL** je `https://aad03.portal.current.int-azure-api.net/signin`. 

Pro **URL ID aplikace**, zadejte buď výchozí nebo vlastní doménu pro Azure Active Directory a k němu připojí do jedinečného řetězce. V tomto příkladu výchozí doménu **https://contoso5api.onmicrosoft.com** se používá s příponou **/api** zadaný.

![Nové vlastnosti aplikace Azure Active Directory][api-management-new-aad-application-2]

Klikněte na tlačítko zaškrtnutí a uložte a vytvořit aplikaci, přepněte na **konfigurace** kartu a nakonfigurujte novou aplikaci.

![Vytvořit novou aplikaci Azure Active Directory][api-management-new-aad-app-created]

Pokud se má použít pro tuto aplikaci více Azure Active Directory, klikněte na tlačítko **Ano** pro **aplikace je víceklientské**. Výchozí hodnota je **ne**.

![Aplikace je více klientů][api-management-aad-app-multi-tenant]

Kopírování **adresy URL pro přesměrování** z **Azure Active Directory** části **externí identity** kartě na portálu vydavatele a vložte ji do **odpovědi Adresa URL** textové pole. 

![Adresa URL odpovědi][api-management-aad-reply-url]

Přejděte do dolní části kartu Konfigurace, vyberte **oprávnění aplikací** rozevíracího seznamu a zkontrolujte **čtení dat adresáře**.

![Oprávnění aplikací][api-management-aad-app-permissions]

Vyberte **delegování oprávnění** rozevíracího seznamu a zkontrolujte **povolit přihlášení a čtení uživatelských profilů**.

![Přidělená oprávnění][api-management-aad-delegated-permissions]

> Další informace o aplikaci a přidělená oprávnění najdete v tématu [přístup k rozhraní Graph API][Accessing the Graph API].
> 
> 

Kopírování **Id klienta** do schránky.

![Id klienta][api-management-aad-app-client-id]

Přepněte zpět na portál vydavatele a vložte **Id klienta** zkopírovaných z konfigurace aplikace Azure Active Directory.

![Id klienta][api-management-client-id]

Přepněte zpět na konfiguraci služby Azure Active Directory a klikněte na tlačítko **vyberte dobu trvání** rozevírací seznam v **klíče** části a zadat interval. V tomto příkladu **1 rok** se používá.

![Klíč][api-management-aad-key-before-save]

Klikněte na tlačítko **Uložit** zobrazí klíč a uložte konfiguraci. Klíč zkopírujte do schránky.

> Tento klíč si poznamenejte. Po zavření okna konfigurace Azure Active Directory, klíč nelze zobrazit znovu.
> 
> 

![Klíč][api-management-aad-key-after-save]

Přepněte zpět na portál vydavatele a vložte klíč do **tajný klíč klienta** textové pole.

![Tajný klíč klienta][api-management-client-secret]

**Povolené klienty** Určuje adresáře, které mají přístup k rozhraním API v instanci služby API Management. Zadejte domény instancí Azure Active Directory, ke kterým chcete udělit přístup. Více domén můžete oddělit vložení znaků newline, mezery nebo čárkami.

![Povolené klientů][api-management-client-allowed-tenants]


Jakmile je zadána požadované konfigurace, klikněte na možnost **Uložit**.

![Uložit][api-management-client-allowed-tenants-save]

Po změny jsou uloženy, uživatelé v zadané Azure Active Directory můžete přihlásit na portál pro vývojáře podle kroků v [Přihlaste se k portálu pro vývojáře pomocí účtu Azure Active Directory] [ Log in to the Developer portal using an Azure Active Directory account].

V lze zadat více domén **klientům povoleno** části. Než každý uživatel může přihlásit z jiné doméně než původní domény, kde byla aplikace registrovaná, globální správce jinou doménu, musí udělit oprávnění pro aplikaci pro přístup k datům adresáře. Udělit oprávnění, má globální správce, přejděte k `https://<URL of your developer portal>/aadadminconsent` (například https://contoso.portal.azure-api.net/aadadminconsent), zadejte název domény klienta služby Active Directory chtějí poskytnout přístup a klikněte na tlačítko Odeslat. V následujícím příkladu, globální správce `miaoaad.onmicrosoft.com` pokouší udělit oprávnění k tomuto portálu konkrétní developer. 

![Oprávnění][api-management-aad-consent]

Na další obrazovce se výzva k potvrzení, udělíte oprávnění globálního správce. 

![Oprávnění][api-management-permissions-form]

> Pokud není globální správce pokusí o přihlášení, než oprávnění globální správce, pokus o přihlášení selže a se zobrazí chybové obrazovce.
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Postup přidání externí Azure skupině služby Active Directory
Když povolíte přístup pro uživatele v Azure Active Directory, můžete přidat skupiny Azure Active Directory do rozhraní API správy snadnější správu přidružení požadované produkty vývojářů ve skupině.

> Ke konfiguraci externích skupin Azure Active Directory, Azure Active Directory musí nejdřív nakonfigurovat na kartě identit pomocí postupu v předchozím oddílu. 
> 
> 

Externí skupiny Azure Active Directory se přidají **viditelnost** karta produktu, pro které chcete udělit přístup ke skupině. Klikněte na tlačítko **produkty**a potom klikněte na název požadovaného produktu.

![Konfigurace produktu][api-management-configure-product]

Přepnout **viditelnost** a klikněte na **přidat skupiny z Azure Active Directory**.

![Přidání skupin][api-management-add-groups]

Vyberte **klienta Azure Active Directory** z rozevíracího seznamu a pak zadejte název požadované skupiny v **skupiny** přidat textové pole.

![Vyberte skupinu][api-management-select-group]

Tento název skupiny lze nalézt v **skupiny** seznam pro Azure Active Directory, jak je znázorněno v následujícím příkladu.

![Seznam skupin Azure Active Directory][api-management-aad-groups-list]

Klikněte na tlačítko **přidat** ověřit název skupiny a přidejte skupinu. V tomto příkladu **Contoso 5 vývojáři** je přidána externí skupina. 

![Přidat skupinu][api-management-aad-group-added]

Klikněte na tlačítko **Uložit** uložte novou skupinu výběr.

Jakmile skupinu služby Azure Active Directory byla nakonfigurována jedním z produktů, je k dispozici kontrolu **viditelnost** kartě pro ostatní produkty v instanci služby API Management.

Zkontrolujte a konfigurujte vlastnosti externí skupiny po byly přidány, klikněte na název skupiny z **skupiny** kartě.

![Správa skupin][api-management-groups]

Zde můžete upravit **název** a **popis** skupiny.

![Úprava skupiny][api-management-edit-group]

Uživatelé z nakonfigurovaných Azure Active Directory můžete přihlásit k portálu pro vývojáře a zobrazení a přihlášení k odběru do žádné skupiny, ke kterým mají viditelnost podle pokynů uvedených v následující části.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Jak se přihlásit na portál pro vývojáře pomocí účtu Azure Active Directory
Přihlaste se pomocí účtu Azure Active Directory nakonfigurované v předchozích částech portálu pro vývojáře, otevřete nové okno prohlížeče pomocí **přihlašovací adresa URL** z konfigurace aplikace služby Active Directory a klikněte na tlačítko **Azure Active Directory**.

![Portál pro vývojáře][api-management-dev-portal-signin]

Zadejte přihlašovací údaje pro jeden z uživatelů ve vašem Azure Active Directory a klikněte na tlačítko **přihlášení**.

![Přihlášení][api-management-aad-signin]

S registračním formuláři můžete být vyzváni, pokud je vyžadováno žádné další údaje. Dokončení registrace a klikněte na tlačítko **zaregistrovat**.

![Registrace][api-management-complete-registration]

Uživatel je nyní přihlášeni portál pro vývojáře pro instanci služby API Management.

![Registrace je dokončena.][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

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

