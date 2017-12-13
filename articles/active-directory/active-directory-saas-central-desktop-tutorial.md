---
title: "Kurz: Azure Active Directory integrace s centrální plochy | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a centrální plochy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 62f1a1e2193d9693519bdea86196cf296d4b9780
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Kurz: Azure Active Directory integrace s centrální plochy

V tomto kurzu zjistěte, jak integrovat centrální plochy s Azure Active Directory (Azure AD).

Integrace centrální plochy s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k centrální plochy.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k centrální Desktop (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s centrální Desktop, potřebujete následující položky:

- Předplatné služby Azure AD
- Ploše centrální jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání centrální plochy z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-central-desktop-from-the-gallery"></a>Přidání centrální plochy z Galerie
Při konfiguraci integrace centrální plochy do služby Azure AD, potřebujete přidat centrální plochy z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat centrální plochy z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **centrální plochy**, vyberte **centrální plochy** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Centrální plochy v seznamu výsledků](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s centrální plochy podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v centrální plochy je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v centrální plochy musí navázat.

V centrální plochy přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s centrální plocha, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele centrální plochy](#create-a-central-desktop-test-user)**  – Pokud chcete mít protějšek Britta Simon v centrální plochy, která je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Centrální plochy.

**Ke konfiguraci Azure AD jednotné přihlašování pomocí centrální plochy, proveďte následující kroky:**

1. Na portálu Azure na **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. Na **centrální plochy domény a adresy URL** část, proveďte následující kroky:

    ![Centrální plochy domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.centraldesktop.com`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [tým podpory centrální klienta plochy](https://imeetcentral.com/contact-us) k získání těchto hodnot. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. Na **centrální konfigurace plochy** klikněte na tlačítko **nakonfigurovat centrální plochy** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurace centrálního plochy](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Přihlaste se k vaší **centrální plochy** klienta.

8. Přejděte na **nastavení**, klikněte na tlačítko **Upřesnit**a potom klikněte na **jednotné přihlašování**.

    ![Instalační program – pokročilé](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "instalační program – Rozšířená")

9. Na **jeden znak v nastavení** proveďte následující kroky:

    ![Jednotné přihlašování v nastavení](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "jednotné přihlašování v nastavení")
    
    a. Vyberte **povolit SAML v2 jednotného přihlašování**.
    
    b. V **jednotného přihlašování k adrese URL** textovému poli, Vložit **SAML Entity ID** hodnotu, která jste zkopírovali z portálu Azure.
    
    c. V **adresu URL pro přihlášení SSO** textovému poli, Vložit **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure.
    
    d. V **adresy URL odhlašovací jednotného přihlašování k** textovému poli, Vložit **Sign-Out URL** hodnotu, která jste zkopírovali z portálu Azure.

10. V **metodu ověřování podpisu zpráva** část, proveďte následující kroky:

    ![Zpráva metodu ověřování podpisu](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "zprávy metodu ověřování podpisu") . Vyberte **certifikát**.
    
    b. Z **certifikát jednotného přihlašování k** seznamu, vyberte **RSH SHA256**.
    
    c. V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah certifikátu a vložte ji do **certifikát jednotného přihlašování k** pole.
        
    d. Vyberte **zobrazí odkaz na přihlašovací stránku vaší SAMLv2**.
    
    e. Klikněte na tlačítko **aktualizace**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-central-desktop-test-user"></a>Vytvoření zkušebního uživatele centrální plochy

Azure AD uživatelé moci přihlásit musí být zřízená aplikaci Centrální plochy. Tato část popisuje, jak vytvořit uživatelské účty Azure AD v centrální plochy.

> [!NOTE]
> Další nástroje pro tvorbu centrální plochy uživatele účtu nebo rozhraní API poskytovaných Centrální plochy můžete použít ke zřízení uživatelských účtů Azure AD

**Ke zřízení uživatelských účtů do centrální plochy:**

1. Přihlaste se ke klientovi centrální plochy.

2. Přejděte na **osoby \> vnitřní členy**.

3. Klikněte na tlačítko **přidat vnitřní členy**.

    ![Lidé](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "osoby")
    
4. V **e-mailovou adresu nové členy** textovému poli, zadejte účet služby Azure AD určené ke zřízení a potom klikněte na **Další**.

    ![E-mailové adresy nové členy](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "e-mailové adresy nové členy")

5. Klikněte na tlačítko **přidejte interní členy**.

    ![Přidání interní člena](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "přidat vnitřní člen")
   
   >[!NOTE]
   >Uživatelé, pro které jste přidali obdrží e-mail, který obsahuje odkaz potvrzení, které potřebují k klikněte na možnost aktivovat účet.
   
### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu centrální plochy.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon centrální plochy, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **centrální plochy**.

    ![V seznamu aplikací na odkaz centrální plochy](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici centrální plochy na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Centrální plochy.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_203.png

