---
title: "Kurz: Azure Active Directory integrace s SciQuest tráví ředitel | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SciQuest tráví ředitel."
services: active-directory
documentationcenter: 
author: jeevansd
manager: mtillman
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 2fe4d6a5fdaeebac142e28a72eced9556ef86795
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Kurz: Azure Active Directory integrace s SciQuest tráví ředitel
Cílem tohoto kurzu je ukazují, jak integrovat SciQuest tráví ředitel Azure Active Directory (Azure AD).  
Integrace SciQuest tráví ředitel s Azure AD poskytuje následující výhody: 

* Můžete řídit ve službě Azure AD, který má přístup k SciQuest tráví ředitel 
* Můžete povolit uživatelům, aby automaticky získat přihlášení k SciQuest tráví ředitel (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Konfigurace integrace Azure AD s SciQuest tráví ředitel, potřebujete následující položky:

* Předplatné služby Azure AD
* SciQuest tráví ředitel jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SciQuest tráví ředitel z Galerie 
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Přidání SciQuest tráví ředitel z Galerie
Při konfiguraci integrace SciQuest tráví ředitel do služby Azure AD potřebujete přidat SciQuest tráví ředitel z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat SciQuest tráví ředitel z galerie, postupujte takto:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1]

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **sciQuest tráví ředitel**.
   
    ![Aplikace][5]

7. V podokně výsledků vyberte **SciQuest tráví ředitel**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Aplikace][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s SciQuest tráví ředitel podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v SciQuest tráví ředitel pro uživatele ve službě Azure AD je Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SciQuest tráví ředitel musí navázat.  
Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SciQuest tráví ředitel.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SciQuest tráví ředitel, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jeden jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele ředitel tráví SciQuest](#creating-a-halogen-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v tráví ředitel SciQuest, propojené služby Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurace Azure AD jednoho jednotného přihlašování
Cílem této části je chcete povolit Azure AD jednotného přihlašování na portálu Azure classic a nakonfigurovat jednotné přihlašování v aplikaci SciQuest tráví ředitel.

**Ke konfiguraci Azure AD jednotné přihlašování s SciQuest tráví ředitel, proveďte následující kroky:**

1. Na portálu Azure classic na **SciQuest tráví ředitel** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování**  Dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][8]

2. Na **jak chcete uživatelům se přihlásit ředitel tráví SciQuest** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][9]

3. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 
   
    ![Konfigurovat nastavení aplikace][10]
   
     a. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaši uživatelé k přihlášení do aplikace tráví ředitel SciQuest pomocí následujícího vzorce: *https://.* SciQuest.com/.**
   
     b. V **adresa URL odpovědi** textovému poli, zadejte stejnou hodnotu, která jste zadali do **přihlašovací adresa URL** textové pole. 
   
     c. Klikněte na **Další**.

4. Na **nakonfigurovat jednotné přihlašování v ředitel tráví SciQuest** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.
   
    ![Co je služba Azure AD Connect][11]

5. Kontaktujte SciQuest podporují tuto metodu ověřování pomocí výše uvedených stažené metadat.

6. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno. 
   
    ![Co je služba Azure AD Connect][15]

7. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Co je služba Azure AD Connect][100] 

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Co je služba Azure AD Connect][101] 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**. 
   
    ![Co je služba Azure AD Connect][102] 

5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:
   
    ![Co je služba Azure AD Connect][103] 
   
    a. Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.
   
    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.

6. Na **profil uživatele** dialogové okno proveďte následující kroky: 
   
    ![Co je služba Azure AD Connect][104] 
   
    a. V **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V **příjmení** txtbox, typ, **Simon**.
   
    c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V **Role** seznamu, vyberte **uživatele**.
   
    e. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Co je služba Azure AD Connect][105]  

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Co je služba Azure AD Connect][106]   
   
    a. Poznamenejte si hodnotu **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-a-sciquest-spend-director-test-user"></a>Vytvoření zkušebního uživatele SciQuest tráví ředitel
Cílem této části je vytvoření uživatele volal Britta Simon v SciQuest tráví ředitel.

Budete muset kontaktujte tým podpory SciQuest tráví ředitele a poskytněte s podrobnostmi o vašem účtu testovací získat vytvořen.

Alternativně můžete využít i za běhu zřizování, jeden přihlašování funkce, která podporuje SciQuest tráví ředitel.  
Pokud za běhu zřizování je povolená, uživatelé jsou automaticky vytváří SciQuest tráví ředitel během jednoho pokusu o přihlášení Pokud ještě neexistují. Tato funkce eliminuje nutnost ručně vytvořit protějšku přihlašování uživatelů.

Pokud chcete získat za běhu zřizování povolena, je potřeba obraťte se na to, co váš tým podpory SciQuest tráví ředitel.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD
Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k SciQuest tráví ředitel Azure jednotné přihlašování.

![Co je služba Azure AD Connect][200]

**Pokud chcete přiřadit Britta Simon SciQuest tráví ředitel, proveďte následující kroky:**

1. Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
   
    ![Co je služba Azure AD Connect][201]

2. V seznamu aplikací vyberte **SciQuest tráví ředitel**.
   
    ![Co je služba Azure AD Connect][202]

3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Co je služba Azure AD Connect][203]

4. V seznamu uživatelů vyberte **Britta Simon**.
   
    ![Co je služba Azure AD Connect][204]

5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Co je služba Azure AD Connect][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.  
Když kliknete na dlaždici SciQuest tráví ředitel na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SciQuest tráví ředitel.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

