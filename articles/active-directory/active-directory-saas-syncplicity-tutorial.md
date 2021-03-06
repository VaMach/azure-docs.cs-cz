---
title: 'Kurz: Azure Active Directory integrace s Syncplicity | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a42e17539df4d4c0e57f5a5541232968f897160e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Kurz: Azure Active Directory integrace s Syncplicity

V tomto kurzu zjistěte, jak integrovat Syncplicity s Azure Active Directory (Azure AD).

Integrace Syncplicity s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Syncplicity
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Syncplicity (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Syncplicity, potřebujete následující položky:

- Předplatné služby Azure AD
- Syncplicity jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Syncplicity z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-syncplicity-from-the-gallery"></a>Přidání Syncplicity z Galerie
Při konfiguraci integrace Syncplicity do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Syncplicity z galerie.

**Pokud chcete přidat Syncplicity z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Syncplicity**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. Na panelu výsledků vyberte **Syncplicity**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Syncplicity je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Syncplicity musí navázat.

V Syncplicity, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Syncplicity](#creating-a-syncplicity-test-user)**  – Pokud chcete mít protějšek Britta Simon v Syncplicity propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Syncplicity.

**Ke konfiguraci Azure AD jednotné přihlašování s Syncplicity, proveďte následující kroky:**

1. Na portálu Azure na **Syncplicity** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. Na **Syncplicity domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.syncplicity.com`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Syncplicity klienta](https://www.syncplicity.com/contact-us) k získání těchto hodnot. 
 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. Na **Syncplicity konfigurace** klikněte na tlačítko **konfigurace Syncplicity** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Přihlaste se k vaší **Syncplicity** klienta.

8. V nabídce v horní části, klikněte na tlačítko **správce**, vyberte **nastavení**a potom klikněte na **vlastní domény a jednotné přihlašování**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. Na **jednotné přihlašování (SSO)** dialogové okno stránky, proveďte následující kroky:
   
    ![Jednotné přihlašování \(jednotného přihlašování\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. V **vlastní domény** textovému poli, zadejte název vaší domény.
  
    b. Vyberte **povoleno** jako **jednotné přihlašování stav**.

    c. V **Entity Id** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.

    d. V **přihlašovací adresa URL stránky** textovému poli, Vložit **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    e. V **adresa URL odhlašovací stránky** textovému poli, Vložit **Sign-Out URL** který jste zkopírovali z portálu Azure.

    f. V **certifikát zprostředkovatele Identity**, klikněte na tlačítko **zvolte soubor**a pak nahrajte certifikát, který jste si stáhli z portálu Azure. 

    g. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-syncplicity-test-user"></a>Vytvoření zkušebního uživatele Syncplicity
AAD uživatelé moci přihlásit musí být zřízená Syncplicity aplikaci. Tato část popisuje, jak ve Syncplicity vytvořit uživatelské účty AAD.

**K poskytnutí uživatelského účtu do Syncplicity, proveďte následující kroky:**

1. Přihlaste se k vaší **Syncplicity** klienta (například: `https://company.Syncplicity.com`).

2. Klikněte na tlačítko **správce** a vyberte **uživatelské účty**.

3. Klikněte na tlačítko **přidat uživatele**.
   
    ![Správa uživatelů](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Správa uživatelů")

4. Typ **e-mailových adres** účtu AAD, které chcete zřídit, vyberte **uživatele** jako **Role**a potom klikněte na **Další**.
   
    ![Informace o účtu](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "účet informace")
   
    >[!NOTE]
    >Držitel účtu AAD získá e-mailu, včetně odkaz k potvrzení a aktivovat účet. 
    > 

5. Vyberte skupinu ve vaší společnosti, nový uživatel by měl stane členem, a pak klikněte na **Další**.
   
    ![Členství ve skupinách](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "členství ve skupinách")
   
    >[!NOTE]
    >Pokud nebyly nalezeny žádné skupiny uvedeny, klikněte na tlačítko **Další**. 
    > 

6. Vyberte složky, kterou chcete umístit pod kontrolou společnosti Syncplicity na počítači uživatele a potom klikněte na **Další**.
   
    ![Složky Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity složek")

>[!NOTE]
>Můžete použít všechny ostatní Syncplicity uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Syncplicity zřídit AAD uživatelské účty. 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Syncplicity.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Syncplicity, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Syncplicity**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.

Když kliknete na dlaždici Syncplicity na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Syncplicity.
## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

