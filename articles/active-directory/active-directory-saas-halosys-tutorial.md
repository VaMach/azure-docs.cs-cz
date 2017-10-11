---
title: 'Kurz: Azure Active Directory integrace s Halosys | Microsoft Docs'
description: "Další informace o použití Halosys s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18c5cd8eb4ca211f8ae2b8dd994c0e8c48625a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Kurz: Azure Active Directory integrace s Halosys

V tomto kurzu zjistěte, jak integrovat Halosys s Azure Active Directory (Azure AD).

Integrace Halosys s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Halosys
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Halosys (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Halosys, potřebujete následující položky:

- Předplatné služby Azure AD
- Halosys jednotného přihlašování povolené předplatné


> [!NOTE] 
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Halosys z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-halosys-from-the-gallery"></a>Přidání Halosys z Galerie
Při konfiguraci integrace Halosys do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Halosys z galerie.

**Pokud chcete přidat Halosys z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.

    ![Active Directory][1]
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** v dolní části stránky.

    ![Aplikace][3]

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **Halosys**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. V podokně výsledků vyberte **Halosys**a potom klikněte na **Complete** tuto aplikaci přidat.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Halosys je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Halosys musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Halosys.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halosys, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Halosys](#creating-a-halosys-test-user)**  – Pokud chcete mít protějšek Britta Simon v Halosys propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Halosys.


**Ke konfiguraci Azure AD jednotné přihlašování s Halosys, proveďte následující kroky:**

1. Na portálu classic na **Halosys** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
     
    ![Konfigurovat jednotné přihlašování][6] 

2. Na **jak chcete uživatelům se přihlásit Halosys** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace Halosys pomocí následujícího vzorce: `https://<company-name>.Halosys.com/client-api/api`.

    b.In **identifikátoru adresy URL** textovému poli, zadejte adresu URL v následujících vzoru: `https://<company-name>.Halosys.com`.   
         
4. Na **nakonfigurovat jednotné přihlašování v Halosys** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Halosys a poskytněte s následujícími službami:

    • Stažené **soubor metadat**
    
    • **URL jednotné přihlašování SAML**
    

6. Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
V této části vytvoříte testovacího uživatele na portálu classic názvem Britta Simon.


![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. Na **Povězte nám o tohoto uživatele** dialogové okno stránky, proveďte následující kroky: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.

    c. Klikněte na **Další**.

6.  Na **profil uživatele** dialogové okno stránky, proveďte následující kroky: ![vytváření testovací uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. V **křestní jméno** textovému poli, typ **Britta**.  

    b. V **příjmení** textovému poli, typ, **Simon**.

    c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.

    d. V **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Poznamenejte si hodnotu **nové heslo**.

    b. Klikněte na **Dokončit**.   



### <a name="creating-a-halosys-test-user"></a>Vytvoření zkušebního uživatele Halosys

V této části vytvoříte volal Britta Simon v Halosys uživatele. Spojte se s Halosys tým podpory pro přidání uživatele v platformě Halosys.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k Halosys Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Halosys, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Halosys**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203]

4. V seznamu uživatelů vyberte **Britta Simon**.

5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.

    ![Přiřadit uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Halosys na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Halosys.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
