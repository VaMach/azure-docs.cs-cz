---
title: 'Kurz: Azure Active Directory integrace s Google Apps v Azure | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Kurz: Azure Active Directory integrace s Google Apps

V tomto kurzu zjistěte, jak integrovat Google Apps v Azure Active Directory (Azure AD).

Integrace s Azure AD Google Apps poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup ke Google Apps
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Google Apps (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Google Apps, potřebujete následující položky:

- Předplatné služby Azure AD
- Google Apps jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Videokurz
Postup při povolení jednotného přihlašování ke Google Apps během 2 minut.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
1. **Otázka: je Chromebooks a dalších zařízení Chrome kompatibilní s Azure AD jednotné přihlašování?**
   
    Odpověď: Ano, se uživatelé moct přihlásit k jejich Chromebook zařízení pomocí svých přihlašovacích údajů Azure AD. Toto [Google Apps podporují článku](https://support.google.com/chrome/a/answer/6060880) informace o důvod, proč může získat uživatelé vyzváni k zadání pověření dvakrát.

2. **Otázka: Pokud lze povolit jednotné přihlašování, budou uživatelé moci žádné Google produkt, například Google učebny, GMail, Google Drive, YouTube a tak dále se přihlaste pomocí přihlašovacích údajů Azure AD?**
   
    Odpověď: Ano, v závislosti na [které Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) rozhodnete povolit nebo zakázat pro vaši organizaci.

3. **Otázka: je možné povolit jednotné přihlašování pro pouze podmnožinu Moji uživatelé Google Apps?**
   
    Odpověď: Ne, zapnout jednotné přihlašování okamžitě vyžaduje všichni uživatelé Google Apps k ověření pomocí přihlašovacích údajů Azure AD. Protože Google Apps nepodporuje, s více poskytovatelů identit, zprostředkovatele identity pro vaše prostředí Google Apps může být buď Azure AD nebo Google – ale ne oba ve stejnou dobu.

4. **Otázka: Pokud je uživatel přihlášený prostřednictvím systému Windows, jsou že automaticky ověřování pro Google Apps bez získávání výzva k zadání hesla?**
   
    Odpověď: existují dvě možnosti pro povolení tohoto scénáře. Nejdřív by uživatelé přihlašovat do zařízení s Windows 10 prostřednictvím [Azure Active Directory Join](active-directory-azureadjoin-overview.md). Alternativně může uživatelé přihlašovat do zařízení s Windows, které jsou připojené k doméně pro místní Active Directory byla povolena pro jednotné přihlašování do služby Azure AD pomocí [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) nasazení. Obě možnosti vyžadují, abyste proveďte kroky v následujícím kurzu umožňující jednotného přihlašování mezi službou Azure AD a Google Apps.

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Google Apps z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-google-apps-from-the-gallery"></a>Přidání Google Apps z Galerie
Při konfiguraci integrace Google Apps do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS aplikace Google z galerie.

**Pokud chcete přidat Google Apps z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Google Apps**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. Na panelu výsledků vyberte **Google Apps**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Google Apps je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Google Apps.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Google Apps.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Google Apps](#creating-a-google-apps-test-user)**  – Pokud chcete mít protějšek Britta Simon v Google Apps, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Google Apps.

**Ke konfiguraci Azure AD jednotné přihlašování s Google Apps, postupujte takto:**

1. Na portálu Azure na **Google Apps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Na **Google Apps domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte hodnotu s skutečná adresa URL přihlašování. Obraťte se [tým podpory Google](https://www.google.com/contact/).
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. Na **Google Apps konfigurace** klikněte na tlačítko **konfigurace Google Apps** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby a změnit heslo URL** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. V prohlížeči otevřete novou kartu a přihlaste se k [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce.

8. Klikněte na tlačítko **zabezpečení**. Pokud nevidíte odkaz, mohou být skryty pod **více ovládacích prvků** nabídky v dolní části obrazovky.
   
    ![Klikněte na Zabezpečení.][10]

9. Na **zabezpečení** klikněte na tlačítko **nastavit jednotné přihlašování (SSO).**
   
    ![Klikněte na tlačítko jednotné přihlašování.][11]

10. Proveďte následující změny konfigurace:
   
    ![Konfigurace jednotného přihlašování][12]
   
    a. Vyberte **nastavení jednotného přihlašování pomocí zprostředkovatele identity jiných výrobců**.

    b. V **přihlašovací adresa URL stránky** pole v Google Apps, vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.

    c. V **adresy URL odhlašovací stránky** pole v Google Apps, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure. 

    d. V **změnit heslo URL** pole v Google Apps, vložte hodnotu **změnit heslo URL**, který jste zkopírovali z portálu Azure. 

    e. V Google Apps pro **ověřovací certifikát**, nahrajte certifikát, který jste si stáhli z portálu Azure.

    f. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-google-apps-test-user"></a>Vytvoření zkušebního uživatele Google Apps

Cílem této části je vytvoření uživatele volal Britta Simon v Google Apps softwaru. Google Apps podporuje automatické zřizování, který je ve výchozím nastavení povolené. Neexistuje žádná akce pro vás v této části. Pokud uživatel v Google Apps softwaru ještě neexistuje, vytvoří se nový při pokusu o přístup k softwaru Google Apps.

>[!NOTE] 
>Pokud je potřeba ručně vytvořit uživateli, obraťte se [tým podpory Google](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ke Google Apps.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Google Apps, postupujte takto:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Google Apps**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části, pokud chcete otestovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu v [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), pak se přihlaste do testovací účet a klikněte na tlačítko **Google Apps** dlaždici na přístupovém panelu.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png