---
title: 'Kurz: Azure Active Directory integrace s GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: jeedes
ms.openlocfilehash: 44a1a7d1c3116af71e07f9579dd8b7570693b813
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Kurz: Azure Active Directory integrace s GoToMeeting

V tomto kurzu zjistěte, jak integrovat GoToMeeting s Azure Active Directory (Azure AD).

Integrace GoToMeeting s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k GoToMeeting.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k GoToMeeting (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s GoToMeeting, potřebujete následující položky:

- Předplatné služby Azure AD
- GoToMeeting jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání GoToMeeting z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-gotomeeting-from-the-gallery"></a>Přidání GoToMeeting z Galerie
Při konfiguraci integrace GoToMeeting do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS GoToMeeting z galerie.

**Pokud chcete přidat GoToMeeting z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **GoToMeeting**, vyberte **GoToMeeting** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![GoToMeeting v seznamu výsledků](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s GoToMeeting podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v GoToMeeting je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v GoToMeeting musí navázat.

V GoToMeeting, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s GoToMeeting, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele GoToMeeting](#create-a-gotomeeting-test-user)**  – Pokud chcete mít protějšek Britta Simon v GoToMeeting propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci GoToMeeting.

**Ke konfiguraci Azure AD jednotné přihlašování s GoToMeeting, proveďte následující kroky:**

1. Na portálu Azure na **GoToMeeting** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Na **GoToMeeting domény a adresy URL** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** a proveďte následující akce -

    ![GoToMeeting domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_ssourl.png)

    a. V **identifikátor** textovému poli, zadejte adresu URL:`https://authentication.logmeininc.com/saml/sp`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL:`https://authentication.logmeininc.com/saml/acs`

    c. V **předávání stavu** textovému poli, zadejte jeden z těchto adres URL:

    **Pro GoToMeeting**:`https://global.gotomeeting.com`
    
    **Pro GoToTraining**:`https://global.gototraining.com`

    **Pro GoToWebinar**:`https://global.gotowebinar.com`

    **Pro GoToAssist**:`https://app.gotoassist.com`


4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_400.png)

6. Ke generování **Metadata** adresu url, proveďte následující kroky:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpointicon.png)

    c. Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_endpoint.png)
     
    d. Nyní přejděte na stránku vlastností **GoToMeeting** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_appid.png)

    e. Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`   

7. Na **GoToMeeting konfigurace** klikněte na tlačítko **konfigurace GoToMeeting** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurace GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_configure.png) 

8. V okně jiný prohlížeč, přihlaste se k vaší [GoToMeeting organizace Center](https://account.citrixonline.com/organization/administration/)

9. V části **zprostředkovatele identity** kartě, můžete nakonfigurovat nastavení Azure buď pomocí poskytování vygenerovaného **adresu URL metadat** nebo stažené **soubor metadat** nebo **Ruční**.

10. Pro **adresu URL metadat** proveďte následující kroky:

    ![Konfigurace GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config1.png)

    a. V **jak byste chtěli nakonfigurovat vaše SAML IDP?**, vyberte **automatické** z rozevíracího seznamu.

    b. Vložení **adresu URL metadat**, který se mají vygenerovat v předchozích krocích do **adresu URL metadat** textové pole.

    c. Klikněte na **Uložit**.

11. Pro **soubor metadat** proveďte následující kroky:

    ![Konfigurace GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config2.png)

    a. V **jak byste chtěli nakonfigurovat vaše SAML IDP?**, vyberte **SAML nahrát soubor metadat** z rozevíracího seznamu.

    b. Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **metadata souboru k odeslání**.

    c. Klikněte na **Uložit**.

12. Pro **ruční** proveďte následující kroky:

    ![Konfigurace GoToMeeting](./media/active-directory-saas-gotomeeting-tutorial/config3.png)

    a.  V **přihlašovací adresa URL stránky** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    b.  V **adresy URL odhlašovací stránky** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.

    c.  V **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.

    d. Rozbalte certifikátu x 509 z soubor stažený metadat a kliknutím na tento certifikát nahrát **nahrání certifikátu**.

    e. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-gotomeeting-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-gotomeeting-test-user"></a>Vytvoření zkušebního uživatele GoToMeeting

V této části se uživatel volá Britta Simon vytvoří v GoToMeeting. GoToMeeting podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v GoToMeeting ještě neexistuje, vytvoří se při pokusu o přístup k GoToMeeting novou.

> [!NOTE]
> Pokud je potřeba ručně vytvořit uživatele, obraťte se na [tým podpory GoToMeeting](https://support.logmeininc.com/gotomeeting).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu GoToMeeting.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon GoToMeeting, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **GoToMeeting**.

    ![V seznamu aplikací na GoToMeeting odkaz](./media/active-directory-saas-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici GoToMeeting na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci GoToMeeting.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gotomeeting-tutorial/tutorial_general_203.png

