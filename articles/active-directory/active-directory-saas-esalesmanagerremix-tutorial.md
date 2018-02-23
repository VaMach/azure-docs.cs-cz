---
title: 'Kurz: Azure Active Directory integrace s E prodej Manager Remix | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Remix Manager E organizační jednotky prodej."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Kurz: Azure Active Directory integrace s Remix Manager prodej E

V tomto kurzu zjistěte, jak integrovat E prodej Manager Remix s Azure Active Directory (Azure AD).

Integrace E prodej Manager Remix s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k E prodej Manager Remix.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k E prodej Manager Remix (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Remix Manager E prodej, potřebujete následující položky:

- Předplatné služby Azure AD
- Remix Manager prodej E jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání E prodej Manager Remix z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>Přidání E prodej Manager Remix z Galerie
Chcete-li nakonfigurovat integraci E prodej Manager Remix do služby Azure AD, přidejte E prodej Manager Remix z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat E prodej Manager Remix z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **E prodej Manager Remix**, vyberte **E prodej Manager Remix** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![E prodej Manager Remix v seznamu výsledků](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s E prodej Manager Remix podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v E prodej Manager Remix je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživateli v E prodej Manager Remix musí navázat.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Remix Manager E prodeje, musíte dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele E prodej Manager Remix](#create-an-e-sales-manager-remix-test-user)**  – Pokud chcete mít protějšek Britta Simon v E prodej Manager Remix propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci E prodej Manager Remix.

**Ke konfiguraci Azure AD jednotné přihlašování s Remix Manager E prodej, proveďte následující kroky:**

1. Na portálu Azure na **E prodej Manager Remix** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Na **E prodej Manager Remix domény a adresy URL** část, proveďte následující kroky:

    ![E prodej Manager Remix domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<Server-Based-URL>/<sub-domain>/`

    c. Kopírování **identifikátor** hodnotu v poznámkovém bloku. Identifikátor hodnotu použijete později v tomto kurzu.
    
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory E prodej Manager Remix klienta](mailto:esupport@softbrain.co.jp) k získání těchto hodnot.

4. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Vyberte **zobrazit a upravit všechny ostatní atributy uživatele** a klikněte na **emailaddress** atribut.
    
    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Kopírování **Namespace** a **název** hodnota deklarace identity z textové pole. Generovat hodnota v následující vzor - `<Namespace>/<Name>`. Tuto hodnotu použijete později v tomto kurzu.

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Na **E prodej Manager Remix konfigurace** klikněte na tlačítko **konfigurace Remix Manager E na prodejní** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Přihlásit se k vaší aplikaci E prodej Manager Remix jako správce.

10. Vyberte **do nabídky správce** v nabídce vpravo nahoře.

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Vyberte **nastavení systému**>**spolupráci s externím systému**

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Vyberte **SAML**

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. V **nastavení ověřování SAML** část, proveďte následující kroky:

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Vyberte **PC verze**
    
    b. Vyberte **e-mailu** z rozevíracího seznamu ve spolupráci položky oddílu.

    c. Do textového pole položky spolupráce, vložte **hodnoty deklarace identity** kterou jste zkopírovali z Azure jednofaktorovému portálu `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. V **vystavitele (entity ID)** textovému poli, Vložit **identifikátor** hodnotu, kterou jste zkopírovali z portálu Azure z **E prodej Manager Remix domény a adresy URL** části.

    e. Nahrát váš stažené **certifikát** z portálu Azure vyberte **souboru výběr**.

    f. V **ID zprostředkovatele přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** kterou jste zkopírovali z portálu Azure.

    g. V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte **Sign-Out URL** hodnotu, kterou jste zkopírovali z portálu Azure.

    h. Klikněte na tlačítko **nastavení dokončení**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Vytvoření zkušebního uživatele E prodej Manager Remix

1. Přihlásit se k vaší aplikaci E prodej Manager Remix jako správce.

2. Vyberte **do nabídky správce** v nabídce vpravo nahoře.

    ![Konfigurace Remix vedoucí prodeje E](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Vyberte **nastavení společnosti**>**údržby oddělení a zaměstnanci** a vyberte **zaměstnanci zaregistrovat**.

    ![Uživatel](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. V **nové registrace zaměstnanec** část, proveďte následující kroky:
    
    ![Uživatel](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. V **jméno zaměstnance** textovému poli, zadejte jméno uživatele, jako je Britta.

    b. Vyplní příslušné povinná pole s informacemi o uživateli.
    
    c. Pokud povolíte SAML, správce nebude moci přihlásit z přihlašovací obrazovky, takže udělte správci přihlášení oprávnění pro uživatele tak, že vyberete **přihlašovací jméno správce**

    d. Klikněte na tlačítko **registrace**

5. V budoucnu, pokud chcete k přihlášení jako správce, přihlaste se pomocí uživatele, který bylo uděleno oprávnění správce a klikněte na tlačítko **do nabídky správce** v nabídce vpravo nahoře.

    ![Uživatel](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k E prodej Manager Remix.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon E prodej Manager Remix, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **E prodej Manager Remix**.

    ![Odkaz E prodej Manager Remix v seznamu aplikací](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici E prodej Manager Remix na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci E prodej Manager Remix.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

