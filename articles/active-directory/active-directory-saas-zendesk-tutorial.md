---
title: 'Kurz: Azure Active Directory integrace s Zendesk | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Kurz: Azure Active Directory integrace s Zendesk

V tomto kurzu zjistěte, jak integrovat Zendesk s Azure Active Directory (Azure AD).

Integrace služby Zendesk s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k této služby
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Zendesk (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Zendesk, potřebujete následující položky:

- Předplatné služby Azure AD
- Této služby jednotného přihlašování povolené předplatné


> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Zendesk z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-zendesk-from-the-gallery"></a>Přidání Zendesk z Galerie
Při konfiguraci integrace služby Zendesk do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Zendesk z galerie.

**Pokud chcete přidat Zendesk z galerie, proveďte následující kroky:**

1. V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Zendesk**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Na panelu výsledků vyberte **Zendesk**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí služby Zendesk podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v této služby je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v této služby je nutné stanovit.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v této služby.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zendesk, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Zendesk](#creating-a-zendesk-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zendesku propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci služby Zendesk.

**Ke konfiguraci Azure AD jednotné přihlašování s Zendesk, proveďte následující kroky:**

1. Na portálu Azure na **Zendesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Na **Zendesk domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.zendesk.com`

    b. V **identifikátor** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátoru adresy URL. Obraťte se na [tým podpory služby Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) k získání těchto hodnot. 

4. Zendesk očekává SAML kontrolní výrazy ve specifickém formátu. Neexistují žádné povinné atributy SAML, ale můžete volitelně přidat atribut z **uživatelské atributy** části podle následujících kroků: 

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy** zaškrtávací políčko.
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Klikněte **přidat atribut** otevřete **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. V **název** textovému poli, zadejte název atributu (například **emailaddress**).
    
    d. Z **hodnotu** vyberte hodnotu atributu (jako **user.mail**).
    
    e. Klikněte na tlačítko **Ok**
 
    > [!NOTE] 
    > Atributy rozšíření je použít k přidání atributů, které nejsou ve službě Azure AD ve výchozím nastavení. Klikněte na tlačítko [atributy uživatele, které lze nastavit v SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) získat úplný seznam SAML atributy, které **Zendesk** přijímá. 

5. Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Na **Zendesk konfigurace** klikněte na tlačítko **konfigurace Zendesk** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zendesk.

8. Klikněte na tlačítko **správce**.

9. V levém navigačním podokně klikněte na tlačítko **nastavení**a potom klikněte na **zabezpečení**.

10. Na **zabezpečení** proveďte následující kroky: 
   
     ![Zabezpečení](./media/active-directory-saas-zendesk-tutorial/ic773089.png "zabezpečení")

    ![Jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/ic773090.png "jednotného přihlašování")

     a. Klikněte **správce & agenty** kartě.

     b. Vyberte **jednotné přihlašování (SSO) a SAML**a potom vyberte **SAML**.

     c. V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure. 

     d. V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.
        
     e. V **otisků prstů certifikátů** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.
     
     f. Klikněte na **Uložit**.

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-a-zendesk-test-user"></a>Vytvoření zkušebního uživatele Zendesk

Povolit uživatelům přihlášení do Azure AD **Zendesk**, musí být zřízená do **Zendesk**.  
V závislosti na role přiřazené v aplikace je očekávané chování:

 1. **Koncový uživatel** účty jsou zřízené automaticky při přihlášení.
 2. **Agent** a **správce** účty muset ručně zřídit v **Zendesk** před přihlášením.
 
**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k vaší **Zendesk** klienta.

2. Vyberte **seznamu zákazníků** kartě.

3. Vyberte **uživatele** a klikněte na **přidat**.
   
    ![Přidat uživatele](./media/active-directory-saas-zendesk-tutorial/ic773632.png "přidat uživatele")
4. Zadejte e-mailovou adresu stávajícího účtu Azure AD určené ke zřízení a potom klikněte na **Uložit**.
   
    ![Nový uživatel](./media/active-directory-saas-zendesk-tutorial/ic773633.png "nového uživatele")

> [!NOTE]
> Můžete použít všechny ostatní Zendesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Zendesk zřídit AAD uživatelské účty.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k této služby.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Zendesk, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Zendesk**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici služby Zendesk na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci služby Zendesk.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
