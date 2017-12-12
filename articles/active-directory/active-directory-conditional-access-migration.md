---
title: "Migraci na portálu Azure classic zásad | Microsoft Docs"
description: "Migrujte na portálu Azure classic zásady."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 416ae171564fa23473d09bad950d149e201bcc98
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migrace na portálu Azure classic zásady 


[Podmíněný přístup](active-directory-conditional-access-azure-portal.md) je funkce služby Azure Active directory (Azure AD), jak vám umožní řídit oprávnění uživatelům přístup cloudových aplikací. Při účelem je dál stejná, má verzi na nový portál Azure také zavedla významná vylepšení na tom, jak podmíněný přístup funguje. Zásady podmíněného přístupu, které jste nakonfigurovali mimo portál Azure mohou existovat vedle sebe na nové zásady, kterou vytvoříte na portálu Azure. Tak dlouho, dokud nejsou zakázáním nebo odebráním, použijí se stále ve vašem prostředí. Doporučujeme však migrovat do nového podmíněného Azure AD classic zásad zásady přístupu, protože:

- Nové zásady umožňují scénáře adresu nemohla zpracovat classic zásadám.

- Můžete snížit počet zásad, které budete muset spravovat konsolidovat.   

Toto téma vám pomůže migraci stávající classic zásady do nového nový Azure AD zásady podmíněného přístupu.


## <a name="classic-policies"></a>Classic zásady

Zásady podmíněného přístupu pro Azure AD a Intune na webu Azure portal jste nevytvořili se také označují jako **classic zásady**. Pokud chcete migrovat classic zásad, nemusíte mít přístup k portálu Azure classic. Portál Azure nabízí [ **zásady Classic (preview)** zobrazení](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) , můžete si projděte classic zásady.

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


### <a name="open-a-classic-policy"></a>Otevřete zásadu classic

**Chcete-li otevřít classic zásad:**

1. V [portál Azure](https://portal.azure.com), v levém navigační panel, klikněte na tlačítko **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Na **Azure Active Directory** stránky v **spravovat** klikněte na tlačítko **podmíněného přístupu**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/02.png)
 
2. Na **podmíněný přístup – zásady** stránky v **spravovat** klikněte na tlačítko **zásady Classic (preview)**.

3. Seznam classic zásad vyberte zásadu, kterou kterých vám nejvíc záleží.   

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/34.png)



## <a name="azure-ad-conditional-access-policies"></a>Zásady podmíněného přístupu Azure AD

Toto téma poskytuje vám podrobné kroky, které vám umožní migraci aniž byste museli být obeznámeni s podmíněným přístupem Azure AD classic zásad zásady. Však se seznámit se základními koncepty a přehled terminologie služby Azure AD pomáhá zlepšit prostředí migrace podmíněného přístupu.

Přejděte na téma:

- [Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md) Další informace o základních koncepcích a technologiím

- [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) Seznamte se s uživatelského rozhraní na portálu Azure


 





## <a name="multi-factor-authentication-policy"></a>Zásady vícefaktorového ověřování 

Tento příklad ukazuje, jak migrovat classic zásady, které vyžadují **služby Multi-Factor authentication** pro cloudové aplikace. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


**Pokud chcete migrovat classic zásad:**

1. [Otevřete classic zásady](#open-a-classic-policy) získat nastavení konfigurace.
2. Vytvoření nové zásady podmíněného přístupu Azure AD classic zásady nahradit. 


### <a name="create-a-new-conditional-access-policy"></a>Vytvořit novou zásadu podmíněného přístupu


1. V [portál Azure](https://portal.azure.com), v levém navigační panel, klikněte na tlačítko **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Na **Azure Active Directory** stránky v **spravovat** klikněte na tlačítko **podmíněného přístupu**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/02.png)



3. Na **podmíněného přístupu** stránky, otevřete **nový** stránky, na panelu nástrojů v horní části klikněte na tlačítko **přidat**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/03.png)

4. Na **nový** stránky v **název** textovému poli, zadejte název zásady.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/29.png)

5. V **přiřazení** klikněte na tlačítko **uživatelů a skupin**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/05.png)

    a. Pokud máte všechny vybrané v classic zásady uživatele, klikněte na tlačítko **všichni uživatelé**. 

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/35.png)

    b. Pokud máte vybrané v classic zásady skupiny, klikněte na tlačítko **vyberte uživatele a skupiny**a potom vyberte požadovaných uživatelů a skupin.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/36.png)

    c. Pokud máte vyloučené skupiny, klikněte na tlačítko **vyloučit** a potom vyberte požadované uživatele a skupiny. 

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/37.png)

6. Na **nový** stránky, otevřete **cloudových aplikací** stránky v **přiřazení** klikněte na tlačítko **cloudových aplikací**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Na **cloudových aplikací** proveďte následující kroky:

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/08.png)

    a. Klikněte na tlačítko **vybrat aplikace**.

    b. Klikněte na **Vybrat**.

    c. Na **vyberte** vyberte cloudové aplikace a pak klikněte na tlačítko **vyberte**.

    d. Na **cloudových aplikací** klikněte na tlačítko **provádí**.



9. Pokud máte **vyžadovat vícefaktorové ověřování** vybrané:

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/26.png)

    a. V **přístup k ovládacím prvkům** klikněte na tlačítko **Grant**.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/27.png)

    b. Na **Grant** klikněte na tlačítko **udělit přístup**a potom klikněte na **vyžadovat vícefaktorové ověřování**.

    c. Klikněte na **Vybrat**.


10. Klikněte na tlačítko **na** povolit vaše zásady.

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/30.png)

11. Zakažte classic zásadu. 

    ![Podmíněný přístup](./media/active-directory-conditional-access-migration/38.png)



 


## <a name="next-steps"></a>Další kroky

- Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 
