---
title: "Přiřadit přístup v Azure náklady na správu | Microsoft Docs"
description: "Přiřadíte přístup náklady data správy s uživatelskými účty, které definují úrovně přístupu pro entity."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: a42f3b51bf6d888d0d5602887ed317c6164391ef
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="assign-access-to-cost-management-data"></a>Přiřadit přístup k datům náklady na správu

Přístup k datům náklady na správu pochází od uživatele nebo Správa entit. Přístup k určení Cloudyn uživatelské účty *entity* a funkce správy. Existují dva typy přístupu: správce a uživatele. Pokud nezměnil na uživatele, správce přístup umožňuje na portálu Cloudyn uživatele neomezený používání všech funkcí, včetně: Správa uživatelů, správy příjemce seznamy a entity kořenový přístup k datům všechny entity. Uživatelský přístup je určený pro koncové uživatele k zobrazení sestav a vytváření sestav pomocí přístup, že mají k datům entity.

Entity se používají tak, aby odrážela obchodní organizaci hierarchická struktura. Identifikují oddělení, rozdělení a týmy ve vaší organizaci v Cloudyn. Hierarchie entit umožňuje lépe sledovat výdaje podle entity.

Při registraci do Azure smlouvy nebo účtu, účet s oprávněním správce byla vytvořena v Cloudyn, tak abyste mohli provést všechny kroky v tomto kurzu. Tento kurz se zaměřuje na přístup k datům náklady na správu včetně správu uživatelů a Správa entit. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit uživatele s přístupem správce
> * Vytvořit uživatele s přístupu uživatele
> * Vytvořte entity



## <a name="create-a-user-with-admin-access"></a>Vytvořit uživatele s přístupem správce

I když už máte přístup správce, spolupracovníky ve vaší organizaci může také nutné mít přístup správce. V portálu Cloudyn, klikněte na symbol ozubené kolečko v horním pravém rohu a vyberte **Správa uživatelů**. Klikněte na tlačítko **přidat nové uživatele** přidat nového uživatele.

Zadejte požadované informace o uživateli. Pole hesla můžete ponechat prázdné, tak, aby na první přihlášení může uživatel nastavit nové heslo. Odkaz s přihlašovací informace se odešlou do uživatele podle e-mailu z Cloudyn při výběru **uživatele oznámení e-mailem**. Vyberte oprávnění k povolení správy uživatelů, tak, aby uživatel můžete vytvořit a upravit jiní uživatelé. Uvádí Správa příjemců chcete umožnit uživatelům upravit příjemce seznamy.

V části **má uživatel přístup správce**, kořenové entity vaší organizace je vybrána. Nechte vybrané kořenové a potom uložte informace o uživateli. Výběr entity kořenové umožňuje uživatelům mít oprávnění správce nejen kořenové entity ve stromové struktuře, ale také všechny entity, které jsou umístěné pod ním.  
  ![Přidání nového uživatele s přístupem správce](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Vytvořit uživatele s přístupu uživatele
Typické uživatelům, kteří potřebují přístup k datům náklady na správu, jako řídicí panely a sestavy musí mít přístup uživatelů k jejich zobrazení. Vytvořte nového uživatele s přístupem uživatele podobnou té, kterou jste vytvořili s přístupem správce, s následující rozdíly:

- Vymazat **povolit správu uživatelů**, **povolit příjemce uvádí správu**a zrušte zaškrtnutí všech v **má uživatel přístup správce** seznamu.
- Vyberte entity, že uživatel potřebuje přístup k v **má uživatel přístup uživatele** seznamu.
- Podle potřeby můžete také povolit správce přístup ke konkrétní entity.

![Přidání nového uživatele pomocí přístupu uživatele](.\media\tutorial-user-access\new-user-access.png)

Podívejte se na kurz video o přidávání uživatelů, najdete v tématu [přidání uživatelů do Azure náklady na správu Cloudyn](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Vytvořte entity

Když definujete vaše hierarchie entit náklady, osvědčeným postupem je k identifikaci struktuře vaší organizace.

Při sestavování stromu, zvažte, jak můžete potřebujete nebo chcete zobrazit jejich náklady oddělených obchodních jednotek, náklady centrech, prostředí a prodejní oddělení. Stromu entity v Cloudyn je flexibilní kvůli dědičnosti entity. Jednotlivé odběry pro vaše cloudové účty jsou propojeny s konkrétní entity. Ano entity jsou více klientů. Přístupu konkrétním uživatelům můžete přiřadit pouze jejich segmentu vaší firmy pomocí entity. Tím zajistí dat izolované, dokonce i v jiných velká část obchodní jako pobočky. A izolaci dat pomáhá s zásad správného řízení.  

Při registraci Azure smlouvy nebo účet s Cloudyn daty prostředků Azure, včetně využití, výkon, fakturace a značky data z vašich předplatných byl zkopírován do vašeho účtu Cloudyn. Však musíte ručně vytvořit vašeho stromu entity. Při přeskočení registrace Azure Resource Manager pak pouze fakturační údaje a několik sestavy asset jsou dostupné na portálu Cloudyn.

Na portálu Cloudyn, klikněte na tlačítko **nastavení** v horním pravém rohu a vyberte **cloudové účty**. Začněte s jednou entitou (uživatel root) a sestavení vašeho stromu entity v kořenovém adresáři. Tady je příklad hierarchie entit, který může vypadat řada organizací IT po dokončení stromu:

![entity stromu](.\media\tutorial-user-access\entity-tree.png)

Vedle **entity**, klikněte na tlačítko **Přidat entitu**. Zadejte informace o osobě nebo oddělení, které chcete přidat. **Úplný název** a **e-mailu** pole nemají tak, aby odpovídala stávající uživatele. Pokud chcete zobrazit seznam úrovně přístupu, vyhledejte v nápovědě pro *přidání entity*.

![Přidání entity](.\media\tutorial-user-access\add-entity.png)

Když jste hotovi, **Uložit** entity.


Podívejte se na kurz video o vytváření hierarchie entit náklady, najdete v tématu [vytváření hierarchie entit náklady v Azure náklady na správu Cloudyn](https://youtu.be/dAd9G7u0FmU).

Pokud se uživatel s Azure Enterprise Agreement, podívejte se na kurz video o přiřazení účtům a předplatným pro entity v [připojení k Azure Resource Manager s Azure náklady na správu Cloudyn](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvořit uživatele s přístupem správce
> * Vytvořit uživatele s přístupu uživatele
> * Vytvořte entity

Přechodu na v dalším kurzu se dozvíte, jak k prognózy výdajů pomocí historická data.

> [!div class="nextstepaction"]
> [Odhad budoucích výdajů](tutorial-forecast-spending.md)
