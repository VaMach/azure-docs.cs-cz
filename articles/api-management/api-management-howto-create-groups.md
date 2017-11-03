---
title: "Spravovat účty pro vývojáře používání skupin v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak chcete spravovat účty pro vývojáře používání skupin v Azure API Management"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 00322340d67fd064a0dc9149790e031b94390709
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Postup vytvoření a používání skupin Správa účtů pro vývojáře ve službě Azure API Management
Ve službě API Management se ke správě viditelnosti produktů pro vývojáře používají skupiny. Produkty se první dostupná pro skupiny, a pak mohou vývojáři v těchto skupinách zobrazovat a odebírat produkty, které jsou přidružené skupiny. 

Služba API Management má následující neměnné systémové skupiny.

* **Správci** – členy této skupiny jsou správci předplatného Azure. Správci spravují instance služby API Management, vytváření rozhraní API, operace a produkty, které používají vývojáři.
* **Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře. Vývojáři jsou zákazníci, kteří vytvářejí aplikace pomocí vašich rozhraní API. Vývojáři mají přístup k portálu pro vývojáře a vytvářejí aplikace, které volají operace rozhraní API.
* **Hosté** – do této skupiny patří neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře v instanci služby API Management. Můžete jim udělit omezený přístup jenom ke čtení, například k zobrazení rozhraní API bez možnosti jeho volání.

Kromě těchto systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory][leverage external groups in associated Azure Active Directory tenants]. Vlastní a externí skupiny můžete používat společně se systémovými skupinami, a nastavovat tak vývojářům viditelnost produktů s rozhraním API a přístup k nim. Můžete například vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerskou organizací a povolit jim přístup k rozhraním API z produktu, který obsahuje jenom příslušná rozhraní API. Uživatel může být členem několika skupin.

Tato příručka ukazuje, jak přidat nové skupiny a přiřadit je k produktů a vývojáři správci instance API Management.

> [!NOTE]
> Kromě vytváření a Správa skupin na portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.
> 
> 

## <a name="create-group"></a>Vytvořit skupinu
Chcete-li vytvořit novou skupinu, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure. Tím přejdete na portál vydavatele služby API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

Klikněte na tlačítko **skupiny** z **API Management** nabídky na levé straně a pak klikněte na **přidat skupinu**.

![Přidat nové skupiny][api-management-add-group]

Zadejte jedinečný název pro skupinu a volitelný popis a klikněte na tlačítko **Uložit**.

![Přidat nové skupiny][api-management-add-group-window]

Do nové skupiny se zobrazí na kartě skupiny. Chcete-li upravit **název** nebo **popis** skupiny, klikněte na název skupiny v seznamu. Chcete-li odstranit skupinu, klikněte na tlačítko **odstranit**.

![Přidat skupinu][api-management-new-group]

Teď, když je skupina vytvořená, může být přidružen produktů a vývojáři.

## <a name="associate-group-product"></a>Spojit skupinu s produktem
Chcete-li přidružit skupinu s produktem, klikněte na tlačítko **produkty** z **API Management** nabídky na levé straně a potom klikněte na název požadovaného produktu.

![Nastavit viditelnost][api-management-add-group-to-product]

Vyberte **viditelnost** k přidání a odebrání skupiny, a chcete zobrazit aktuální skupiny pro produkt. Pokud chcete přidat nebo odebrat skupiny, zaškrtněte nebo zrušte zaškrtnutí políček pro požadované skupiny a klikněte na **Uložit**.

![Nastavit viditelnost][api-management-add-group-to-product-visibility]

> [!NOTE]
> Chcete-li přidat skupiny Azure Active Directory, přečtěte si téma [autorizace vývojářským účtům pomocí služby Azure Active Directory v Azure API Management](api-management-howto-aad.md).
> 
> Ke konfiguraci skupin z **viditelnost** pro produkt, klikněte na **spravovat skupiny**.
> 
> 

Jakmile produkt je přidružena ke skupině, vývojáři v této skupině můžou zobrazovat a odebírat produktu.

## <a name="associate-group-developer"></a>Přidružení skupin k vývojářům
Přidružení skupin k vývojářům, klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak zaškrtněte políčko vedle položky vývojáře, které chcete přiřadit ke skupině.

![Přidat vývojáře do skupiny][api-management-add-group-to-developer]

Jakmile jsou zaškrtnutá políčka požadované vývojáři, klikněte na požadovanou skupinu v **přidat do skupiny** rozevíracího seznamu. Vývojářům můžete odebrat ze skupiny pomocí **odebrat ze skupiny** rozevíracího seznamu. 

![Vývojáři][api-management-add-group-to-developer-saved]

Po přidání přidružení mezi na vývojáře a skupiny můžete zobrazit v **uživatelé** kartě.

## <a name="next-steps"></a>Další kroky
* Jakmile vývojář se přidá do skupiny, se můžou zobrazovat a odebírat produkty, které jsou přidružené k této skupině. Další informace najdete v tématu [vytvoření a publikování produktu v Azure API Management][How create and publish a product in Azure API Management],
* Kromě vytváření a Správa skupin na portálu vydavatele, můžete vytvořit a spravovat vaše skupiny pomocí rozhraní API REST API správy [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
