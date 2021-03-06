---
title: 'Kurz: Azure Active Directory integrace s SpringCM | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SpringCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 7538c749b3fc31ad02e52ca986e43e3293e2fe20
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Kurz: Azure Active Directory integrace s SpringCM

V tomto kurzu zjistěte, jak integrovat SpringCM s Azure Active Directory (Azure AD).

Integrace SpringCM s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k SpringCM
- Můžete povolit uživatelům, aby automaticky získat přihlášení k SpringCM (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SpringCM, potřebujete následující položky:

- Předplatné služby Azure AD
- SpringCM jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SpringCM z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-springcm-from-the-gallery"></a>Přidání SpringCM z Galerie
Při konfiguraci integrace SpringCM do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS SpringCM z galerie.

**Pokud chcete přidat SpringCM z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **SpringCM**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. Na panelu výsledků vyberte **SpringCM**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SpringCM podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SpringCM je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SpringCM musí navázat.

V SpringCM, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SpringCM, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SpringCM](#creating-a-springcm-test-user)**  – Pokud chcete mít protějšek Britta Simon v SpringCM propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SpringCM.

**Ke konfiguraci Azure AD jednotné přihlašování s SpringCM, proveďte následující kroky:**

1. Na portálu Azure na **SpringCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. Na **SpringCM domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory SpringCM klienta](https://knowledge.springcm.com/support) získat tuto hodnotu. 
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. Na **SpringCM konfigurace** klikněte na tlačítko **konfigurace SpringCM** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. V okně prohlížeče jiný web, přihlaste se k vaší **SpringCM** společnosti lokality jako správce.

8. V nabídce v horní části, klikněte na tlačítko **přejít na**, klikněte na tlačítko **Předvolby**a pak na **předvolby účtu** klikněte na tlačítko **jednotné přihlašování SAML**.
   
    ![JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML")

9. V části Konfigurace zprostředkovatele Identity proveďte následující kroky:
   
    ![Konfigurace zprostředkovatele identity](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "konfigurace zprostředkovatele Identity")
    
    a. Nahrajte certifikát stažený Azure Active Directory, klikněte na tlačítko **vybrat certifikát vystavitele** nebo **změnu vystavitele certifikátu**.
    
    b. Vložení **SAML Entity ID** hodnotu, kterou jste zkopírovali z portálu Azure do **vystavitele** textové pole.
    
    c. Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **koncový bod iniciované poskytovatele služeb (SP)** textové pole.
            
    d. Vyberte **povoleno SAML** jako **povolit**.

    e. Klikněte na **Uložit**.
 
> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-springcm-test-user"></a>Vytvoření zkušebního uživatele SpringCM

Pokud chcete povolit uživatelů Azure Active Directory k přihlášení do SpringCM, musí být zřízená do SpringCM. V případě SpringCM zřizování je ruční úloha.

>[!NOTE]
>Další informace najdete v tématu [vytvoření a úprava uživatele SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user). 

**K poskytnutí uživatelského účtu do SpringCM, proveďte následující kroky:**

1. Přihlaste se k vaší **SpringCM** společnosti lokality jako správce.

2. Klikněte na tlačítko **GOTO**a potom klikněte na **adresáře**.
   
    ![Vytvoření uživatele](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "vytvoření uživatele")

3. Klikněte na tlačítko **vytvořit uživatele**.

4. Vyberte **Role uživatele**.

5. Vyberte **odeslání e-mailu aktivace**.

6. Zadejte jméno, příjmení a e-mailovou adresu platného účtu uživatele Azure Active Directory, který má být zahrnuty do související textových polí.

7. Přidejte uživatele do **skupiny zabezpečení**.

8. Klikněte na **Uložit**.

  >[!NOTE]
  >Můžete použít všechny ostatní SpringCM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SpringCM zřídit AAD uživatelské účty.  
  > 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SpringCM.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon SpringCM, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SpringCM**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.
 
Když kliknete na dlaždici SpringCM na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SpringCM.

Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

