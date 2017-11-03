---
title: 'Kurz: Azure Active Directory integrace s ServiceNow | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ServiceNow a ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a91fab90a94b655b93c8ae9064ea4836b80d7678
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Kurz: Azure Active Directory integrace s ServiceNow
V tomto kurzu zjistěte, jak integrovat ServiceNow a ServiceNow Express s Azure Active Directory (Azure AD).

Integrace s Azure AD ServiceNow a ServiceNow Express poskytuje následující výhody:

* Můžete řídit ve službě Azure AD, kdo má přístup k ServiceNow a ServiceNow Express
* Můžete povolit uživatelům, aby automaticky získat přihlášení k ServiceNow a ServiceNow Express (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Ke konfiguraci integrace služby Azure AD s ServiceNow a ServiceNow Express, potřebujete následující položky:

* Předplatné služby Azure AD
* Pro ServiceNow, instanci nebo klienta ServiceNow Calgary verze nebo vyšší
* Pro instance ServiceNow Express, Helsinkách verze ServiceNow Express nebo vyšší
* Musí mít klienta ServiceNow [více jeden znak na modul Plugin poskytovatele](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) povolena. To lze provést [odesílá se žádost o službu](https://hi.service-now.com). 

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ServiceNow z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování pro ServiceNow a ServiceNow Express

## <a name="adding-servicenow-from-the-gallery"></a>Přidání ServiceNow z Galerie
Při konfiguraci integrace ServiceNow nebo ServiceNow Express do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS ServiceNow z galerie. 

**Pokud chcete přidat ServiceNow z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4]
6. Do vyhledávacího pole zadejte **ServiceNow**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. V podokně výsledků vyberte **ServiceNow**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s ServiceNow a ServiceNow Express podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ServiceNow je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ServiceNow musí navázat.
Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ServiceNow. Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ServiceNow, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování pro ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Konfigurace Azure AD jednotné přihlašování pro expresní ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  – Pokud chcete povolit uživatelům tuto funkci používat.
3. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření zkušebního uživatele ServiceNow](#creating-a-servicenow-test-user)**  – Pokud chcete mít protějšek Britta Simon v ServiceNow propojeném s Azure AD reprezentace jí.
5. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

> [!NOTE]
> Pokud chcete nakonfigurovat ServiceNow vynechat krok 2. Podobně pokud chcete nakonfigurovat ServiceNow Express vynechat krok 1.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurace Azure AD jednotné přihlášení pro ServiceNow
1. Na portálu Azure AD classic na **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")

2. Na **jak chcete uživatelům se přihlásit ServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")

3. Na **nakonfigurovat nastavení aplikace** proveďte následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")
   
    a. v **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace ServiceNow následující vzoru: `https://<instance-name>.service-now.com`.
   
    b. V **identifikátor** textové pole, zadejte adresu URL používá vaši uživatelé k přihlášení do aplikace ServiceNow následující vzoru: `https://<instance-name>.service-now.com`.
   
    c. Klikněte na **Další**

4. Pokud chcete, aby Azure AD automaticky konfigurovat ServiceNow pro ověřování na základě SAML, zadejte název instance ServiceNow, uživatelské jméno správce a heslo správce v **automaticky nakonfigurovat jednotné přihlašování** formuláři a klikněte na tlačítko *konfigurace*. Všimněte si, že zadané uživatelské jméno správce musí mít **security_admin** role přiřazené v ServiceNow pro tento postup. Jinak, jak ručně nakonfigurovat ServiceNow používat Azure AD jako poskytovatele identity SAML, klikněte na tlačítko **ruční konfigurace aplikace pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a proveďte následující kroky.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")

5. Na **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložit soubor certifikátu místně na vašem počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")

6. Přihlášení do aplikace ServiceNow jako správce.

7. Aktivovat *integrace - více jeden přihlašování instalační program zprostředkovatele* modulu plug-in podle následující další kroky:
   
    a. V navigačním podokně na levé straně přejděte na **definice systému** části a pak klikněte na **modulů plug-in**.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktivovat modulu plug-in")
   
    b. Vyhledejte *integrace - více jeden přihlašování instalační program zprostředkovatele*.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktivovat modulu plug-in")
   
    c. Vyberte modul plug-in. Rigth klikněte a vyberte **aktivovat nebo upgradovat**.
   
    d. Klikněte **aktivovat** tlačítko.

8. V navigačním podokně na levé straně klikněte na **vlastnosti**.  
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "konfigurovat adresu URL aplikace")

9. Na **více jednotného přihlašování k vlastnosti zprostředkovatele** dialogové okno, proveďte následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "konfigurovat adresu URL aplikace")
   
    a. Jako **povolit zprostředkovatel vícenásobného jednotného přihlašování k**, vyberte **Ano**.
   
    b. Jako **povolit protokolování ladění získali zprostředkovatel vícenásobného jednotného přihlašování k integraci**, vyberte **Ano**.
   
    c. V **pole na uživatele, který tabulky...**  textovému poli, typ **uživatelské_jméno**.
   
    d. Klikněte na **Uložit**.

10. V navigačním podokně na levé straně klikněte na **x509 certifikáty**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "nakonfigurovat jednotné přihlašování")

11. Na **certifikáty X.509** dialogové okno, klikněte na tlačítko **nový**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "nakonfigurovat jednotné přihlašování")

12. Na **certifikáty X.509** dialogové okno, proveďte následující kroky:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")
    
     a. Klikněte na možnost **Nové**.
    
     b. V **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).
    
     c. Vyberte **Active**.
    
     d. Jako **formátu**, vyberte **PEM**.
    
     e. Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.
    
     f. V poznámkovém bloku otevřete váš certifikát kódovaný v Base64, kopírovat obsah ho do schránky a vložte jej do **PEM certifikát** textové pole.
    
     g. Klikněte na tlačítko **aktualizace**.

13. V navigačním podokně na levé straně klikněte na **zprostředkovatelů Identity**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "nakonfigurovat jednotné přihlašování")

14. Na **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **nový**:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "nakonfigurovat jednotné přihlašování")

15. Na **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **aktualizaci1 typu SAML2?**:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "nakonfigurovat jednotné přihlašování")

16. V dialogovém okně Vlastnosti typu SAML2 aktualizaci1 proveďte následující kroky:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "nakonfigurovat jednotné přihlašování")

    a. v **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).

    b. V **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na pole, které slouží k jednoznačné identifikaci uživatele ve vašem nasazení ServiceNow. 

    > [!NOTE] 
    > Můžete configue Azure AD pro vydávání ID uživatele Azure AD (hlavní název uživatele) nebo e-mailovou adresu jako jedinečný identifikátor v tokenu SAML přechodem na **ServiceNow > atributy > jednotné přihlašování** části portálu Azure classic a mapování pro požadované pole **nameidentifier** atribut. Hodnota uložená vybraného atributu ve službě Azure AD (například hlavní název uživatele) se musí shodovat s hodnotou uloženou v ServiceNow pro zadané pole (například uživatelské_jméno)

    c. V klasickém portálu Azure AD, zkopírujte **ID zprostředkovatele Identity** hodnotu a vložte ji do **adresa URL poskytovatele Identity** textové pole.

    d. V klasickém portálu Azure AD, zkopírujte **adresa URL žádosti o ověření** hodnotu a vložte ji do **AuthnRequest zprostředkovatele Identity** textové pole.

    e. V klasickém portálu Azure AD, zkopírujte **jednu adresu URL služby Sign-Out** hodnotu a vložte ji do **SingleLogoutRequest zprostředkovatele Identity** textové pole.

    f. V **domovské stránky ServiceNow** textovému poli, zadejte adresu URL domovské stránce instance ServiceNow.

    > [!NOTE] 
    > Na domovské stránce instance ServiceNow je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např:`https://fabrikam.service-now.com/navpage.do`).

    g. V **Entity ID / vystavitele** textovému poli, zadejte adresu URL vašeho klienta ServiceNow.

    h. V **cílovou skupinu URL** textovému poli, zadejte adresu URL vašeho klienta ServiceNow. 

    i. V **protokol vazby pro rozšíření IDP na SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.

    j. Do textového pole NameID zásady zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.

    kB. Zrušte výběr **vytvořit AuthnContextClass**.

    l. V **AuthnContextClassRef metoda**, typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. To je potřeba jenom Pokud jste cloudu pouze organizace. Pokud používáte místní služby AD FS nebo vícefaktorového ověřování pro ověřování pak byste neměli konfigurovat tuto hodnotu. 

    m. V **hodin zkreslit** textovému poli, typ **60**.

    n. Jako **jednoho přihlášení na skriptu**, vyberte **MultiSSO_SAML2_Update1**.

    o. Jako **x509 certifikát**, vyberte certifikát, který jste vytvořili v předchozím kroku.

    p. Klikněte na tlačítko **odeslání**. 

1. Na portálu Azure AD classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Další**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")

2. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurace Azure AD jednotné přihlášení pro ServiceNow Express
1. Na portálu Azure AD classic na **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")

2. Na **jak chcete uživatelům se přihlásit ServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")

3. Na **nakonfigurovat nastavení aplikace** proveďte následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")
   
    a. v **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace ServiceNow následující vzoru: `https://<instance-name>.service-now.com`.
   
    b. V **URL vystavitele** textové pole, zadejte adresu URL používá vaši uživatelé k přihlášení do aplikace ServiceNow následující vzor `https://<instance-name>.service-now.com`.
   
    c. Klikněte na **Další**

4. Klikněte na tlačítko **ruční konfigurace aplikace pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a proveďte následující kroky.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")

5. Na **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložit soubor certifikátu místně na vašem počítači a pak klikněte na tlačítko **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")

6. Přihlášení do aplikace ServiceNow Express jako správce.

7. V navigačním podokně na levé straně klikněte na **jednotné přihlašování**.  
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "konfigurovat adresu URL aplikace")

8. Na **jednotné přihlašování** dialogové okno, klikněte na ikonu konfigurace v pravém horním rohu a nastavte následující vlastnosti:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "konfigurovat adresu URL aplikace")
   
    a. Přepnutí **povolit zprostředkovatel vícenásobného jednotného přihlašování k** vpravo.
   
    b. Přepnutí **ladění povolení protokolování pro více zprostředkovatele jednotného přihlašování k integraci** vpravo.
   
    c. V **pole na uživatele, který tabulky...**  textovému poli, typ **uživatelské_jméno**.
9. Na **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nový certifikát**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "nakonfigurovat jednotné přihlašování")
10. Na **certifikáty X.509** dialogové okno, proveďte následující kroky:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")
    
    a. V **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).
    
    b. Vyberte **Active**.
    
    c. Jako **formátu**, vyberte **PEM**.
    
    d. Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.
    
    e. Vytvořte soubor kódováním Base64 z stažený certifikát.
    
    > [!NOTE]
    > Další podrobnosti najdete v tématu [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. V poznámkovém bloku otevřete váš certifikát kódovaný v Base64, kopírovat obsah ho do schránky a vložte jej do **PEM certifikát** textové pole.
    
    g. Klikněte na tlačítko **aktualizace**.
11. Na **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nové rozšíření IdP**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "nakonfigurovat jednotné přihlašování")
12. Na **přidat nového poskytovatele Identity** dialogové okno v části **konfigurace zprostředkovatele Identity**, proveďte následující kroky:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "nakonfigurovat jednotné přihlašování")

    a. v **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).

    b. V klasickém portálu Azure AD, zkopírujte **ID zprostředkovatele Identity** hodnotu a vložte ji do **adresa URL poskytovatele Identity** textové pole.

    c. V klasickém portálu Azure AD, zkopírujte **adresa URL žádosti o ověření** hodnotu a vložte ji do **AuthnRequest zprostředkovatele Identity** textové pole.

    d. V klasickém portálu Azure AD, zkopírujte **jednu adresu URL služby Sign-Out** hodnotu a vložte ji do **SingleLogoutRequest zprostředkovatele Identity** textové pole.

    e. Jako **certifikát zprostředkovatele Identity**, vyberte certifikát, který jste vytvořili v předchozím kroku.


1. Klikněte na tlačítko **Upřesnit nastavení**a v části **další vlastnosti zprostředkovatele Identity**, proveďte následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "nakonfigurovat jednotné přihlašování")
   
    a. V **protokol vazby pro rozšíření IDP na SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.
   
    b. V **NameID zásad** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.    
   
    c. V **AuthnContextClassRef metoda**, typ **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Zrušte výběr **vytvořit AuthnContextClass**.

2. V části **další vlastnosti zprostředkovatele služby**, proveďte následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "nakonfigurovat jednotné přihlašování")
   
    a. V **domovské stránky ServiceNow** textovému poli, zadejte adresu URL domovské stránce instance ServiceNow.
   
    > [!NOTE]
    > Na domovské stránce instance ServiceNow je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. V **Entity ID / vystavitele** textovému poli, zadejte adresu URL vašeho klienta ServiceNow.
   
    c. V **identifikátor URI cílové skupiny** textovému poli, zadejte adresu URL vašeho klienta ServiceNow. 
   
    d. V **hodin zkreslit** textovému poli, typ **60**.
   
    e. V **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na pole, které slouží k jednoznačné identifikaci uživatele ve vašem nasazení ServiceNow.
   
    > [!NOTE]
    > Můžete configue Azure AD pro vydávání ID uživatele Azure AD (hlavní název uživatele) nebo e-mailovou adresu jako jedinečný identifikátor v tokenu SAML přechodem na **ServiceNow > atributy > jednotné přihlašování** části portálu Azure classic a mapování pro požadované pole **nameidentifier** atribut. Hodnota uložená vybraného atributu ve službě Azure AD (například hlavní název uživatele) se musí shodovat s hodnotou uloženou v ServiceNow pro zadané pole (například uživatelské_jméno)
    > 
    > 
   
    f. Klikněte na **Uložit**. 

3. Na portálu Azure AD classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Další**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")

4. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")

## <a name="configuring-user-provisioning"></a>Konfiguraci zřizování uživatelů
Cílem této části se popisují postup povolení zřizování uživatelů z uživatelských účtů služby Active Directory pro ServiceNow.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:
1. Na portálu správy Azure classic na **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů**. 
   
    ![Zřizování uživatelů](./media/active-directory-saas-servicenow-tutorial/IC769498.png "zřizování uživatelů")

2. Na **zadejte své přihlašovací údaje ServiceNow zapněte automatické uživatele zřizování** stránky, zadejte následující nastavení konfigurace:
   
     a. V **název Instance ServiceNow** textovému poli, zadejte název instance ServiceNow.
   
     b. V **uživatelské jméno správce ServiceNow** textovému poli, zadejte název účtu správce ServiceNow.
   
     c. V **heslo správce ServiceNow** textovému poli, zadejte heslo pro tento účet.
   
     d. Klikněte na tlačítko **ověření** k ověření vaší konfigurace.
   
     e. Klikněte **Další** tlačítko Otevřít **další kroky** stránky.
   
     f. Pokud chcete zřídit všichni uživatelé k této aplikaci, vyberte možnost "**automaticky zřizovat všechny uživatelské účty v adresáři do této aplikace**". 
   
    ![Další kroky](./media/active-directory-saas-servicenow-tutorial/IC698804.png "další kroky")
   
     g. Na **další kroky** klikněte na tlačítko **Complete** uložte konfiguraci.

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
V této části vytvoříte testovacího uživatele na portálu classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.

6. Na **profil uživatele** dialogové okno proveďte následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. V **křestní jméno** textovému poli, typ **Britta**.  
   
   b. V **příjmení** textovému poli, typ, **Simon**.
   
   c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
   d. V **Role** seznamu, vyberte **uživatele**.
   
   e. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Poznamenejte si hodnotu **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-a-servicenow-test-user"></a>Vytvoření zkušebního uživatele ServiceNow
V této části vytvoříte uživatele volal Britta Simon v ServiceNow. V této části vytvoříte uživatele volal Britta Simon v ServiceNow. Pokud nevíte jak přidat uživatele ve vašem účtu ServiceNow a ServiceNow Express, kontaktujte tým podpory ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD
V této části povolíte Britta Simon používat tak, že udělíte přístup k ServiceNow Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon ServiceNow, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **ServiceNow**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 

4. V seznamu všechny uživatele, vyberte **Britta Simon**.

5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.

Když kliknete na dlaždici ServiceNow na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ServiceNow.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
