---
title: "Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SilkRoad životnosti Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite
Cílem tohoto kurzu je ukazují, jak integrovat SilkRoad životnosti Suite s Azure Active Directory (Azure AD). 

Integrace s Azure AD SilkRoad životnosti Suite nabízí následující výhody: 

* Můžete řídit ve službě Azure AD, kdo má přístup k SilkRoad životnosti Suite 
* Můžete povolit uživatelům, aby automaticky získat přihlášení k SilkRoad životnosti Suite jednotné přihlašování (SSO) s jejich účty Azure AD

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Konfigurace Azure AD integrace se sadou Suite životnosti SilkRoad, potřebujete následující položky:

* Předplatné služby Azure AD
* Předplatné SilkRoad životnosti Suite jednotné přihlašování povoleno

>[!NOTE]
>K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní Azure AD jednotného přihlašování k testování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SilkRoad životnosti Suite z Galerie 
2. Konfigurace a testování Azure AD SSO

## <a name="add-silkroad-life-suite-from-the-gallery"></a>Přidat Suite životnosti SilkRoad z Galerie
Při konfiguraci integrace sady životnosti SilkRoad do služby Azure AD, potřebujete přidat Suite životnosti SilkRoad z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Suite životnosti SilkRoad z galerie, postupujte takto:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 
   
    ![Active Directory][1]

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Aplikace][3]

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **SilkRoad životnosti Suite**.
   
    ![Aplikace][5]

7. V podokně výsledků vyberte **Suite životnosti SilkRoad**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Aplikace][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD přihlášení SSO se sada životnosti SilkRoad podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v sadě životnosti SilkRoad pro uživatele ve službě Azure AD je Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v sadě životnosti SilkRoad musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v sadě SilkRoad životnosti.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SilkRoad životnosti Suite, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Suite životnosti SilkRoad](#creating-a-silkroad-life-suite-test-user)**  – Pokud chcete mít protějšek Britta Simon životnosti sady SilkRoad, který je propojený s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování
Cílem této části je k povolení přihlášení SSO Azure AD na portálu Azure classic a nakonfigurovat jednotné přihlašování v aplikaci SilkRoad životnosti Suite.

**Ke konfiguraci Azure AD jednotné přihlašování s SilkRoad životnosti Suite, proveďte následující kroky:**

1. Přihlašování k webu společnosti SilkRoad jako správce. 

  >[!NOTE] 
  > Pokud chcete získat přístup k aplikaci SilkRoad životnosti Suite ověřování pro konfiguraci federační služby Microsoft Azure AD, kontaktujte prosím podporu SilkRoad nebo vaším zástupcem SilkRoad služby.
  > 

2. Přejděte na **poskytovatele služeb**a potom klikněte na **Federation podrobnosti**. 
   
    ![Azure AD jednotné přihlášení][10] 

3. Klikněte na tlačítko **stáhnout federačních metadat**a potom uložte soubor metadat ve vašem počítači.
   
    ![Azure AD jednotné přihlášení][11] 

4. Na portálu Azure classic na **Suite životnosti SilkRoad** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 

5. Na **jak chcete uživatelům se přihlásit SilkRoad životnosti Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][7] 

6. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
   
    ![Azure AD jednotné přihlášení][8]   
 1. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlašování na váš web Suite životnosti SilkRoad (např: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Otevřete stažené **Silkroad** soubor metadat. 
 3. Vyhledejte **AssertionConsumerService** označení a poté zkopírujte **umístění** atribut.         
   
    ![Azure AD jednotné přihlášení][21] 
 4. Vložte hodnotu do **adresa URL odpovědi** textové pole.  
 5. Klikněte na **Další**.

6. Na **nakonfigurovat jednotné přihlašování v SilkRoad životnosti Suite** proveďte následující kroky:
   
    ![Azure AD jednotné přihlášení][9]  
 1. Klikněte na možnost stažení certifikátu a potom uložte soubor v počítači.  
 2. Klikněte na **Další**.

7. Ve vaší **SilkRoad** aplikace, klikněte na tlačítko **ověřování zdrojů**.
   
    ![Azure AD jednotné přihlášení][12] 

8. Klikněte na tlačítko **přidat zdroj ověřování**. 
   
    ![Azure AD jednotné přihlášení][13] 

9. V **přidat zdroj ověřování** část, proveďte následující kroky: 
   
    ![Azure AD jednotné přihlášení][14]  
 1. V části **možnost 2 – soubor metadat**, klikněte na tlačítko **Procházet** nahrát soubor stažený metadat.  
 2. Klikněte na tlačítko **vytvořit zprostředkovatelů Identity pomocí dat souboru**.

10. V **ověřování zdrojů** klikněte na tlačítko **upravit**. 
    
     ![Azure AD jednotné přihlášení][15] 

11. Na **upravit zdroj ověřování** dialogové okno, proveďte následující kroky: 
    
     ![Azure AD jednotné přihlášení][16] 
 1. Jako **povoleno**, vyberte **Ano**.   
 2. V **IdP popis** textovému poli, zadejte popis pro konfiguraci (například: *Azure AD jednotného přihlašování k*).  
 3. V **IdP název** textovému poli, zadejte název, která je specifická pro konfiguraci (například: *Azure SP*).  
 4. Klikněte na **Uložit**.

12. Zakažte všechny ostatní zdroje ověřování. 
    
     ![Azure AD jednotné přihlášení][17]

13. Na portálu Azure classic na **jednotné přihlašování potvrzení** klikněte na tlačítko **Další**.  
    
     ![Azure AD jednotné přihlášení][18]

14. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
    
     ![Azure AD jednotné přihlášení][19]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.  
 2. V uživatelské jméno **textbox**, typ **BrittaSimon**. 
 3. Klikněte na **Další**.

6. Na **profil uživatele** dialogové okno proveďte následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. V **křestní jméno** textovému poli, typ **Britta**.    
 2. V **příjmení** textovému poli, typ, **Simon**. 
 3. V **zobrazovaný název** textovému poli, typ **Britta Simon**. 
 4. V **Role** seznamu, vyberte **uživatele**.
 5. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Poznamenejte si hodnotu **nové heslo**. 
 2. Klikněte na **Dokončit**.   

### <a name="create-a-silkroad-life-suite-test-user"></a>Vytvoření zkušebního uživatele SilkRoad životnosti Suite
Cílem této části je vytvoření uživatele názvem Britta Simon v sadě SilkRoad životnosti. Na Britta musí mít ID jednotné přihlašování (někdy označovány jako *AuthParam*) odpovídající na Britta **emailaddress** ve službě Azure AD.

**Vytvoření uživatele volat Britta Simon v sadě životnosti SilkRoad, proveďte následující kroky:**

- Požádejte váš tým podpory SilkRoad životnosti Suite pro vytvoření uživatele, která má jako **jednotného přihlašování k ID** atribut stejnou hodnotu jako **emailaddress** z Britta Simon ve službě Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD
Cílem této části je umožnit Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k SilkRoad životnosti Suite.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon SilkRoad životnosti Suite, proveďte následující kroky:**

1. Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
   
    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SilkRoad životnosti Suite**.
   
    ![Přiřadit uživatele][202] 

3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 

4. V seznamu uživatelů vyberte **Britta Simon**.

5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.  

Když kliknete na dlaždici SilkRoad životnosti Suite na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SilkRoad životnosti Suite.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





