---
title: 'Kurz: Azure Active Directory integrace s SuccessFactors | Microsoft Docs'
description: "Další informace o použití SuccessFactors s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Kurz: Azure Active Directory integrace s SuccessFactors
Cílem tohoto kurzu je ukazují, jak integrovat SuccessFactors s Azure Active Directory (Azure AD).

Integrace SuccessFactors s Azure AD poskytuje následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup k SuccessFactors
* Můžete povolit uživatelům, aby automaticky získat přihlášení k SuccessFactors (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Konfigurace integrace Azure AD s SuccessFactors, potřebujete následující položky:

* Platné předplatné Azure
* Klienta v SuccessFactors

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

1. Přidání SuccessFactors z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-successfactors-from-the-gallery"></a>Přidání SuccessFactors z Galerie
Při konfiguraci integrace SuccessFactors do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS SuccessFactors z galerie.

**Pokud chcete přidat SuccessFactors z galerie, proveďte následující kroky:**

1. V portálu Azure classic, na levém navigačním panelu klikněte na **služby Active Directory**.
   
    ![Konfigurace jednotného přihlašování][1]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Konfigurace jednotného přihlašování][2]
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Konfigurace jednotného přihlašování][4]
6. V **vyhledávacího pole**, typ **SuccessFactors**.
   
    ![Konfigurace jednotného přihlašování][5]
7. Na panelu výsledků vyberte **SuccessFactors**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Konfigurace jednotného přihlašování][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s SuccessFactors podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v SuccessFactors pro uživatele ve službě Azure AD je Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SuccessFactors musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SuccessFactors.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SuccessFactors, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SuccessFactors](#creating-a-successfactors-test-user)**  – Pokud chcete mít protějšek Britta Simon v SuccessFactors propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování
V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci SuccessFactors.

**Ke konfiguraci Azure AD jednotné přihlašování s SuccessFactors, proveďte následující kroky:**

1. Na portálu Azure classic na **SuccessFactors** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurace jednotného přihlašování][7]
2. Na **jak chcete uživatelům se přihlásit SuccessFactors** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurace jednotného přihlašování][8]
3. Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.
   
    ![Konfigurace jednotného přihlašování][9]
   
    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí jedné z následujících kritérií: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí jedné z následujících kritérií: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Klikněte na **Další**. 

    > [!NOTE]
    > Upozorňujeme, že tyto nejsou skutečné hodnoty. Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a adresa URL odpovědi. Chcete-li získat tyto hodnoty, obraťte se na [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).

1. Na **nakonfigurovat jednotné přihlašování v SuccessFactors** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu místně na vašem počítači.
   
    ![Konfigurace jednotného přihlašování][10]

2. V okně prohlížeče jiný web, přihlaste se k vaší **portál pro správu SuccessFactors** jako správce.

3. Navštivte **zabezpečení aplikací** a nativní pro **jeden znak na funkci**. 

4. Umístění všechny hodnoty v **resetovat tokenu** a klikněte na tlačítko **uložit tokenu** umožňující jednotného přihlašování SAML.
   
    ![Konfigurace jednotného přihlašování na straně aplikace][11]

    > [!NOTE] 
    > Tato hodnota se právě používá jako přepínač zapnout nebo vypnout. Pokud je uloženo žádnou hodnotu, jednotné přihlašování SAML je ON. Pokud je uloženo na prázdnou hodnotu jednotné přihlašování SAML je VYPNUTÝ.

1. Nativní na následující snímek obrazovky a proveďte následující akce.
   
    ![Konfigurace jednotného přihlašování na straně aplikace][12]
   
    a. Vyberte **jednotné přihlašování SAML v2** přepínač
   
    b. Nastavte Name(e.g. SAml issuer + company name) strany potvrzující SAML.
   
    c. V **SAML vystavitele** textbox vložte hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.
   
    d. Vyberte **odpovědi (zákazníka generované nebo IdP/přístupový bod)** jako **vyžadují povinné podpis**.
   
    e. Vyberte **povoleno** jako **povolit SAML příznak**.
   
    f. Vyberte **ne** jako **podpisu požadavku přihlášení (SF generované/SP/RP)**.
   
    g. Vyberte **profil prohlížeče nebo Pozálohovacího** jako **SAML profil**.
   
    h. Vyberte **ne** jako **vynutit období platný certifikát**.
   
    i. Kopírovat obsah souboru stažený certifikát a vložte ji do **ověření certifikátu SAML** textové pole.

    > [!NOTE] 
    > Obsahu certifikátu musí začínat certifikátu a koncovou značkou certifikátu.

1. Přejděte do SAML V2 a pak proveďte následující kroky:
   
    ![Konfigurace jednotného přihlašování na straně aplikace][13]
   
    a. Vyberte **Ano** jako **podporu spouštěná SP globální odhlášení**.
   
    b. V **globální odhlášení adresa URL služby (LogoutRequest cíl)** textbox vložte hodnotu **vzdálené adresy URL odhlašovací** z Průvodce konfigurací aplikace Azure AD.
   
    c. Vyberte **ne** jako **vyžadují sp musí zašifrovat všechny element NameID**.
   
    d. Vyberte **neurčené** jako **NameID formátu**.
   
    e. Vyberte **Ano** jako **povolit sp iniciované přihlášení (AuthnRequest)**.
   
    f. V **odeslán požadavek na jako vydavatel společnosti** textbox vložte hodnotu **vzdálené adresy URL pro přihlášení** z Průvodce konfigurací aplikace Azure AD.
2. Proveďte tyto kroky, pokud chcete, aby uživatelská jména přihlášení malá a velká písmena,.
   
    a. Navštivte **nastavení společnosti**(u dolního).
   
    b. Zaškrtněte políčko téměř **povolit uživatelské jméno bez rozlišování**.
   
    c.Click **Uložit**.
   
    ![Konfigurovat jednotné přihlašování][29]

    > [!NOTE] 
    > Pokud se pokusíte povolit tuto funkci, systém zkontroluje, pokud vytvoří duplicitní SAML přihlašovací jméno. Například pokud zákazník má uživatelských jmen uživatel1 a user1. Pořízení rychle rozlišování umožňuje tyto duplikáty. Systém vám poskytne chybovou zprávu a neumožní funkci. Zákazník muset změnit jeden z uživatelských jménech, je napsaný ve skutečnosti jiný. 

1. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Aplikace][14]
2. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Aplikace][15]

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][16]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD][17]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD][18]
4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD][19]
5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][20]
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.
6. Na **profil uživatele** dialogové okno proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][21]
   
    a. V **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V **příjmení** textovému poli, typ, **Simon**.
   
    c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V **Role** seznamu, vyberte **uživatele**.
   
    e. Klikněte na **Další**.
7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD][22]
8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][23]
   
    a. Poznamenejte si hodnotu **nové heslo**.
   
    b. Klikněte na **Dokončit**.  

### <a name="creating-a-successfactors-test-user"></a>Vytvoření zkušebního uživatele SuccessFactors
Pokud chcete povolit uživatelům Azure AD přihlášení do SuccessFactors, musí být zřízená do SuccessFactors.  
V případě SuccessFactors zřizování je ruční úloha.

Chcete-li získat uživatelé vytvoření ve SuccessFactors, budete muset kontaktovat [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD
Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k SuccessFactors Azure jednotné přihlašování.

![Přiřadit uživatele][24]

**Pokud chcete přiřadit Britta Simon SuccessFactors, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][25]
2. V seznamu aplikací vyberte **SuccessFactors**.
   
    ![Konfigurovat jednotné přihlašování][26]
3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][27]
4. V seznamu uživatelů vyberte **Britta Simon**.
5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][28]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.

Když kliknete na dlaždici SuccessFactors na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SuccessFactors.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
