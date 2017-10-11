---
title: 'Kurz: Azure Active Directory integraci s Workday | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a během pracovního dne."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 164d5c644f120fa86e2b690649241892764b64b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Kurz: Azure Active Directory integraci s Workday

V tomto kurzu zjistěte, jak integrovat Workday s Azure Active Directory (Azure AD).

Integrace Workday s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Workday
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Workday (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Chcete-li konfigurovat Azure AD integraci s Workday, potřebujete následující položky:

- Předplatné služby Azure AD
- Workday jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Workday z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-workday-from-the-gallery"></a>Přidání Workday z Galerie
Konfigurace integrace Workday do Azure AD, potřebujete přidat Workday z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Workday z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Workday**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Na panelu výsledků vyberte **Workday**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workday je pro uživatele ve službě Azure AD. Jinými slovy musí odkaz vztah mezi uživatele Azure AD a související uživatelské ve Workday zřídit.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** ve Workday.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Workday](#creating-a-workday-test-user)**  – Pokud chcete mít protějšek Britta Simon ve Workday propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci během pracovního dne.

**Ke konfiguraci Azure AD jednotné přihlašování s Workday, proveďte následující kroky:**

1. Na portálu Azure na **Workday** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Na **Workday domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Tyto hodnoty nejsou reálné. Tyto hodnoty aktualizujte skutečná adresa URL přihlašování a adresa URL odpovědi. Vaše adresa URL odpovědi musí mít například subdoména: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Pomocí něco podobného jako "*http://www.myworkday.com*" funguje, ale "*http://myworkday.com*" neexistuje. Obraťte se na [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) k získání těchto hodnot. 
 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. Na **Workday konfigurace** klikněte na tlačítko **konfigurace Workday** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Workday jako správce.

8. Přejděte na **nabídky \> Workbench**.
   
    ![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

9. Přejděte na **účet správy**.
   
    ![Účet správy](./media/active-directory-saas-workday-tutorial/IC782924.png "účet správy")

10. Přejděte na **upravit nastavení klienta – zabezpečení**.
   
    ![Upravit zabezpečení klienta](./media/active-directory-saas-workday-tutorial/IC782925.png "upravit klienta zabezpečení")

11. V **adresy URL pro přesměrování** část, proveďte následující kroky:
   
    ![Adresy URL pro přesměrování](./media/active-directory-saas-workday-tutorial/IC7829581.png "adresy URL pro přesměrování")
   
    a. Klikněte na tlačítko **přidejte řádek**.
   
    b. V **adresy URL přesměrování při přihlášení** textové pole a **adresu URL pro přesměrování mobilní** textovému poli, typ **přihlašovací adresa URL** jste zadali na **Workday domény a adresy URL** části portálu Azure.
   
    c. Na portálu Azure na **konfigurovat přihlášení** okno, kopie **Sign-Out URL**a vložte ji do **adresy URL přesměrování odhlašovací** textové pole.
   
    d.  V **prostředí** textovému poli, zadejte název prostředí.  

    >[!NOTE]
    > Hodnota atributu prostředí je vázaný na hodnotu adresy URL klienta:  
    >-Pokud název domény adresy URL klienta Workday začíná impl například: *https://impl.workday.com/\<klienta\>/login-saml2.htmld*), **prostředí** musí být nastaven pro implementaci.  
    >– Pokud je název domény začíná něco jiného, budete muset kontaktovat [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) získat shody **prostředí** hodnotu.

12. V **SAML instalace** část, proveďte následující kroky:
   
    ![Instalační program SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "instalace SAML")
   
    a.  Vyberte **povolit ověřování SAML**.
   
    b.  Klikněte na tlačítko **přidejte řádek**.

13. V části zprostředkovatelů Identity SAML proveďte následující kroky:
   
    ![Zprostředkovatelé Identity SAML](./media/active-directory-saas-workday-tutorial/IC7829271.png "zprostředkovatelů Identity SAML")
   
    a. Do textového pole Název zprostředkovatele Identity, zadejte název zprostředkovatele (například: *SPInitiatedSSO*).
   
    b. Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML Entity ID** hodnotu a vložte ji do **vystavitele** textové pole.
   
    c. Vyberte **povolit Workday Initiated odhlášení**.
   
    d. Na portálu Azure na **konfigurovat přihlášení** okno, kopie **Sign-Out URL** hodnotu a vložte ji do **adresy URL žádosti o odhlášení** textové pole.

    e. Klikněte na tlačítko **certifikátu veřejného klíče zprostředkovatele Identity**a potom klikněte na **vytvořit**. 

    ![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782928.png "vytvořit")

    f. Klikněte na tlačítko **vytvořit x509 veřejný klíč**. 

    ![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782929.png "vytvořit")


14. V **veřejný klíč zobrazení x509** část, proveďte následující kroky: 
   
    ![Veřejný klíč zobrazení x509](./media/active-directory-saas-workday-tutorial/IC782930.png "zobrazení x509 veřejný klíč") 
   
    a. V **název** textovému poli, zadejte název pro svůj certifikát (například: *PPE\_SP*).
   
    b. V **platný z** textovému poli, zadejte platný z hodnota atributu vašeho certifikátu.
   
    c.  V **platný k** textovému poli, zadejte platnou hodnotu atributu vašeho certifikátu.
   
    > [!NOTE]
    > Můžete získat od data a platném datum, od stažený certifikát poklepáním platný.  Kalendářní data jsou uvedeny v seznamu **podrobnosti** kartě.
    > 
    >
   
    d.  Otevření kódovaného certifikátu kódování base-64 v programu Poznámkový blok a zkopírujte obsah ho.
   
    e.  V **certifikát** textovému poli, vložit obsah schránky.
   
    f.  Klikněte na **OK**.

15. Proveďte následující kroky: 
   
    ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "Konfigurace jednotného přihlašování")
   
    a.  Povolit **x509 pár privátního klíče**.
   
    b.  V **ID zprostředkovatele služby** textovému poli, typ **http://www.workday.com**.
   
    c.  Vyberte **povolit SP spustil ověřování SAML**.
   
    d.  Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **IdP adresa URL služby jednotného přihlašování k** textové pole.
   
    e. Vyberte **není Deflate žádosti o ověření spouštěná SP**.
   
    f. Jako **metoda žádosti o podpis ověřování**, vyberte **SHA256**. 
   
    ![Metoda ověřování žádost o podpis](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "metodu ověřování žádost o podpis") 
   
    g. Klikněte na **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-workday-test-user"></a>Vytvoření zkušebního uživatele Workday

Pokud chcete získat testovacího uživatele zřízené do Workday, budete muset kontaktovat [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html).
[Tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) pro vás vytvoří uživatele.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k Workday.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Workday, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Workday**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

