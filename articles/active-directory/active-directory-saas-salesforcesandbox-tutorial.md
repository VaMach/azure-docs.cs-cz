---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce

V tomto kurzu zjistěte, jak integrovat Salesforce izolovaného prostoru se službou Azure Active Directory (Azure AD).

Integrace služby Salesforce izolovaného prostoru s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k izolovanému prostoru Salesforce
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Salesforce izolovaného prostoru (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Salesforce izolovaného prostoru, potřebujete následující položky:

- Předplatné služby Azure AD
- Izolovaného prostoru Salesforce jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání izolovaného prostoru Salesforce z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Přidání izolovaného prostoru Salesforce z Galerie
Při konfiguraci integrace služby Salesforce izolovaného prostoru do služby Azure AD potřebujete přidat izolovaného prostoru Salesforce z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat izolovaného prostoru Salesforce z galerie, postupujte takto:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **izolovaného prostoru Salesforce**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. Na panelu výsledků vyberte **izolovaného prostoru Salesforce**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurujete a testu Azure AD jednotné přihlašování s izolovaného prostoru Salesforce podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Salesforce izolovaného prostoru je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v izolovaném prostoru Salesforce musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v izolovaném prostoru služby Salesforce.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele izolovaného prostoru Salesforce](#creating-a-salesforce-sandbox-test-user)**  – Pokud chcete mít protějšek Britta Simon v Salesforce izolovaný prostor, který je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce izolovaného prostoru.

**Ke konfiguraci Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, proveďte následující kroky:**

1. Na portálu Azure na **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Na **Salesforce izolovaného prostoru domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Tato hodnota není reálné. Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce izolovaného prostoru klienta](https://help.salesforce.com/support) získat tuto hodnotu.


4. Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je rovněž nutné nakonfigurovat **identifikátor** do mají stejnou hodnotu jako **přihlásit na adrese URL**. 
    
    >[!Note]
    >**Identifikátor** pole naleznete kontrolou **zobrazit upřesňující nastavení** zaškrtávací políčko je na **konfigurace adresy URL aplikace** stránky dialogového okna 


5. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. Na **Salesforce izolovaného prostoru konfigurace** klikněte na tlačítko **konfigurace izolovaného prostoru Salesforce** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. V prohlížeči otevřete novou kartu a přihlaste se k účtu správce izolovaného prostoru služby Salesforce.

9. V nabídce v horní části, klikněte na tlačítko **instalační program**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. V navigačním podokně na levé straně klikněte na **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. V části Nastavení jednotného přihlašování, proveďte následující kroky: ![nakonfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Vyberte **povoleno SAML**. 

     b.  Klikněte na možnost **Nové**.

12. V části SAML jeden přihlašování nastavení proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In textového pole Název zadejte název konfigurace (např: *SPSSOWAAD\_Test*). 

    b. Vložení **SMAL Entity ID** hodnoty do **vystavitele** textové pole.

    c. V **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud je první instance Salesforce izolovaného prostoru, který chcete přidat do vašeho adresáře. Pokud jste již přidali instance Salesforce izolovaného prostoru, pak pro **Entity ID** zadejte **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`  
 
    d. Klikněte na tlačítko **Procházet** nahrát stažený certifikát.  

    e. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje ID federace z objektu uživatele**.
 
    f. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier příkaz subjektu**.

    g. Vložení **jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole. 

    h. SFDC nepodporuje SAML odhlášení.  Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' ji do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.

    i. Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**. 

    j. Klikněte na **Uložit**.

### <a name="enable-your-domain"></a>Povolit doménu
V této části se předpokládá, že jste již vytvořili domény.  Další informace najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Pokud chcete povolit doménu, proveďte následující kroky:**

1. V levém navigačním podokně klikněte na tlačítko **Správa domén**a pak klikněte na tlačítko **Moje domény.**
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Přesvědčte se, že doménu je správně nakonfigurováno. 

2. V **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název SAML jeden přihlašování nastavení z předchozí části a nakonec klikněte na tlačítko **Uložit**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Jakmile máte doménu nakonfigurovat, uživatelé měli používat adresa URL domény k přihlášení k izolovanému prostoru služby Salesforce.  

Chcete-li získat hodnotu adresy URL, klikněte na tlačítko jednotného přihlašování k profilu, který jste vytvořili v předchozí části.    

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Vytvoření zkušebního uživatele izolovaného prostoru Salesforce

V této části se uživatel volá Britta Simon vytvoří v izolovaného prostoru služby Salesforce. Izolovaný prostor Salesforce podporuje zřizování za běhu, který je ve výchozím nastavení povolené.
Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v izolovaném prostoru Salesforce ještě neexistuje, vytvoří se nový při pokusu o přístup k izolovanému prostoru služby Salesforce.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k izolovanému prostoru služby Salesforce.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Salesforce izolovaného prostoru, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **izolovaného prostoru Salesforce**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

