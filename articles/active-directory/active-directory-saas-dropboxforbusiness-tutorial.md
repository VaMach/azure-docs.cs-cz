---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Kurz: Azure Active Directory integrace s Dropbox pro firmy

V tomto kurzu zjistěte, jak integrovat Dropbox pro firmy s Azure Active Directory (Azure AD).

Integrace Dropbox pro firmy s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Dropboxu pro firmy
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Dropboxu pro firmy (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Dropbox pro firmy, potřebujete následující položky:

- Předplatné služby Azure AD
- Dropbox pro obchodní jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Dropbox pro firmy z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Přidání Dropbox pro firmy z Galerie
Pokud chcete nakonfigurovat integraci Dropbox pro firmy do služby Azure AD, potřebujete přidat Dropbox pro firmy z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Dropbox pro firmy z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Dropbox pro firmy**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Na panelu výsledků vyberte **Dropbox pro firmy**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy na základě testovací uživatele, nazývá "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Dropbox pro firmy je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Dropbox pro firmy.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Dropbox pro firmy.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy, musíte dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Dropbox pro obchodní testovacího uživatele](#creating-a-dropbox-for-business-test-user)**  – Pokud chcete mít protějšek Britta Simon v Dropbox pro podnikání, které je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování do vaší schránky pro obchodní aplikace.

**Ke konfiguraci Azure AD jednotné přihlašování s Dropbox pro firmy, proveďte následující kroky:**

1. Na portálu Azure na **Dropbox pro firmy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Na **Dropbox obchodní domény a adresy URL** část, proveďte následující kroky:

    a. Přihlásit se do vaší schránky pro obchodní klienta. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "nakonfigurovat jednotné přihlašování")
   
    b. V navigačním podokně na levé straně klikněte na **konzoly pro správu**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "nakonfigurovat jednotné přihlašování")
   
    c. Na **konzoly pro správu**, klikněte na tlačítko **ověřování** v levém navigačním podokně. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "nakonfigurovat jednotné přihlašování")
   
    d. V **jednotného přihlašování** vyberte **povolit jednotné přihlašování**a potom klikněte na **Další** rozbalte v této části.  
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "nakonfigurovat jednotné přihlašování")
   
    e. Zkopírujte adresu URL do **uživatelé se mohou přihlásit zadáním e-mailové adresy nebo můžete přejít přímo na**. 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. Na portálu Azure v **přihlašovací adresa URL** textovému poli, vložte adresu URL.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Tato hodnota není skutečné hodnoty. Aktualizujte hodnotu adresou URL skutečné přihlášení můžete získat z jejich části jednom přihlášení. Obraťte se na [Dropbox pro klienta obchodní tým podpory](https://www.dropbox.com/business/contact) získat tuto hodnotu. 
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Na **Dropbox pro konfiguraci obchodní** klikněte na tlačítko **Dropbox nakonfigurovat pro firmy** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Konfigurace jednotného přihlašování na **Dropbox pro firmy** straně, přejděte na vaší schránky pro obchodní klienta v **jednotného přihlašování** části **ověřování** proveďte následující kroky: 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "nakonfigurovat jednotné přihlašování")
   
    a. Klikněte na tlačítko **požadované**.
   
    b. Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **přihlašovací adresa URL** textové pole.

    c. Klikněte na tlačítko **vyberte certifikát**a pak přejděte do vaší **Base64 kódovaného certifikátu souboru**.

    d. Klikněte na tlačítko **uložit změny** k dokončení konfigurace na vaší schránky pro obchodní klienta.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>Vytváření Dropbox pro obchodní testovacího uživatele

V této části se uživatel volá Britta Simon vytvoří v Dropbox pro firmy. Dropbox pro firmy podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v Dropbox pro firmy ještě neexistuje, vytvoří se nový při pokusu o přístup k Dropboxu pro firmy.

>[!Note]
>Pokud je potřeba ručně vytvořit uživatele, obraťte se na [Dropbox pro tým podpory obchodní klienta](https://www.dropbox.com/business/contact) 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Dropbox pro firmy.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Dropbox pro firmy, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Dropbox pro firmy**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Po kliknutí na tlačítko Dropbox pro obchodní dlaždici na přístupovém panelu, měli byste obdržet přihlašovací stránku vaší schránky pro obchodní aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

