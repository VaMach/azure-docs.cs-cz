---
title: 'Kurz: Azure Active Directory integrace s Google Apps | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: f3b0d48534113dea152aba632e59d03ed78db301
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Kurz: Azure Active Directory integrace s Google Apps

V tomto kurzu zjistěte, jak integrovat Google Apps v Azure Active Directory (Azure AD).

Integrace s Azure AD Google Apps poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup ke Google Apps.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Google Apps (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Google Apps, potřebujete následující položky:

- Předplatné služby Azure AD
- Google Apps jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

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

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Google Apps**, vyberte **Google Apps** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Google Apps v seznamu výsledků](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Google Apps je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Google Apps.

V Google Apps, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Google Apps](#create-a-google-apps-test-user)**  – Pokud chcete mít protějšek Britta Simon v Google Apps, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Google Apps.

**Ke konfiguraci Azure AD jednotné přihlašování s Google Apps, postupujte takto:**

1. Na portálu Azure na **Google Apps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_samlbase.png)

3. Na **Google Apps domény a adresy URL** část, proveďte následující kroky:

    ![Google Apps domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://mail.google.com/a/<yourdomain.com>`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:

    | |
    |--|
    | `google.com`|
    | `http://google.com`|
    | `google.com/<yourdomain.com>`|
    | `http://google.com/a/<yourdomain.com>`|
       
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Google Apps Client](https://www.google.com/contact/) k získání těchto hodnot. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-googleapps-tutorial/tutorial_general_400.png)

6. Na **Google Apps konfigurace** klikněte na tlačítko **konfigurace Google Apps** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby a změnit heslo URL** z **Stručná referenční příručka části.**

    ![Konfigurace aplikace Google](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_configure.png) 

7. V prohlížeči otevřete novou kartu a přihlaste se k [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce.

8. Klikněte na tlačítko **zabezpečení**. Pokud nevidíte odkaz, mohou být skryty pod **více ovládacích prvků** nabídky v dolní části obrazovky.
   
    ![Klikněte na Zabezpečení.][10]

9. Na **zabezpečení** klikněte na tlačítko **nastavit jednotné přihlašování (SSO).**
   
    ![Klikněte na tlačítko jednotné přihlašování.][11]

10. Proveďte následující změny konfigurace:
   
    ![Konfigurace jednotného přihlašování][12]
   
    a. Vyberte **nastavení jednotného přihlašování pomocí zprostředkovatele identity jiných výrobců**.

    b. V **přihlašovací adresa URL stránky** pole v Google Apps, vložte hodnotu **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. V **adresy URL odhlašovací stránky** pole v Google Apps, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure. 

    d. V **změnit heslo URL** pole v Google Apps, vložte hodnotu **změnit heslo URL** který jste zkopírovali z portálu Azure. 

    e. V Google Apps pro **ověřovací certifikát**, nahrajte certifikát, který jste si stáhli z portálu Azure.

    f. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-googleapps-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-googleapps-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-googleapps-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-googleapps-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-google-apps-test-user"></a>Vytvoření zkušebního uživatele Google Apps

Cílem této části je vytvoření uživatele volal Britta Simon v Google Apps softwaru. Google Apps podporuje automatické zřizování, který je ve výchozím nastavení povolené. Neexistuje žádná akce pro vás v této části. Pokud uživatel v Google Apps softwaru ještě neexistuje, vytvoří se nový při pokusu o přístup k softwaru Google Apps.

>[!NOTE] 
>Pokud je potřeba ručně vytvořit uživateli, obraťte se [tým podpory Google](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ke Google Apps.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon Google Apps, postupujte takto:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Google Apps**.

    ![V seznamu aplikací na odkaz Google Apps](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici aplikace Google na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Google Apps.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-googleapps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-googleapps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-googleapps-tutorial/gapps-sso-config.png

