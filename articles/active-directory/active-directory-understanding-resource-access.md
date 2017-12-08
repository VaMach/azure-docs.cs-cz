---
title: "Principy přístupu k prostředkům v Azure | Microsoft Docs"
description: "Toto téma vysvětluje koncepty o používání správci předplatného k řízení přístupu k prostředkům v plné verzi portálu Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 9492afeda8c11d9d4df866e416a2c2c7e1684569
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="understanding-resource-access-in-azure"></a>Principy přístupu k prostředkům v Azure

Spustí se z hlediska fakturační řízení přístupu v Azure. Vlastník účtu Azure, navštivte stránky přístup [centra účtů Azure](https://account.windowsazure.com/subscriptions), je správce účtu (AA). Předplatné je kontejner pro fakturaci, ale také slouží jako hranice zabezpečení: každého předplatného se služby správce (SA), který můžete přidat, odebrat a úpravy prostředků v tomto předplatném Azure pomocí [portál Azure](https://portal.azure.com/). Výchozí SA nového předplatného je AA, ale AA změnit přidružení zabezpečení v centru účtů Azure.

<br><br>![Účty Azure][1]

Odběry mají také přidružení s adresářem. Adresář definuje sadu uživatelů. Může jít o uživatele z práce nebo škola vytvořený adresář nebo může se jednat o externí uživatele (to znamená, Accounts Microsoft). Odběry jsou přístupné pro podmnožinu těchto directory uživatelů, kteří mají přiřazený jako služba správce nebo Spolusprávce (CA); Jedinou výjimkou je, že starší verze důvodů Accounts Microsoft (dříve Windows Live ID) může být přiřazen jako SA nebo certifikační Autority bez se nachází v adresáři.

<br><br>![Řízení přístupu v Azure][2]

Funkcionalitu v rámci portálu Azure classic umožňuje SAs, které jsou podepsané pomocí Account Microsoft Chcete-li změnit adresář, který je přidružen odběru pomocí **upravit adresář** příkaz na **odběry** stránky v **nastavení**. Všimněte si, že tato operace má dopad na řízení přístupu daného předplatného.

> [!NOTE]
> **Upravit adresář** příkaz na portálu Azure classic není k dispozici pro uživatele, kteří jsou přihlášení pomocí pracovního nebo školního účtu, protože tyto účty se můžete přihlásit pouze k adresáři, do které patří.
> 
> 

<br><br>![Tok přihlášení jednoduchá uživatele][3]

V případě jednoduchého bude organizace (například Contoso) vynutit fakturace a řízení přístupu v stejnou sadu odběry. Adresář je přidružen k odběry, které jsou vlastněny jeden účet Azure. Po úspěšném přihlášení k portálu Azure classic se uživatelům zobrazí dvě kolekce prostředků (znázorněný v oranžová na předchozím obrázku):

* Adresáře, kde existuje uživatelského účtu (jako zdroj nebo přidat jako cizí objekt zabezpečení). Všimněte si, že adresáři použít pro přihlášení není relevantní pro tento výpočet tak adresářů se vždy zobrazí bez ohledu na to, kde jste se přihlásili.
* Prostředky, které jsou součástí odběry, které jsou přidruženy k adresáři použít pro přihlášení a který má uživatel přístup (kde se jedná SA nebo certifikační Autority).

<br><br>![Uživatel s více předplatnými a adresáři][4]

Možnost přepnout aktuální kontext portálu Azure classic pomocí filtru předplatné mít uživatelé s odběry v několika adresářích. V pozadí výsledkem je samostatný přihlášení do jiného adresáře, ale to se provádí bezproblémově pomocí jednotného přihlašování (SSO).

Operace, například přesun prostředků mezi předplatnými může být obtížnější v důsledku toto zobrazení jednoho adresářového předplatných. Pokud chcete provést přenos prostředků, může být potřeba první použití **upravit adresář** příkaz na stránce předplatných v **nastavení** přidružení předplatných do stejného adresáře.

## <a name="next-steps"></a>Další kroky
* Další informace o tom, jak změnit správce pro předplatné služby Azure naleznete v tématu [Postup přidání nebo změna role správce služby Azure](../billing/billing-add-change-azure-subscription-administrator.md)
* Další informace o tom, jak Azure Active Directory má vztah k předplatnému Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* Další informace o tom, jak přiřadit role ve službě Azure AD, najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
