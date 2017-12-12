---
title: "Kurz: Azure Active Directory integrace s nástrojem Adobe prostředí Manager | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Adobe prostředí správce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4a4fccc4210fd6cf0ddbe99089c84a1fd38d5b09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Kurz: Azure Active Directory integrace s nástrojem Adobe prostředí Manager

V tomto kurzu zjistěte, jak integrovat Adobe prostředí správce s Azure Active Directory (Azure AD).

Integrace Adobe prostředí správce s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, kdo má přístup k aplikaci Adobe prostředí Manager.
- Můžete povolit uživatelům, aby automaticky získat přihlášeného k aplikaci Adobe prostředí správce (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD pomocí Adobe prostředí správce, potřebujete následující položky:

- Předplatné služby Azure AD
- Adobe prostředí správce jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání správce prostředí Adobe z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Přidání správce prostředí Adobe z Galerie
Konfigurace integrace nástroje Adobe prostředí Manager do Azure AD, potřebujete přidat správce prostředí Adobe z Galerie si na seznam spravovaných aplikací SaaS.

**Přidání správce prostředí Adobe z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Manager prostředí Adobe**, vyberte **Manager prostředí Adobe** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Adobe prostředí správce v seznamu výsledků](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe prostředí Manager na základě testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v aplikaci Adobe prostředí správce je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v aplikaci Adobe prostředí správce musí navázat.

Ve Správci prostředí Adobe přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Adobe prostředí správce, musíte dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit uživatele s testovací prostředí správce Adobe](#create-an-adobe-experience-manager-test-user)**  – Pokud chcete mít protějšek Britta Simon v Adobe prostředí správce, který je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe prostředí správce.

**Ke konfiguraci Azure AD jednotné přihlašování s Adobe prostředí Manager, proveďte následující kroky:**

1. Na portálu Azure na **Manager prostředí Adobe** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. Na **Adobe prostředí správce domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace v **IdP** režimu:

    ![Adobe prostředí správce domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. V **identifikátor** textovému poli, zadejte jedinečnou hodnotu, která definujete na vašem AEM serveru. 

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<AEM Server Url>/saml_login`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi. Obraťte se na [tým podpory Adobe prostředí správce](https://helpx.adobe.com/support/experience-manager.html) k získání těchto hodnot.
 
4. Zkontrolujte zobrazit upřesňující nastavení adresy URL a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![Adobe prostředí správce domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL vašeho Adobe prostředí správce serveru. 

5. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. V části Adobe prostředí Správce konfigurace klikněte na konfigurovat Adobe prostředí Správce konfigurace přihlášení okno otevřete. Kopírování **SAML adresa URL přihlašování služby**, **SAML Entity ID** a **Sign-Out ID** z části Stručná referenční příručka.

    ![Odkaz na konfiguraci oddílu](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Otevřete **Manager prostředí Adobe** portál pro správu v jiném okně prohlížeče.

9. Vyberte **nastavení** -> **zabezpečení** -> **uživatelé**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Vyberte **správce** nebo jiné příslušné uživatele.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Vyberte **nastavení účtu** -> **vytvořit a spravovat TrustStore**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Klikněte na tlačítko **vyberte soubor certifikátu** z **přidat certifikát ze souboru CER** tlačítko. Procházet a vyberte soubor certifikátu, který jste si stáhli z portálu Azure.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Certifikát je přidán do TrustStore. Všimněte si alias certifikátu.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Na **uživatelé** vyberte **Služba ověřování**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Vyberte **nastavení účtu** -> **vytvořit nebo spravovat úložiště klíčů**. Vytvoření úložiště klíčů zadáním hesla.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Přejděte zpět na správu obrazovky a vyberte **nastavení** -> **operace** -> **webové konzole**.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

17. Otevře se stránka konfigurace.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

18. Najít **Adobe Granite SAML 2.0 obslužná rutina ověřování** a klikněte na **přidat** ikonu.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Proveďte následující akce na této stránce.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. V **cesta** textovému poli, zadejte  **/** .

    b. V **IDP URL** textovému poli, zadejte hodnotu **SAML adresa URL přihlašování služby**, který jste zkopírovali z portálu Azure.

    c. V **IDP certifikát Alias** textovému poli, zadejte hodnotu **certifikát Alias**, který jste přidali v TrustStore.

    d. V **zabezpečení zadané ID Entity** textovému poli, zadejte hodnotu jedinečný **SAML Entity ID**, který jste nakonfigurovali v portálu Azure.

    e. V **adresa URL služby příjemce Assertion** textovému poli, zadejte hodnotu **adresa URL odpovědi**, který jste nakonfigurovali v portálu Azure.

    f. V **úložiště hesel klíč** textovému poli, zadejte **heslo**, které jste nastavili v úložišti klíčů.

    g. V **atribut ID uživatele** textovému poli, zadejte **ID názvu** nebo jiné ID uživatele, který je ve vašem případě relevantní.

    h. Vyberte **CRX automaticky vytvořit uživatele.**

    i. V **adresy URL odhlašovací** textovému poli, zadejte hodnotu jedinečný **Sign-Out URL** které jste z portálu Azure.

    j. Klikněte na tlačítko **uložit**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Vytvořit testovací uživatele s Adobe činnost správce

V této části vytvoříte uživatele volal Britta Simon v aplikaci Adobe prostředí správce. Pokud jste vybrali **automaticky vytvořit CRX uživatelé** možnost uživatelé vytvoří automaticky po úspěšném ověření. 

Pokud chcete ručně vytvořit uživatele, spojte se s [tým podpory Adobe prostředí správce](https://helpx.adobe.com/support/experience-manager.html) přidat uživatele do platformy Adobe prostředí správce. 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k aplikaci Adobe prostředí správce.

![Přiřadit role uživatele][200] 

**Britta Simon přiřadit k aplikaci Adobe prostředí Manager, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Manager prostředí Adobe**.

    ![Odkaz Adobe prostředí správce v seznamu aplikací](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Adobe prostředí správce na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Adobe prostředí správce.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_203.png

