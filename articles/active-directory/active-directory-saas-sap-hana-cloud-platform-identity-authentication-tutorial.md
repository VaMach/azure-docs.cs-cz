---
title: "Kurz: Azure Active Directory integrace s SAP cloudové platformy identitu ověřování | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP cloudové platformy identitu ověřování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: e4e7c7273acc216ae82a52b3e6dcd530a6ad1bd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Kurz: Azure Active Directory integrace s SAP cloudové platformy identitu ověřování

V tomto kurzu zjistěte, jak integrovat SAP cloudové platformy identitu ověřování s Azure Active Directory (Azure AD). SAP cloudové platformy identitu ověřování slouží jako proxy server poskytovatelů identity pro přístup k SAP aplikacím pomocí služby Azure AD jako hlavní deklarací identity.

Integrace SAP cloudové platformy identitu ověřování s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, kdo má přístup k aplikaci SAP.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k SAP aplikace jednotné přihlašování (SSO)) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SAP cloudové platformy identitu ověřování, potřebujete následující položky:

- Předplatné služby Azure AD
- Ověření Identity platformy cloudu SAP jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP cloudové platformy identitu ověřování z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

Než začnete technické informace, je důležité pochopit, koncepty, které se chystáte podívejte se na. SAP cloudové platformy identitu ověřování a Azure Active Directory federation umožňuje implementovat jednotného přihlašování napříč aplikacím nebo službám chráněným pomocí AAD (jako IdP) s aplikacemi SAP a službám chráněným pomocí SAP Cloudová platforma identita Ověřování.

V současné době SAP cloudové platformy identitu ověřování funguje jako zprostředkovatel Identity Proxy aplikací SAP. Azure Active Directory pak funguje jako počáteční zprostředkovatele Identity v rámci této instalace. 

Následující diagram znázorňuje toto:

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

S tímto nastavením budou klientovi SAP cloudové platformy identitu ověřování nakonfigurované jako důvěryhodné aplikace v Azure Active Directory. 

Všechny aplikace SAP a službách, které chcete chránit pomocí tímto způsobem jsou následně nakonfigurované v konzole pro správu SAP cloudové platformy identitu ověřování!

To znamená, že autorizace pro udělení přístupu k SAP aplikace a služby se musí provést v SAP cloudové platformy identitu ověřování pro taková konfigurace (na rozdíl od konfigurace ověřování ve službě Azure Active Directory).

Nakonfigurováním SAP cloudové platformy identitu ověřování jako aplikaci prostřednictvím Azure Active Directory Marketplace nepotřebujete postará o konfiguraci potřebné jednotlivé deklaracích / SAML kontrolní výrazy a transformace, které jsou potřebné k vytvoření platná ověřovací token pro SAP aplikace.

>[!NOTE] 
>Obě strany, pouze byl testován aktuálně jednotného přihlašování k webu. Toky potřebné pro aplikaci API nebo rozhraní API API komunikace by měla fungovat, ale nebyly testovány, ještě. Bude být testována jako součást následné aktivity.
>

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Přidání SAP cloudové platformy identitu ověřování z Galerie
Při konfiguraci integrace SAP cloudové platformy identitu ověřování do služby Azure AD potřebujete přidat SAP cloudové platformy identitu ověřování z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat SAP cloudové platformy identitu ověřování z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **SAP cloudové platformy identitu ověřování**, vyberte **SAP cloudové platformy identitu ověřování** z panelu výsledků klikněte **přidat** tlačítko pro přidáte aplikaci.

    ![SAP cloudové platformy identitu ověřování v seznamu výsledků](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP cloudové platformy ověření Identity na základě testovací uživatele, nazývá "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP cloudové platformy identitu ověřování je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SAP cloudové platformy identitu ověřování je potřeba vytvořit.

V SAP cloudové platformy identitu ověřování přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP cloudové platformy identitu ověřování, musíte dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s SAP cloudové platformy identitu ověřování](#create-an-sap-cloud-platform-identity-authentication-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP cloudové platformy identitu ověřování propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP cloudové platformy identitu ověřování.

**Ke konfiguraci Azure AD jednotné přihlašování s SAP cloudové platformy identitu ověřování, proveďte následující kroky:**

1. Na portálu Azure na **SAP cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Na **SAP cloudové platformy identitu ověřování domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:

    ![SAP cloudové platformy identitu ověřování domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<entity-id>.accounts.ondemand.com`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tato hodnota se skutečným identifikátorem. Obraťte se na [tým podpory SAP cloudové platformy identitu ověřování klienta](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) získat tuto hodnotu. Pokud si nejste jisti tuto hodnotu, postupujte podle dokumentace SAP cloudové platformy identitu ověřování v [konfiguraci klienta SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**. Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![SAP cloudové platformy identitu ověřování domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<entity-id>.accounts.ondemand.com/admin`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory SAP cloudové platformy identitu ověřování klienta](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) získat tuto hodnotu.

5. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. Ověření Identity platformy cloudové aplikace SAP očekává SAML kontrolní výrazy ve specifickém formátu. Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad pro tento. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, pokud vaše aplikace SAP očekává atribut například "jméno". V dialogovém okně atributy tokenu SAML přidejte atribut "jméno".

    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. V **název** textovému poli, zadejte název atributu "jméno".
    
    c. Z **hodnotu** vyberte hodnotu atributu "user.givenname".
    
    d. Klikněte na tlačítko **OK**.

8. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. Na **SAP cloudové platformy identitu ověřování konfigurace** klikněte na tlačítko **konfigurace SAP cloudové platformy identitu ověřování** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurace ověřování SAP cloudové platformy Identity](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte do konzoly pro SAP cloudové platformy identitu ověřování správu. Adresa URL má následující vzor: `https://<tenant-id>.accounts.ondemand.com/admin`. Potom postupujte podle dokumentace na SAP cloudové platformy identitu ověřování pro [konfigurace Microsoft Azure AD jako podnikové poskytovatele Identity v SAP cloudové platformy identitu ověřování](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Na portálu Azure klikněte na tlačítko **Uložit** tlačítko.

12. Následující kroky pokračujte, pouze pokud chcete přidat a povolení jednotného přihlašování pro jinou aplikaci SAP. Opakujte kroky v části "Přidání SAP cloudové platformy ověření Identity z Galerie" přidat další instanci SAP cloudové platformy identitu ověřování.

13. Na portálu Azure na **SAP cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **propojené přihlášení**.

    ![Konfigurovat propojené přihlášení](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. Uložte konfiguraci.

>[!NOTE] 
>Nová aplikace bude využívat Konfigurace jednotného přihlašování pro předchozí aplikaci SAP. Zkontrolujte prosím, že používáte stejné podnikové poskytovatelů identit v konzole pro správu ověřování SAP cloudové platformy Identity.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Vytvořit testovací uživatele s SAP cloudové platformy identitu ověřování

Nemusíte vytvořit uživatele na SAP cloudové platformy identitu ověřování. Uživatelé, kteří jsou v úložišti uživatele Azure AD můžete použít funkci jednotného přihlašování.

SAP cloudové platformy identitu ověřování podporuje možnost federaci identit. Tato možnost umožňuje aplikace a zjistit, pokud ověřený zprostředkovatelem podnikové identitě uživatele existovat v úložišti z SAP cloudové platformy Identity ověření uživatele. 

Ve výchozím nastavení je zakázána možnost federaci identit. Pokud je povoleno federaci identit, mohou pouze uživatelé, kteří jsou importovány v SAP cloudové platformy identitu ověřování přístup k aplikaci. 

Další informace o tom, jak povolit nebo zakázat federaci identit s SAP cloudové platformy identitu ověřování najdete v tématu povolit federaci identit s SAP cloudové platformy identitu ověřování v [konfigurace federaci identit pomocí Ověření Identity uživatele úložiště SAP cloudové platformy](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k SAP cloudové platformy identitu ověřování.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon SAP cloudové platformy identitu ověřování, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SAP cloudové platformy identitu ověřování**.

    ![Odkaz SAP cloudové platformy identitu ověřování v seznamu aplikací](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici SAP cloudové platformy Identity ověřování na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SAP cloudové platformy identitu ověřování.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

