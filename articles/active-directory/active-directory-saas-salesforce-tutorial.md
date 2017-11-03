---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Kurz: Azure Active Directory integrace s Salesforce

V tomto kurzu zjistěte, jak integrovat Salesforce s Azure Active Directory (Azure AD).

Integrace služby Salesforce s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup do služby Salesforce
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Salesforce (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD pomocí služby Salesforce, potřebujete následující položky:

- Předplatné služby Azure AD
- Služby Salesforce jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Salesforce z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-salesforce-from-the-gallery"></a>Přidání Salesforce z Galerie
Při konfiguraci integrace služby Salesforce do služby Azure AD potřebujete přidat Salesforce z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Salesforce z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Salesforce**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Na panelu výsledků vyberte **Salesforce**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Salesforce je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Salesforce musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Salesforce.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí služby Salesforce, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Salesforce](#creating-a-salesforce-test-user)**  – Pokud chcete mít protějšek Britta Simon v Salesforce, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce.

**Ke konfiguraci Azure AD jednotné přihlašování pomocí služby Salesforce, proveďte následující kroky:**

1. Na portálu Azure na **Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Na **Salesforce domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce: 
   * Účet organizace:`https://<subdomain>.my.salesforce.com`
   * Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou reálné. Tyto hodnoty aktualizujte s skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce klienta](https://help.salesforce.com/support) k získání těchto hodnot. 
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Na **Salesforce konfigurace** klikněte na tlačítko **konfigurace Salesforce** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.** 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  V prohlížeči a přihlaste se ke svému účtu Salesforce správce otevřete novou kartu.

8.  V části **správce** navigačním podokně klikněte na tlačítko **ovládacích prvků zabezpečení** rozbalte související část. Pak klikněte na tlačítko **nastavení jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Na **nastavení jednotného přihlašování** klikněte na tlačítko **upravit** tlačítko.
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Pokud nelze povolit nastavení jednotného přihlašování pro váš účet Salesforce, budete možná muset kontaktovat [tým podpory služby Salesforce klienta](https://help.salesforce.com/support). 

10. Vyberte **povoleno SAML**a potom klikněte na **Uložit**.

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. Chcete-li nakonfigurovat SAML jeden přihlašování nastavení, klikněte na tlačítko **nový**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Na **SAML jeden přihlašování nastavení upravit** stránky, proveďte následující konfigurace:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Pro **název** pole, zadejte popisný název pro tuto konfiguraci. Poskytuje hodnotu pro **název** automaticky vyplnit **název rozhraní API** textové pole.

    b. Vložení **SMAL Entity ID** hodnoty do **vystavitele** pole v Salesforce.

    c. V **textové pole Entity Id**, zadejte název domény vaší služby Salesforce pomocí následujícího vzorce:
      
      * Účet organizace:`https://<subdomain>.my.salesforce.com`
      * Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Klikněte na tlačítko **Procházet** nebo **zvolit soubor** otevřete **vybrat soubor k odeslání** dialogovém okně, vyberte svůj certifikát Salesforce a pak klikněte na tlačítko **otevřete** na kterou odešlete certifikát.

    e. Pro **typ Identity SAML**, vyberte **kontrolní výraz obsahuje uživatelské jméno uživatele salesforce.com**.

    f. Pro **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier příkaz subjektu**

    g. Vložení **jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení zprostředkovatele Identity** pole v Salesforce.
    
    h. Pro **zprostředkovatele iniciované žádosti vazby služby**, vyberte **přesměrování protokolu HTTP**.
    
    i. Nakonec klikněte na **Uložit** použít SAML jeden přihlašování nastavení.

13. V levém navigačním podokně v Salesforce, klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Přejděte dolů k položce **konfiguraci ověřování** části a klikněte na tlačítko **upravit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. V **ověřovací služby** část, vyberte popisný název konfigurace jednotného přihlašování SAML a pak klikněte na tlačítko **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Pokud je vybrána více než jedna služba ověřování, jsou uživatelé vyzváni k vyberte služby ověřování, která se má při inicializaci jednotné přihlašování pro vaše prostředí Salesforce přihlásit. Pokud nechcete, aby mohla nastat, pak byste měli **nechte nezaškrtnuté všechny ostatní služby ověřování**.
<CE>    
> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V levém navigačním podokně v **portál Azure**, klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-salesforce-test-user"></a>Vytvoření zkušebního uživatele Salesforce

V této části se uživatel volá Britta Simon vytvoří v Salesforce. Salesforce podporuje za běhu zřizování, který je ve výchozím nastavení povolené.
Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v Salesforce ještě neexistuje, vytvoří se nový při pokusu o přístup k Salesforce.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup do služby Salesforce.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Salesforce, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Salesforce**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

K testování vaše nastavení jednotného přihlašování, otevřete Panel přístupu na [https://myapps.microsoft.com](https://myapps.microsoft.com/), pak se přihlaste do testovací účet a klikněte na tlačítko **Salesforce**.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

