---
title: "Postup vytvoření a publikování produktu v Azure API Management"
description: "Naučte se vytvářet a publikovat produkty ve službě Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 2cf905967f26de97613ff7d5fc495c9223e11390
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Postup vytvoření a publikování produktu v Azure API Management
Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také kvóty využití a podmínky použití. Po publikování produktu vývojáři můžou přihlásit k produktu a začít používat rozhraní API produktu. Téma poskytuje pokyny pro vytváření produktu, přidání rozhraní API a publikování pro vývojáře.

## <a name="create-product"></a>Vytvoření produktu
Operace jsou přidány a nakonfigurovat tak, aby rozhraní API na portálu vydavatele. Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

Klikněte na **produkty** v nabídce na levé straně zobrazíte **produkty** a klikněte na tlačítko **přidat produkt**.

![Produkty][api-management-products]

![Nového produktu][api-management-add-new-product]

Zadejte popisný název produktu v **název** pole a popis produktu v **popis** pole.

Produkty v API Management můžou být **otevřete** nebo **chráněné**. V případě chráněných produktů se musíte nejdřív přihlásit k jejich odběru a až potom je můžete používat. Otevřené produkty můžete používat bez předplatného. Zkontrolujte **vyžadovat předplatné** vytvořit chráněný produkt, který vyžaduje předplatné. Toto je výchozí nastavení.

Zkontrolujte **vyžadovat schválení předplatného** Pokud chcete, aby správci zkontrolovat a následně přijímal nebo odmítal předplatné pokusy o tohoto produktu. Pokud pole není zaškrtnuto, bude předplatné pokusů o automatické schválení. Další informace o předplatných najdete v tématu [zobrazení předplatitelů produktu][View subscribers to a product].

K přihlášení k vícenásobným odběrům produktu vývojářským účtům povolit, zkontrolujte **povolit více předplatných** zaškrtávací políčko. Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit jen jednou produktu.

![Neomezená více předplatných][api-management-unlimited-multiple-subscriptions]

Chcete-li omezit počet více souběžných předplatných, zkontrolujte **omezit počet souběžných odběrů** zaškrtněte políčko a zadejte limitu předplatného. V následujícím příkladu je omezen na čtyři za vývojářský účet souběžných předplatných.

![Čtyři více předplatných][api-management-four-multiple-subscriptions]

Po nakonfigurování všech možností nového produktu, klikněte na tlačítko **Uložit** k vytvoření nového produktu.

![Produkty][api-management-products-page]

> Ve výchozím nastavení jsou nové produkty jsou publikování a jsou viditelné pouze pro **správci** skupiny.
> 
> 

Chcete-li konfigurovat produkt, klikněte na název produktu v **produkty** kartě.

## <a name="add-apis"></a>Přidat rozhraní API pro určitý produkt
**Produkty** stránka obsahuje čtyři odkazy pro konfiguraci: **Souhrn**, **nastavení**, **viditelnost**, a  **Odběratelé, kteří**. **Souhrn** karta je, kde můžete přidat rozhraní API a provést nebo zrušit produktu.

![Souhrn][api-management-new-product-summary]

Před publikováním produktu je nutné přidat jeden nebo více rozhraní API. Chcete-li to provést, klikněte na tlačítko **přidat rozhraní API do produktu**.

![Přidání rozhraní API][api-management-add-apis-to-product]

Vyberte požadované rozhraní API a klikněte na **Uložit**.

## <a name="add-description"></a>Přidat popisné informace pro určitý produkt
**Nastavení** karta umožňuje poskytují podrobné informace o produktu, například jeho účel, poskytuje přístup k rozhraní API a další užitečné informace. Obsah je cílena na vývojáře, které bude možné volání rozhraní API a může být napsán v jako prostý text ani značka jazyka HTML.

![Nastavení produktu][api-management-product-settings]

Zkontrolujte **vyžadovat předplatné** vytvořit chráněný produkt, který vyžaduje předplatné použít, nebo zrušte zaškrtnutí políčka vytvořit otevřete produktu, kterou lze volat bez předplatného.

Vyberte **vyžadovat schválení předplatného** Pokud chcete ručně schválit všechny požadavky odběru produktu. Ve výchozím nastavení jsou automaticky udělí všechny odběry produktu.

K přihlášení k vícenásobným odběrům produktu vývojářským účtům povolit, zkontrolujte **povolit více předplatných** zaškrtněte políčko a volitelně určete omezení. Pokud toto políčko není zaškrtnuté, každý vývojářský účet se mohou přihlásit jen jednou produktu.

Volitelně můžete vyplnit **podmínky použití** pole s popisem podmínky použití pro produkt, které odběratele musí přijmout, abyste mohli používat produkt.

## <a name="publish-product"></a>Publikování produktu
Předtím, než je možné volat rozhraní API v produktu, musí být publikován produktu. Na **Souhrn** pro produkt, klikněte na **publikovat**a potom klikněte na **Ano, publikovat** k potvrzení. Chcete-li změnit publikované dříve produktu soukromé, klikněte na tlačítko **zrušit publikování**.

![Publikování produktu][api-management-publish-product]

## <a name="make-visible"></a>Zviditelnit produktu pro vývojáře
**Viditelnost** karta umožňuje vybrat, které role se můžou zobrazit produktu na portál pro vývojáře a přihlásit se k produktu.

![Přehled produktu][api-management-product-visiblity]

K povolení nebo zakázání viditelnost produktu pro vývojáře ve skupině, zaškrtněte nebo zrušte zaškrtnutí políček vedle skupiny a pak klikněte na **Uložit**.

> Další informace najdete v tématu [postup vytvoření a používání skupin Správa účtů pro vývojáře ve službě Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Zobrazení předplatitelů produktu
**Odběratelé, kteří** karta Vypíše seznam vývojáři, kteří se přihlásili k odběru produktu. Podrobnosti a nastavení pro každý vývojář lze zobrazit kliknutím na název pro vývojáře. V tomto příkladu žádné vývojáři mají ještě přihlásit k odběru produktu.

![Vývojáři][api-management-developer-list]

## <a name="next-steps"></a>Další kroky
Po přidání požadované rozhraní API a publikování produktu vývojáři můžou přihlášení k odběru produktu a začít volat rozhraní API. Kurz, který ukazuje, tyto položky, jakož i konfigurace pokročilé produktu najdete v části [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management][How create and configure advanced product settings in Azure API Management].

Další informace o práci s produkty najdete v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
