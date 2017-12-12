---
title: 'Kurz: Azure Active Directory integrace s Boomi | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: f63f317cc213519c50b0f07b4f93842a72500a82
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Kurz: Azure Active Directory integrace s Boomi

V tomto kurzu zjistěte, jak integrovat Boomi s Azure Active Directory (Azure AD).

Integrace Boomi s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Boomi
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Boomi (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Boomi, potřebujete následující položky:

- Předplatné služby Azure AD
- Boomi jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Boomi z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-boomi-from-the-gallery"></a>Přidání Boomi z Galerie
Při konfiguraci integrace Boomi do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Boomi z galerie.

**Pokud chcete přidat Boomi z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Boomi**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Na panelu výsledků vyberte **Boomi**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Boomi je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Boomi musí navázat.

V Boomi, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Boomi](#creating-a-boomi-test-user)**  – Pokud chcete mít protějšek Britta Simon v Boomi propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Boomi.

**Ke konfiguraci Azure AD jednotné přihlašování s Boomi, proveďte následující kroky:**

1. Na portálu Azure na **Boomi** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. Na **Boomi domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://platform.boomi.com/sso/<accountname>/saml`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi. Obraťte se na [tým podpory Boomi](https://boomi.com/company/contact/) k získání těchto hodnot.

4. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Aplikace Boomi očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. V **uživatelské atributy** části na **jednotného přihlašování** dialogu pro každý řádek v tabulce níže, proveďte následující kroky:

    | Název atributu | Hodnota atributu |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.
    
    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.
    
    d. Klikněte na tlačítko **OK**.

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. Na **Boomi konfigurace** klikněte na tlačítko **konfigurace Boomi** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Boomi. 

9. Přejděte na **název společnosti** a přejděte na **nastavit**.

10. Klikněte **možnosti jednotného přihlašování k** kartě a proveďte následující kroky.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Zkontrolujte **povolit SAML jednotné přihlašování** zaškrtávací políčko.

    b. Klikněte na tlačítko **Import** na kterou odešlete certifikát stažený z Azure AD **certifikát zprostředkovatele Identity**.
    
    c. V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.

    d. Jako **Federation Id umístění**, vyberte **federace je v elementu FEDERATION_ID atribut** přepínač. 

    e. Klikněte na tlačítko **Uložit** tlačítko.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-boomi-test-user"></a>Vytvoření zkušebního uživatele Boomi

Pokud chcete povolit uživatelům Azure AD přihlášení k Boomi, musí být zřízená do Boomi. V případě Boomi zřizování je ruční úloha.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>K poskytnutí uživatelského účtu, proveďte následující kroky:

1. Přihlaste se k serveru vaší společnosti Boomi jako správce.

2. Po přihlášení, přejděte na **Správa uživatelů** a přejděte na **uživatelé**.

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "uživatelů")

3. Klikněte na tlačítko  **+**  ikonu a **role uživatele přidat/zachování** otevře se dialogové okno.

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "uživatelů")

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "uživatelů")

    a. V **e-mailová adresa uživatele** jako typ e-mailu uživatele k textovému poli, BrittaSimon@contoso.com.
    
    b. V **křestní jméno** textovému poli, název typu první uživatele jako Britta.

    c. V **příjmení** textovému poli, zadejte příjmení uživatele jako Simon.
    
    d. Zadejte uživatele **federace**. Každý uživatel musí mít ID federace, která jednoznačně identifikuje uživatele v rámci účtu.
    
    e. Přiřazení **standardní uživatel** role pro uživatele. Vzhledem k tomu, který by mu udělit normální prostředí přístup, jakož i přístup přihlašování není přiřadit role správce.
    
    f. Klikněte na **OK**.
    
    > [!NOTE]
    > Uživatel nebude přijímat úvodní oznamovací e-mail obsahující heslo, které lze použít k přihlášení k účtu AtomSphere, protože heslo je spravován pomocí zprostředkovatele identity. Může použít jakékoli jiné Boomi uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Boomi zřídit AAD uživatelské účty. 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Boomi.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Boomi, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Boomi**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Boomi na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Boomi.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

