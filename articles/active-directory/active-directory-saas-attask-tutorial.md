---
title: 'Kurz: Azure Active Directory integrace s @Task| Microsoft Docs'
description: "Naučte se konfigurovat jednotné přihlašování mezi Azure Active Directory a @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Kurz: Azure Active Directory integrace s@Task
Cílem tohoto kurzu je ukazují, jak integrovat @Task službou Azure Active Directory (Azure AD).  
Integrace @Task s Azure AD poskytuje následující výhody: 

* Můžete řídit ve službě Azure AD, kdo má přístup k@Task
* Můžete povolit uživatelům automaticky přihlášení k získání @Task (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Při konfiguraci integrace Azure AD s @Task, potřebujete následující položky:

* Předplatné služby Azure AD
* @Task Jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedených v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání @Task z Galerie 
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-task-from-the-gallery"></a>Přidání @Task z Galerie
Konfigurace integrace @Task do služby Azure AD, je nutné přidat @Task z Galerie si na seznam spravovaných aplikací SaaS.

**Chcete-li přidat @Task z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1] 
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2] 
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3] 
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4] 
6. Do vyhledávacího pole zadejte  **@Task** .
   
    ![Aplikace][5] 
7. V podokně výsledků vyberte  **@Task** a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Aplikace][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Cílem této části je návod, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s @Task podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v Azure AD @Task pro uživatele ve službě Azure AD je. Jinými slovy, odkaz vztah mezi uživatele Azure AD a související uživatelské v @Task musí být vytvořeno.   
Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v @Task.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s @Task, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření @Tasktest uživatele](#creating-a-halogen-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v @Taskthat je propojený s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení
Cílem této části je chcete povolit Azure AD jednotného přihlašování na portálu Azure classic a nakonfigurovat jednotné přihlašování v vaší @Task aplikace.

**Ke konfiguraci Azure AD jednotné přihlašování s @Task, proveďte následující kroky:**

1. Na portálu Azure classic na  **@Task**  stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na **jak chcete uživatelům se přihlásit na @Task**  vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][7] 
3. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
   
    ![Konfigurovat nastavení aplikace][8] 
   
     a. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení k vaší @Task aplikace (např:*https://<Tenant name>.attask ondemand.com*).
   
     b. Klikněte na **Další**.
4. Na **nakonfigurovat jednotné přihlašování v @Task**  klikněte na **stáhnout metadata**, uložte soubor metadat místně na vašem počítači a pak klikněte na **Další**.
   
    ![Co je služba Azure AD Connect][9] 
5. Přihlášení k vaší @Task společnosti lokality jako správce.
6. Přejděte na **jednotné přihlašování v konfiguraci**.
7. Na **jednotné přihlašování** dialogové okno, proveďte následující kroky
   
    ![Konfigurovat jednotné přihlašování][23]
   
    a. Jako **typ**, vyberte **SAML 2.0**.
   
    b. Vyberte **služby ID zprostředkovatele**.
   
    c. Na portálu Azure classic, zkopírujte **vzdálené adresy URL pro přihlášení**a vložte ji do **adresu URL pro přihlášení portálu** textové pole.
   
    d. Na portálu Azure classic, zkopírujte **jednu adresu URL služby Sign-Out**a vložte ji do **Sign-Out URL** textové pole.
   
    e. Na portálu Azure classic, zkopírujte **heslo změnit adresu URL**a vložte ji do **heslo změnit adresu URL** textové pole.
   
    f. Klikněte na **Uložit**.
8. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Další**. 
   
    ![Co je služba Azure AD Connect][10]
9. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
   
    ![Co je služba Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.  

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.
6. Na **profil uživatele** dialogové okno proveďte následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. V **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V **příjmení** textovému poli, typ, **Simon**.
   
    c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Poznamenejte si hodnotu **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-an-task-test-user"></a>Vytvoření @Task testovacího uživatele
Cílem této části je vytvoření uživatele volal Britta Simon v @Task.

**Vytvoření uživatele volal Britta Simon v @Task, proveďte následující kroky:**

1. Přihlaste se k vaší @Task společnosti lokality jako správce.
2. V nabídce v horní části, klikněte na tlačítko **osoby**.
3. Klikněte na tlačítko **nové osobě**. 
4. V dialogovém okně nové osobě proveďte následující kroky:
   
    ![Vytvoření @Task testovacího uživatele][21] 
   
    a. V **křestní jméno** textovému poli, zadejte "Britta".
   
    b. V **příjmení** textovému poli, zadejte "Simon".
   
    c. V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.
   
    d. Klikněte na tlačítko **přidat osobu**.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD
Cílem této části je povolení Britta Simon používat Azure jednotné přihlašování, poskytněte svůj přístup k @Task.

![Přiřadit uživatele][200] 

**Přiřadit Britta Simon k @Task, proveďte následující kroky:**

1. Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][201] 
2. V seznamu aplikací vyberte  **@Task** .
   
    ![Přiřadit uživatele][202] 
3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 
4. V seznamu uživatelů vyberte **Britta Simon**.
5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.  
Když kliknete @Task dlaždici na přístupovém panelu, měli byste obdržet automaticky přihlášení k vaší @Task aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






