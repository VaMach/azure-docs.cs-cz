---
title: 'Kurz: Azure Active Directory integrace s Cisco Webex | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cisco Webex."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 835b7c139fc466719489c6fd1986dcbf16de549f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Kurz: Azure Active Directory integrace s Cisco Webex

V tomto kurzu zjistěte, jak integrovat Cisco Webex s Azure Active Directory (Azure AD).

Integrace Cisco Webex s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k Cisco Webex.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Cisco Webex (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Cisco Webex, potřebujete následující položky:

- Předplatné služby Azure AD
- Cisco Webex jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Cisco Webex z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-cisco-webex-from-the-gallery"></a>Přidání Cisco Webex z Galerie
Při konfiguraci integrace Cisco Webex do služby Azure AD potřebujete přidat Cisco Webex z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Cisco Webex z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Cisco Webex**, vyberte **Cisco Webex** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Cisco Webex v seznamu výsledků](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Webex podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Cisco Webex je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Cisco Webex musí navázat.

V Cisco Webex přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Webex, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Cisco Webex](#create-a-cisco-webex-test-user)**  – Pokud chcete mít protějšek Britta Simon v Cisco Webex propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cisco Webex.

**Ke konfiguraci Azure AD jednotné přihlašování s Cisco Webex, proveďte následující kroky:**

1. Na portálu Azure na **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. Na **Cisco Webex domény a adresy URL** část, proveďte následující kroky:

    ![Cisco Webex domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.webex.com`

    b. V **identifikátor** textovému poli, zadejte adresu URL:`http://www.webex.com`

    c. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečná adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [tým podpory Cisco Webex klienta](https://www.webex.co.in/support/support-overview.html) k získání těchto hodnot. 

5. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_400.png)
    
6. Na **konfigurace Cisco Webex** klikněte na tlačítko **konfigurace Cisco Webex** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se na váš web společnosti Cisco Webex jako správce.

8. V nabídce v horní části, klikněte na tlačítko **Správa webu**.

    ![Správa lokalit](./media/active-directory-saas-cisco-webex-tutorial/ic777621.png "lokality správy")

9. V **spravovat lokality** klikněte na tlačítko **Konfigurace jednotného přihlašování k**.
   
    ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-cisco-webex-tutorial/ic777622.png "Konfigurace jednotného přihlašování")

10. V **federovaného jednotného přihlašování k konfigurace webové** část, proveďte následující kroky:
   
    ![Konfigurace jednotného přihlašování federovaného](./media/active-directory-saas-cisco-webex-tutorial/ic777623.png "federovaného jednotného přihlašování k konfigurace")  

    a. Z **Federation protokol** seznamu, vyberte **SAML 2.0**.

    b. Jako **profil jednotného přihlašování k**, vyberte **SP Intiated**.

    c. Otevřete stažený certifikát v programu Poznámkový blok a zkopírujte obsah ho.

    d. Klikněte na tlačítko **importovat Metadata SAML**a potom vložte zkopírovaný obsahu certifikátu.

    e. V **vystavitele pro SAML (IdP ID)** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.

    f. V **adresy URL zákazníka jednotného přihlašování služby přihlášení** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    g. Z **NameID formátu** seznamu, vyberte **e-mailová adresa**.

    h. V **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.

    i. V **adresy URL odhlašovací zákazníka jednotného přihlašování služby** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.
   
    j. Klikněte na tlačítko **aktualizace**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-cisco-webex-test-user"></a>Vytvoření zkušebního uživatele Cisco Webex

Pokud chcete povolit uživatelům Azure AD přihlášení do Cisco Webex, musí být zřízená do Cisco Webex. V případě Cisco Webex zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k vaší **Cisco Webex** klienta.

2. Přejděte na **Správa uživatelů \> přidat uživatele**.
   
    ![Přidání uživatelů](./media/active-directory-saas-cisco-webex-tutorial/ic777625.png "přidat uživatele")

3. V části přidat uživatele proveďte následující kroky:
   
    ![Přidat uživatele](./media/active-directory-saas-cisco-webex-tutorial/ic777626.png "přidat uživatele")   

    a. Jako **typ účtu**, vyberte **hostitele**.

    b. V **křestní jméno** textovému poli, název typu první uživatele jako Britta.

    c. V **příjmení** textovému poli, zadejte příjmení uživatele jako Simon.

    d. V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.

    e. V **e-mailu** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    f. V **heslo** textovému poli, zadejte heslo uživatele.

    g. V **potvrdit** textové hesla, zadejte znovu heslo uživatele.

    h. Klikněte na tlačítko **Přidat**.

>[!NOTE]
>Můžete použít všechny ostatní Cisco Webex uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cisco Webex zřizovat AAD uživatelské účty. 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Cisco Webex.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon Cisco Webex, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Cisco Webex**.

    ![V seznamu aplikací na odkaz Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Cisco Webex na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Cisco Webex.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_203.png

