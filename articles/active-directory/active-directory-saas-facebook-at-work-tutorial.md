---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.

V tomto kurzu zjistěte, jak k síti na pracovišti ve službě Facebook integraci s Azure Active Directory (Azure AD).

Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k síti na pracovišti ve službě Facebook.
- Můžete povolit uživatelům automaticky získat přihlášeného k síti na pracovišti ve službě Facebook (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě: portál Azure.

Další podrobnosti o softwaru, služba (SaaS) aplikace integraci s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD se na pracovišti ve službě Facebook, potřebujete následující položky:

- Předplatné služby Azure AD
- Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)

Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidejte síti na pracovišti ve službě Facebook z galerie.
2. Konfigurace a otestování Azure AD jednotné přihlašování.

## <a name="add-workplace-by-facebook-from-the-gallery"></a>Přidat pracovní ploše ve službě Facebook z Galerie
Při konfiguraci integrace síti na pracovišti ve službě Facebook do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS síti na pracovišti ve službě Facebook z galerie.

1. V [portál Azure](https://portal.azure.com), v levém podokně vyberte **Azure Active Directory**. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte do **podnikové aplikace, které** > **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**a vyberte **síti na pracovišti ve službě Facebook** z výsledků. Potom vyberte **přidat**, chcete-li přidat aplikaci.

    ![Síti na pracovišti ve službě Facebook v seznamu výsledků](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se síti na pracovišti ve službě Facebook, podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v síti na pracovišti ve službě Facebook je pro uživatele ve službě Azure AD. Jinými slovy je třeba vytvořit propojené vztah mezi uživatele Azure AD a související uživateli v síti na pracovišti ve službě Facebook.

Vytvořit tento vztah přiřazením hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v síti na pracovišti ve službě Facebook.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části povolíte Azure AD jednotného přihlašování k portálu Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.

1. Na portálu Azure na **síti na pracovišti ve službě Facebook** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. V **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. V **síti na pracovišti Facebook domény a adresy URL** část, postupujte takto:

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL, která používá následující vzorec:`https://<company subdomain>.facebook.com`

    b. V **identifikátor** textového pole zadejte adresu URL, která používá následující vzorec:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Tyto hodnoty jsou pouze příklad. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) k získání těchto hodnot. 

4. V **SAML podpisový certifikát** vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Vyberte **Uložit**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. V **síti na pracovišti v konfiguraci sítě Facebook** vyberte **pracoviště nakonfigurovat ve službě Facebook** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka** části.

    ![Síti na pracovišti v konfiguraci sítě Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. V okně prohlížeče jiný web Přihlaste se k pracovní ploše lokalitou Facebook společnosti jako správce.
  
   > [!NOTE] 
   > Jako součást procesu ověřování SAML můžete síti na pracovišti velikost použít řetězce dotazu až 2,5 kilobajtů, aby bylo možné předat parametry do služby Azure AD.

8. V **řídicí panel společnosti**, přejděte na **ověřování** kartě.

9. V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu.

10. Zadejte hodnoty zkopírovaných z **síti na pracovišti v konfiguraci sítě Facebook** části portálu Azure do odpovídajícího pole:

    *   V **SAML URL** text vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.
    *   V **URL vystavitele SAML** textové pole, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.
    *   V **SAML odhlášení přesměrovat (volitelné)**, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.
    *   Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure. Zkopírujte obsah ho do schránky a vložte jej do **certifikátu SAML** textové pole.

11. Možná budete muset zadat adresu URL cílové skupiny příjemce adresy URL a adresy URL služby ACS (služba Assertion příjemce), uvedené v části **konfigurace SAML** části.

12. Přejděte do dolní části a vyberte **Test jednotného přihlašování k**. Automaticky otevírané okno se zobrazí se na přihlašovací stránku služby Azure AD. K ověření, zadejte přihlašovací údaje jako normální. Ujistěte se, e-mailovou adresu vrátil zpět z Azure AD je stejný jako pracovní účet, který jste se přihlásili.

13. Pokud se test byl úspěšně dokončen, přejděte do dolní části stránky a vyberte **Uložit**.

14. Každý, kdo používá síti na pracovišti se teď zobrazí s Azure AD přihlašovací stránka pro ověřování.

Můžete nakonfigurovat přihlašování SAML adresu URL, který můžete použít tak, aby odkazoval na odhlašovací stránky Azure AD. Pokud je toto nastavení povolené a nakonfigurované, uživatel se přesměruje už k síti na pracovišti odhlašovací stránky. Místo toho se uživatel přesměruje na adresu URL, která byla přidána do nastavení odhlášení přesměrování SAML.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco k nastavení aplikace. Po přidání této aplikace z **služby Active Directory** > **podnikové aplikace, které** jednoduše vyberte **jednotné přihlašování** kartě a přístup vložené dokumentace prostřednictvím **konfigurace** v dolní části. Si můžete přečíst informace o funkci embedded dokumentace v [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Nakonfigurovat četnost opětovné ověření

Můžete nakonfigurovat síti na pracovišti, aby se zobrazila výzva pro kontrolu SAML každý den, tři dny jeden týden, dva týdny, jeden měsíc, nebo vůbec.

> [!NOTE] 
>Minimální hodnota pro kontrolu SAML na mobilní aplikace nastavena na jeden týden.

Můžete taky přinutit SAML obnovit pro všechny uživatele. Chcete-li to provést, použijte **vyžadovat ověřování SAML pro všechny uživatele nyní** tlačítko.


### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

1. V **portál Azure**, v levém podokně vyberte **Azure Active Directory**.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a vyberte **všichni uživatelé**.
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat**.
 
    ![Tlačítko Přidat](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. V **uživatele** dialogové okno pole, postupujte takto:
 
    ![Dialogové okno uživatele](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. V **název** textového pole, typ **BrittaSimon**.

    b. V **uživatelské jméno** textového pole, zadejte **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla**a poznamenejte si ho.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Vytvoření pracovišti Facebook testovací uživatel

V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook. Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná akce pro vás v této části. Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, nový vytvoří se při pokusu o přístup k síti na pracovišti Facebook.

>[!Note]
>Pokud je potřeba ručně vytvořit uživateli, obraťte se [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k síti na pracovišti ve službě Facebook.

![Přiřadit uživatele][200] 

1. Na portálu Azure otevřete zobrazení aplikace. Přejděte do zobrazení adresáře, přejděte na **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **síti na pracovišti ve službě Facebook**.

    ![K síti na pracovišti podle propojení sítě Facebook v seznamu aplikací](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. V nabídce na levé straně vyberte **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202] 

4. Vyberte **Přidat**. Potom v **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.

    ![V podokně Přidat přiřazení][203]

5. V **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelů.

6. V **uživatelů a skupin** dialogové okno, vyberte **vyberte**.

7. V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.
Další informace najdete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Další kroky

* Najdete v článku [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md).
* Čtení [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Další informace o tom, jak [konfiguraci zřizování uživatelů](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

