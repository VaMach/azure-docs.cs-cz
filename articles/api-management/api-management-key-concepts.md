---
title: Klíčové koncepty služby API Management
description: Seznamte se s rozhraními API, produkty, rolemi, skupinami a dalšími klíčovými koncepty služby API Management.
services: api-management
documentationcenter: ''
author: steved0x
manager: erikre
editor: ''

ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/09/2016
ms.author: sdanie

---
# Co je služba API Management?
Služba API Management pomáhá organizacím při publikování rozhraní API pro externí, partnerské a interní vývojáře, aby tak uvolnila potenciál jejich dat a služeb. Firmy po celém světě hledají způsoby, jak rozšířit svojí činnost na digitální platformě, vytvářejí nové kanály, hledají nové zákazníky a více se propojují s těmi stávajícími. Služba API Management nabízí základní možnosti pro zajištění úspěšného programu s rozhraním API prostřednictvím zapojení vývojářů, informací o podniku, analýz, zabezpečení a ochrany.

V následujícím videu se podívejte na přehled služby Azure API Management a naučte se API Management používat k přidávání celé řady funkcí do rozhraní API, včetně řízení přístupu, omezování četnosti, sledování, protokolování událostí a ukládání odezev do mezipaměti – s minimálním úsilím na vaší straně.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-API-Management-Overview/player]
> 
> 

Aby bylo možné službu API Management používat, vytvářejí správci rozhraní API. Každé rozhraní API se skládá z jedné nebo několika operací a každé rozhraní API můžete přidat do jednoho nebo více produktů. Aby mohli používat rozhraní API, přihlašují se vývojáři k odběru produktu, který obsahuje toto rozhraní API, a potom můžou volat jeho operace (v souladu s platnými zásadami využití).

Toto téma obsahuje přehled klíčových konceptů služby API Management.

> [!NOTE]
> Další informace najdete v bílé knize [Cloudová služba API Management: využití síly rozhraní API](http://j.mp/ms-apim-whitepaper) (formát PDF). Tato bílá kniha s úvodními informacemi o službě API Management vytvořená společností CITO Research obsahuje: 
> 
> * Běžné požadavky a problémy rozhraní API
> * Oddělení rozhraní API a představení průčelí
> * Rychlý úvod pro vývojáře
> * Zabezpečení přístupu
> * Analýzy a metriky
> * Získání kontroly a přehledu pomocí platformy API Management
> * Použití cloudového nebo místního řešení
> * Azure API Management
> 
> 

## <a name="apis"> </a>Rozhraní API a operace
Rozhraní API jsou základem instance služby API Management. Každé rozhraní API představuje sadu operací, které jsou vývojářům dostupné. Každé rozhraní API obsahuje odkaz na back-endovou službu, která implementuje rozhraní API, a jeho operace se mapují na operace implementované back-endovou službou. Operace ve službě API Management jsou vysoce konfigurovatelné a umožňují kontrolu nad mapováním adres URL, parametry dotazů a cest, obsahem požadavků a odezev a ukládáním operací do mezipaměti. Na úrovni rozhraní API nebo jednotlivé operace můžete implementovat také zásady kvót, omezení četnosti nebo omezení IP.

Další informace najdete v článcích [Vytvoření rozhraní API][Vytvoření rozhraní API] a [Přidání operací do rozhraní API][Přidání operací do rozhraní API].

## <a name="products"> </a> Produkty
Rozhraní API se k vývojářům dostávají prostřednictvím produktů. Produkty v API Management mají jedno nebo několik rozhraní API a mají nakonfigurovaný název, popis a podmínky použití. Produkty můžou být **otevřené** nebo **chráněné**. V případě chráněných produktů se musíte nejdřív přihlásit k jejich odběru a až potom je můžete používat. Otevřené produkty můžete používat bez předplatného. Jakmile je produkt připravený k použití pro vývojáře, můžete ho publikovat. Publikovaný produkt si můžou vývojáři zobrazovat (v případě chráněných produktů je podmínkou předplatné). Schválení předplatného se konfiguruje na úrovni produktu. Buď se může vyžadovat schválení správce, nebo se může schvalovat automaticky.

Ke správě viditelnosti produktů pro vývojáře se používají skupiny. Produkty udělují viditelnost skupinám a vývojáři můžou zobrazovat a odebírat produkty, které jsou viditelné pro skupinu, do které patří. 

Další informace najdete v článku [Vytvoření a publikování produktu][Vytvoření a publikování produktu] a v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"> </a> Skupiny
Ke správě viditelnosti produktů pro vývojáře se používají skupiny. Služba API Management má následující neměnné systémové skupiny.

* **Správci** – členy této skupiny jsou správci předplatného Azure. Správci spravují instance služby API Management, vytváření rozhraní API, operace a produkty, které používají vývojáři.
* **Vývojáři** – do této skupiny patří ověření uživatelé portálu pro vývojáře. Vývojáři jsou zákazníci, kteří vytvářejí aplikace pomocí vašich rozhraní API. Vývojáři mají přístup k portálu pro vývojáře a vytvářejí aplikace, které volají operace rozhraní API.
* **Hosté** – do této skupiny patří neověření uživatelé portálu pro vývojáře, například potenciální zákazníci, kteří navštěvují portál pro vývojáře v instanci služby API Management. Můžete jim udělit omezený přístup jenom ke čtení, například k zobrazení rozhraní API bez možnosti jeho volání.

Kromě těchto systémových skupin můžou správci vytvářet vlastní skupiny nebo [využívat externí skupiny v přidružených klientech služby Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Vlastní a externí skupiny můžete používat společně se systémovými skupinami, a nastavovat tak vývojářům viditelnost produktů s rozhraním API a přístup k nim. Můžete například vytvořit jednu vlastní skupinu pro vývojáře spojené s konkrétní partnerskou organizací a povolit jim přístup k rozhraním API z produktu, který obsahuje jenom příslušná rozhraní API. Uživatel může být členem několika skupin.

Další informace najdete v článku [Vytvoření a používání skupin][Vytvoření a používání skupin].

## <a name="developers"> </a> Vývojáři
Vývojáři představují uživatelské účty v instanci služby API Management. Vývojáře můžou vytvořit nebo pozvat správci, nebo se můžou sami zaregistrovat na [portálu pro vývojáře][portálu pro vývojáře]. Každý vývojář je členem jedné nebo několika skupin a může se přihlásit k odběru produktů, které jsou pro tyto skupiny viditelné.

Když se vývojáři přihlásí k odběru produktu, získají primární a sekundární klíč produktu. Tento klíč se používá při volání do rozhraní API produktu.

Další informace najdete v článcích o [vytváření a zvaní vývojářů][vytváření a zvaní vývojářů] a [přidružení skupin k vývojářům][přidružení skupin k vývojářům].

## <a name="policies"> </a> Zásady
Zásady jsou vynikající funkcí služby API Management, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na základě požadavku nebo odezvy z rozhraní API. Mezi oblíbené příkazy patří převod formátu XML do formátu JSON a omezení četnosti volání, kterým omezíte množství příchozích volání od vývojáře. K dispozici je i mnoho dalších zásad.

Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak). Některé zásady, například [řízení toku](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) a [nastavená proměnná](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable), jsou založené na výrazech zásad. Další informace najdete v článcích [Pokročilé zásady](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [Výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx) a v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

Úplný seznam zásad služby API Management najdete v [referenční příručce o zásadách][referenční příručce o zásadách]. Další informace o používání a konfiguraci zásad najdete v článku [Zásady služby API Management][Zásady služby API Management]. Kurz týkající se vytváření produktu se zásadami kvót a omezování četnosti najdete v článku [Vytvoření a konfigurace pokročilých nastavení produktu][Vytvoření a konfigurace pokročilých nastavení produktu]. Ukázku najdete v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"> </a> portálu pro vývojáře
Portál pro vývojáře je místo, na kterém se můžou vývojáři dozvědět o vašich rozhraních API, zobrazit a volat operace a přihlásit se k odběru produktů. Potenciální zákazníci můžou navštívit portál pro vývojáře, zobrazovat rozhraní API a operace a zaregistrovat se. Adresa URL portálu pro vývojáře je umístěná na řídicím panelu na portálu Azure Classic vaší instance služby API Management.

Vzhled portálu pro vývojáře můžete přizpůsobit přidáním vlastního obsahu, přizpůsobením stylů a přidáním brandingu.

## Služba API Management a ekonomika rozhraní API
Pokud se chcete o službě API Management dozvědět víc, podívejte se na následující prezentaci z konference Microsoft Ignite 2015.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[Rozhraní API a operace]: #apis
[Produkty]: #products
[Skupiny]: #groups
[Vývojáři]: #developers
[Zásady]: #policies
[portálu pro vývojáře]: #developer-portal

[Vytvoření rozhraní API]: api-management-howto-create-apis.md
[Přidání operací do rozhraní API]: api-management-howto-add-operations.md
[Vytvoření a publikování produktu]: api-management-howto-add-products.md
[Vytvoření a používání skupin]: api-management-howto-create-groups.md
[přidružení skupin k vývojářům]: api-management-howto-create-groups.md#associate-group-developer
[Vytvoření a konfigurace pokročilých nastavení produktu]: api-management-howto-product-with-rules.md
[vytváření a zvaní vývojářů]: api-management-howto-create-or-invite-developers.md
[referenční příručce o zásadách]: api-management-policy-reference.md
[Zásady služby API Management]: api-management-howto-policies.md
[Vytvoření instance služby API Management]: api-management-get-started.md#create-service-instance







<!--HONumber=Sep16_HO3-->


