---
title: "Přidání nebo změna role správce Azure předplatné | Microsoft Docs"
description: "Popisuje, jak přidat nebo změnit společné správce Azure, Správce služby a účet správce"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/30/2017
ms.author: genli
ms.openlocfilehash: d78174cd968c0f918a07027daf1e59665d6b6c1e
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby

Správce Azure, který slouží ke správě vašeho předplatného Azure nebo spravuje služby Azure používá v rámci vašeho předplatného, můžete změnit. K zobrazení informací o Azure fakturace a Správa odběrů, musíte se přihlásit k centru účet jako správce účtu. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Přidat správce RBAC vlastníka pro přihlášení k odběru na portálu Azure 

Někdo přidat jako správce pro přihlášení k odběru na portálu Azure, doporučujeme, abyste jim [RBAC](../active-directory/role-based-access-control-configure.md) roli vlastníka. Role vlastníka umožňuje spravovat prostředky v předplatném přiřazen a nemá oprávnění přístupu pro další odběry. Vlastníci přidáte prostřednictvím [portál Azure](https://portal.azure.com) nemůže spravovat prostředku v [portál Azure classic](https://manage.windowsazure.com).

1. Přihlaste se k [odběry zobrazit na portálu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Vyberte odběr, který chcete, aby správce pro přístup.
1. Vyberte **přístup k ovládacímu prvku (IAM)** v nabídce.
1. Vyberte **přidat** > **Role** > **vlastníka**. Zadejte e-mailovou adresu uživatele, které chcete přidat jako vlastník, vyberte uživatele a potom vyberte **Uložit**.

    ![Snímek obrazovky zobrazující vybrané roli vlastníka](./media/billing-add-change-azure-subscription-administrator/add-role.png)

## <a name="add-or-change-co-administrator"></a>Přidat nebo změnit spolusprávcem

Jako správce a společné lze přidat pouze vlastníka. Ostatní uživatelé s rolí, například Přispěvatel a čtečky nelze přidat jako spolusprávce.

1. Pokud jste to ještě neudělali, přidejte někdo jako vlastníka následující z výše uvedených pokynů.
2. **Klikněte pravým tlačítkem na** uživatel vlastníka jste právě přidali a pak vyberte **přidat jako spolusprávce**. Pokud se nezobrazí **přidat jako spolusprávce** možnost čerstvé stránce nebo zkuste jinou internetového prohlížeče. 

     ![Snímek obrazovky, který přidá společné správce](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Je nutné přidat jako spolusprávce účet "Vlastník", pokud uživatel potřebuje ke správě služby Azure v [portál Azure classic](https://manage.windowsazure.com/).

    K odebrání oprávnění spolusprávcem **klikněte pravým tlačítkem na** uživatele "Společné správce" a pak vyberte **odebrat spolusprávcem**.

    ![Snímek obrazovky, který odebere společné správce](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Změnit správce služeb pro předplatné Azure

Pouze správce účtu může změnit správce služeb pro předplatné. Ve výchozím nastavení když se přihlásíte, Správce služby je stejný jako účet správce.

1. Zajistěte, aby váš scénář je podporován kontrolou [limity pro změnu Správci služeb](#limits).
1. Přihlaste se k [centra účtů](https://account.windowsazure.com/subscriptions) jako správce účtu.
1. Vyberte předplatné.
1. Na pravé straně, vyberte **upravit podrobnosti o předplatném**.

    ![Snímek obrazovky tlačítka Upravit předplatného v centru účtů](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. V **Správce služeb** zadejte e-mailovou adresu z nového správce služby.

    ![Snímek obrazovky s poli, chcete-li změnit e-mailu Správce služby](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Omezení pro změnu Správci služeb

* Každé předplatné je spojeno s adresář služby Azure AD. Chcete-li najít adresář předplatné je spojeno s, přejděte na [portál Azure classic](https://manage.windowsazure.com/), vyberte **nastavení** > **odběry**. Zkontrolujte ID předplatného najít adresář.
* Pokud jste přihlášení pomocí pracovního nebo školního účtu, můžete přidat další účty ve vaší organizaci jako správce služeb. Například abby@contoso.com můžete přidat bob@contoso.com jako správce služby, ale nemůžete přidat john@notcontoso.com Pokud john@notcontoso.com má přítomnosti v adresáři contoso.com. Uživatelé přihlášeni s použitím pracovní nebo školní účty můžete dále přidat Account Microsoft uživatele jako správce služeb.

  | Metoda přihlašování | Přidat uživatele Account Microsoft jako SA? | Přidat pracovní nebo školní účet v téže organizaci jako SA? | Přidat pracovní nebo školní účet v jiné organizaci, jako SA? |
  | --- | --- | --- | --- |
  |  Účet Microsoft |Ano |Ne |Ne |
  |  Pracovní nebo školní účet |Ano |Ano |Ne |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Změnit správce účtu pro předplatné Azure

Chcete-li změnit správce účtu předplatného, [přenos vlastnictví předplatného služby Azure na jiný účet](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Nejste si jisti, který je správce účtu?** Postupujte následovně:

1. Přihlaste se k [odběry zobrazit na portálu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Vyberte předplatné, které chcete kontrolovat, a pak hledejte v části **nastavení**.
1. Vyberte **vlastnosti**. Správce účtu předplatného se zobrazí v **správce účtu** pole.  

## <a name="types-of-azure-admin-accounts"></a>Typy účtů správce Azure

 Správce účtu, Správce služeb a spolusprávce jsou tři druhy rolí správců v Microsoft Azure. Následující tabulka popisuje rozdíl mezi tyto tři role správce.

| Role pro správu | Omezení | Popis |
| --- | --- | --- |
| Správce účtu (AA) |1 na účet Azure |Toto je osoba, která registraci aplikace nebo koupili předplatných Azure a má oprávnění k přístupu [centra účtů](https://account.azure.com/Subscriptions) a provádět různé úlohy správy. Mezi ně patří schopnost vytvářet odběry, zrušit předplatné, změnit fakturace předplatného a změnit správce služeb. |
| Správce služeb (SA) |1 za předplatné Azure |Tato role je oprávnění ke správě služeb v [portál Azure](https://portal.azure.com). Ve výchozím nastavení pro nové předplatné je správce účtu také Správce služeb. |
| Spolusprávce (CA) v [portál Azure classic](https://manage.windowsazure.com) |200 na předplatné |Tato role má stejná přístupová oprávnění jako správce služeb, ale nemůže změnit přidružení předplatných k adresářům Azure. |

Azure na základě Role služby Active Directory řízení přístupu (RBAC) umožňuje uživatelům přidat k více rolím. Další informace najdete v tématu [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md).


## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Další informace o řízení přístupu k prostředkům a služby Active Directory

* Další informace o tom, jak je přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům v Azure](../active-directory/active-directory-understanding-resource-access.md).
* Další informace o službě Azure Active Directory najdete v tématu [asociování předplatných Azure se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) a [přiřazení rolí správce v Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.
