---
title: "Kurz: Azure Active Directory integrace s Wizergos produkční Software | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi softwarem Wizergos produktivitu a Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Kurz: Azure Active Directory integrace s Wizergos produktivitu softwaru
Cílem tohoto kurzu je ukazují, jak integrovat Wizergos produkční Software s Azure Active Directory (Azure AD).

Integrace Wizergos produkční Software s Azure AD poskytuje následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup k softwaru Wizergos produktivitu
* Můžete povolit uživatelům, aby automaticky získat přihlášení k softwaru produktivitu Wizergos jednotné přihlašování (SSO) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Konfigurace integrace Azure AD s Wizergos produkční Software, potřebujete následující položky:

* Předplatné služby Azure AD
* Povolené předplatné produktivity Wizergos, jednotného přihlašování k softwaru

>[!NOTE]
>K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní Azure AD jednotného přihlašování k testování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání softwaru produktivitu Wizergos z Galerie
2. Konfigurace a testování Azure AD SSO

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Přidání softwaru produktivitu Wizergos z Galerie
Při konfiguraci integrace Wizergos produktivitu softwaru do služby Azure AD, potřebujete přidat Wizergos produktivitu softwaru z Galerie si na seznam spravovaných aplikací SaaS.

**Chcete-li přidat Wizergos produktivitu softwaru z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4]
6. Do vyhledávacího pole zadejte **Wizergos produktivitu softwaru**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. Na panelu výsledků vyberte **Wizergos produktivitu softwaru**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Výběr aplikace v galerii](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurace a otestování Azure AD SSO
Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD přihlášení SSO se Wizergos produktivitu Software založený na testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v Wizergos produktivitu softwaru pro uživatele ve službě Azure AD je Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Wizergos produktivitu softwaru je nutné stanovit.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Wizergos produktivitu softwaru.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Softwareder BynWizergos produktivitu, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele softwaru produktivitu Wizergos](#creating-a-wizergos-productivity-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v Wizergos produktivitu Software, který je propojený s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-sso"></a>Konfigurace Azure AD jednotného přihlašování
V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Wizergos produktivitu softwaru.

**Ke konfiguraci Azure AD jednotné přihlašování s Wizergos produkční Software, proveďte následující kroky:**

1. Na portálu classic na **Wizergos produktivitu softwaru** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na **jak chcete uživatelům se přihlásit Software pro zvýšení produktivity Wizergos** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. Na **nakonfigurovat nastavení aplikace** dialogové okno stránky, klikněte na tlačítko **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. Na **nakonfigurovat jednotné přihlašování v softwaru produktivitu Wizergos** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor v počítači:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. V okně prohlížeče jiných webových přihlašování ke klientovi Wizergos produktivitu softwaru jako správce.
6. Vyberte z nabídky hamburger **správce**.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. Správce stránce v nabídce vlevo vyberte **ověřování** a klikněte na **Azure AD**.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Proveďte následující kroky na **ověřování** části.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Klikněte na tlačítko **NAHRÁT** tlačítko, na kterou odešlete certifikát stažený z Azure AD. 
  2. V **URL vystavitele** textbox vložte hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.
  3. V **adresy jednotného přihlašování** textbox vložte hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.
  4. V **jednu adresu URL Sign-Out** textbox vložte hodnotu **jednu adresu URL služby Sign-out** z Průvodce konfigurací aplikace Azure AD.
  5. Klikněte na tlačítko **Uložit** tlačítko.
9. Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][10]
10. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.
6. Na **profil uživatele** dialogové okno proveďte následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. V **křestní jméno** textovému poli, typ **Britta**.  
  2. V **příjmení** textovému poli, typ, **Simon**.
  3. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.
7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Poznamenejte si hodnotu **nové heslo**.
  2. Klikněte na **Dokončit**.   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Vytvoření zkušebního uživatele Wizergos produktivitu softwaru
V této části vytvoříte volal Britta Simon v softwaru Wizergos produktivitu uživatele. Spojte se s tým podpory Wizergos produkční Software prostřednictvím [ support@wizergos.com ](emailTo:support@wizergos.com) přidejte uživatele v platformě Wizergos produktivitu softwaru.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD
Cílem této části je povolení Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k Wizergos produktivitu softwaru.

  ![Přiřadit uživatele][200]

**Pokud chcete přiřadit Britta Simon Wizergos produkční Software, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][201]
2. V seznamu aplikací vyberte **Wizergos produktivitu softwaru**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203]
4. V seznamu uživatelů vyberte **Britta Simon**.
5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.

Když kliknete na dlaždici Wizergos produktivitu softwaru na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Wizergos produktivitu softwaru.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
