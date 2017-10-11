---
title: 'Kurz: Azure Active Directory integrace s Cezanne HR softwarem | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Cezanne HR softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Kurz: Integrate Azure Active Directory s Cezanne HR softwaru

V tomto kurzu zjistěte, jak integrovat Cezanne HR softwaru s Azure Active Directory (Azure AD).

Integrace Cezanne HR softwaru s Azure AD poskytuje následující výhody. Můžete:

- Řízení ve službě Azure AD, který má přístup k softwaru Cezanne oddělení lidských zdrojů.
- Povolte uživatelům automaticky přihlásit k Cezanne HR software s jednotné přihlašování (SSO) s jejich účty Azure AD.
- Spravovat účty v jednom centrálním místě: portál Azure.

Další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Cezanne HR softwaru, potřebujete následující položky:

- Předplatné služby Azure AD
- Software Cezanne HR předplatné povolené jednotné přihlašování

> [!NOTE]
> Chcete-li otestovat kroky v tomto kurzu, doporučujeme nepoužívejte provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte produkční prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí. 

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

* Přidání softwaru Cezanne HR z Galerie
* Konfigurace a testování Azure AD SSO

## <a name="add-cezanne-hr-software-from-the-gallery"></a>Přidat software Cezanne HR z Galerie
Při konfiguraci integrace Cezanne HR softwaru do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS Cezanne HR software z galerie.

Pokud chcete přidat Cezanne HR softwaru z galerie, postupujte takto:

1. V  **[portál Azure](https://portal.azure.com)**, v levém podokně, vyberte **Azure Active Directory** tlačítko. 

    ![Tlačítko "Azure Active Directory"][1]

2. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![Na odkaz "Všechny aplikace"][2]
    
3. Chcete-li přidat novou aplikaci, v horní části **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.

    !["Nová aplikace" tlačítko][3]

4. Do vyhledávacího pole zadejte **Cezanne HR softwaru**.

    ![Do vyhledávacího pole](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. V seznamu výsledků vyberte **Cezanne HR softwaru** a pak vyberte **přidat** tlačítko Přidat aplikaci.

    ![Seznam výsledků](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat jednotného přihlašování k AD Azure s Cezanne HR software založený na testovacího uživatele názvem "Britta Simon."

Pro jednotné přihlašování pro práci Azure AD musí znát protějškem softwaru Cezanne HR uživatele Azure AD. Jinými slovy je potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské v oddělení lidských zdrojů Cezanne softwaru.

K navázání vztahu odkaz, přiřazení softwaru Cezanne HR **uživatelské jméno** hodnotu jako Azure AD **uživatelské jméno** hodnotu.

Nakonfigurovat a otestovat Azure AD SSO pomocí softwaru Cezanne oddělení lidských zdrojů, dokončete následující stavební bloky.

### <a name="configure-azure-ad-sso"></a>Konfigurovat Azure AD jednotného přihlašování

V této části můžete povolení jednotného přihlašování Azure AD na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cezanne HR softwaru následujícím způsobem:

1. Na portálu Azure na **Cezanne HR softwaru** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Příkaz "Jednotného přihlašování"][4]

2. Pro povolení jednotného přihlašování, v **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování**.
 
    ![Do pole "Režim"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. V části **Cezanne HR softwaru domény a adresy URL**, postupujte takto:

    ![V části "Cezanne HR softwaru domény adresy URL a"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. V **přihlašovací adresa URL** zadejte adresu URL, která má následující syntaxi:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. V **adresa URL odpovědi** zadejte adresu URL, která má následující syntaxi:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > Předchozí hodnoty nejsou skutečné. Adresa URL skutečné odpovědi a adresa URL přihlašování je aktualizujte. Chcete-li získat hodnoty, obraťte se [tým podpory klientský software Cezanne HR](mailto:info@cezannehr.com).

4. V části **SAML podpisový certifikát**, vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu v počítači.

    ![V části "SAML podpisový certifikát"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Vyberte **Uložit**.

    ![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. V části **Cezanne HR softwarové konfigurace**, vyberte **konfigurace softwaru HR Cezanne** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID** a **SAML-služby přihlášení** adresa URL z **Stručná referenční příručka** části.

    ![V části "Konfigurace softwaru HR Cezanne"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se ke klientovi Cezanne HR softwaru jako správce.

8. V levém podokně vyberte **nastavení systému**. Vyberte **nastavení zabezpečení** > **jednotné přihlašování konfigurace**.

    ![Na odkaz "Jednoho přihlášení konfigurace"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. V **umožňují uživatelům přihlásit pomocí následujících služeb jednotné přihlašování (SSO)** podokně, vyberte **SAML 2.0** zaškrtávací políčko a vyberte **Upřesnit konfiguraci** možnost.

    ![Jednotné přihlašování možnosti služeb](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Vyberte **přidat nové**.

    ![Tlačítko "Přidat nové"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. V části **zprostředkovatelů Identity SAML 2.0**, postupujte takto:

    ![V části "Zprostředkovatelů Identity SAML 2.0"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. V **zobrazovaný název** pole, zadejte název zprostředkovatele identity.

    b. V **identifikátor Entity** pole, vložte **SAML Entity ID** který jste zkopírovali z portálu Azure. 

    c. V **SAML vazby** pole se seznamem, vyberte **POST**.

    d. V **koncový bod služby tokenu zabezpečení** pole, vložte **SAML-služby přihlášení** adresu URL, kterou jste zkopírovali z portálu Azure. 
    
    e. V **název atributu ID uživatele** zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Chcete-li nahrát na server certifikát stažený z Azure AD, vyberte **nahrát** tlačítko.
    
    g. Vyberte **OK**. 

12. Vyberte **Uložit**.

    ![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Jak nastavit aplikaci si můžete přečíst stručným verzi podle předchozích pokynů v [portál Azure](https://portal.azure.com). Po přidání aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotného přihlašování** kartě. Přejděte k embedded dokumentace z **konfigurace** části. 

Další informace o funkci embedded dokumentaci najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele Britta Simon na portálu Azure.

![Britta Simon testovacího uživatele][100]

Vytvoření zkušebního uživatele ve službě Azure AD, postupujte takto:

1. V **portál Azure**, v levém podokně, vyberte **Azure Active Directory** tlačítko.

    ![Tlačítko "Azure Active Directory"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, vyberte **uživatelů a skupin** > **všichni uživatelé**.
    
    ![Na odkaz "Všichni uživatelé"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    **Všichni uživatelé** otevře se dialogové okno.

3. Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat**.
 
    ![Tlačítko "Přidat"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. V **uživatele** dialogové okno pole, postupujte takto:
 
    ![Dialogové okno "User"](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** pole, zadejte uživatele Britta Simon **e-mailová adresa**.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a poznamenejte si hodnotu, která byla vygenerována v **heslo** pole.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Vytvoření zkušebního uživatele Cezanne HR softwaru

Povolit uživatelům Azure AD přihlášení k Cezanne HR softwaru, musí být zřízená do Cezanne HR softwaru. V případě Cezanne HR softwaru zřizování je ruční úloha.

Poskytnutí uživatelského účtu následujícím způsobem:

1.  Přihlaste se k serveru vaší společnosti softwaru Cezanne HR jako správce.

2.  V levém podokně vyberte **nastavení systému** > **spravovat uživatele** > **přidat nové uživatele**.

    ![Na odkaz "Přidat nový uživatel"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nového uživatele")

3.  V části **osoba podrobnosti**, postupujte takto:

    ![V části "Osoba podrobnosti"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nového uživatele")
    
    a. Nastavit **interní uživatele** jako **OFF**.
    
    b. V **křestní jméno** zadejte jméno uživatele, například **Britta**.  
 
    c. V **příjmení** zadejte příjmení uživatele, například **Simon**.
    
    d. V **e-mailu** zadejte e-mailovou adresu uživatele, například Brittasimon@contoso.com.

4.  V části **informace o účtu**, postupujte takto:

    ![V části "Informace o účtu"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nového uživatele")
    
    a. V **uživatelské jméno** zadejte e-mailovou adresu uživatele, například Brittasimon@contoso.com.
    
    b. V **heslo** zadejte heslo uživatele.
    
    c. V **Role zabezpečení** vyberte **HR Professional**.
    
    d. Vyberte **OK**.

5. Na **jednotného přihlašování** ve **SAML 2.0 identifikátory** vyberte **přidat nové**.

    ![Tlačítko "Přidat nové"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "uživatele")

6. V **zprostředkovatele Identity** vyberte zprostředkovatele identity. V **uživatelský identifikátor** zadejte e-mailovou adresu pro testovací uživatele Britta Simon na účet.

    ![Do polí "Zprostředkovatele Identity" a "Uživatelský identifikátor"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "uživatele")
    
7. Vyberte **Uložit**.

    ![Tlačítko "Uložit"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "uživatele")

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte testovacího uživatele Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k softwaru Cezanne oddělení lidských zdrojů.

![Test přístupu uživatele][200] 

1. Na portálu Azure otevřete zobrazení aplikace a pak přejděte do zobrazení adresáře. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![Na odkaz "Všechny aplikace"][201] 

2. V seznamu aplikací vyberte **Cezanne HR softwaru**.

    ![Seznam "Aplikací"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. V nabídce na levé straně vyberte **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Vyberte **Přidat**. Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    !["Uživatelé a skupiny" odkaz][203]

5. V **uživatelů a skupin** v dialogovém **uživatelé** seznamu, vyberte **Britta Simon**.

6. V **uživatelů a skupin** dialogové okno, vyberte **vyberte**.

7. V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-sso"></a>Test jednotného přihlašování

V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí přístupového panelu.

Když vyberete dlaždici Cezanne HR softwaru na přístupovém panelu, můžete přihlásit automaticky do vaší aplikace Cezanne HR softwaru.

## <a name="next-steps"></a>Další kroky

* [Seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

