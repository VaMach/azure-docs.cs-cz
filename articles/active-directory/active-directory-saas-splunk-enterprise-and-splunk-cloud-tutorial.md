---
title: 'Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Splunk Enterprise a Splunk cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 82a7f8ec5be62848b45b23fce1e012aaad3b1798
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu

V tomto kurzu zjistěte, jak integrovat Splunk Enterprise a Splunk cloudu s Azure Active Directory (Azure AD).

Integrace Splunk Enterprise a Splunk cloudu s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, kdo má přístup k podnikové Splunk a Splunk cloudu
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Splunk Enterprise a Splunk cloudu jednotné přihlašování (SSO) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Splunk Enterprise a Splunk cloudu, potřebujete následující položky:

- Předplatné služby Azure AD
- Splunk Enterprise nebo jednotného přihlašování k cloudu Splunk povolené předplatné


>[!NOTE]
>K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
>

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Splunk Enterprise a Splunk cloudu z Galerie
2. Konfigurace a testování Azure AD SSO


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Přidat Splunk Enterprise a Splunk cloudu z Galerie
Při konfiguraci integrace Splunk Enterprise a Splunk cloudu do služby Azure AD, potřebujete přidat Splunk Enterprise a Splunk cloudu z Galerie si na seznam spravovaných aplikací SaaS.

**Chcete-li přidat Splunk Enterprise a Splunk cloudu z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.

    ![Active Directory][1]

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** v dolní části stránky.

    ![Aplikace][3]

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **Splunk Enterprise nebo Splunk Cloud**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. V podokně výsledků vyberte **Splunk Enterprise a cloudu Splunk**a potom klikněte na **Complete** tuto aplikaci přidat.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotného přihlašování k Splunk organizace a Splunk cloudu podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Splunk Enterprise a Splunk cloudu je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Splunk Enterprise a Splunk cloudu musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** Splunk Enterprise a Splunk cloudu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Splunk Enterprise a cloudu Splunk](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  – Pokud chcete mít protějšku Britta Simon ve Splunk podniku a Splunk cloudu, který je propojený s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování k portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Splunk Enterprise a Splunk cloudu.


**Ke konfiguraci Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, proveďte následující kroky:**

1. Na portálu classic na **Splunk Enterprise a cloudu Splunk** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
     
    ![Konfigurovat jednotné přihlašování][6] 

2. Na **jak chcete uživatelům se přihlásit Splunk Enterprise a cloudu Splunk** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace Splunk Enterprise a Splunk cloudu pomocí následujícího vzorce:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. V **identifikátor** textovému poli, zadejte adresu URL serveru Splunk.
  3. V **adresa URL odpovědi** textovému poli, zadejte adresu URL s vzoru následující:`https://<splunkserver>/saml/acs`
  4. Klikněte na **Další**.
 
4. Na **nakonfigurovat jednotné přihlašování v Splunk Enterprise a Splunk cloudu** proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači.
  2. Klikněte na **Další**.

5. Získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, obraťte se na Splunk Enterprise a tým podpory Splunk cloudu a uveďte s následujícími službami:

    * Stažené **federaton metadat**
6. Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
 
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele na portálu classic názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.

6.  Na **profil uživatele** dialogové okno proveďte následující kroky:
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. V **křestní jméno** textovému poli, typ **Britta**.  
  2. V **příjmení** textovému poli, typ, **Simon**.
  3. V **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Poznamenejte si hodnotu **nové heslo**.
  2. Klikněte na **Dokončit**.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Vytvoření Splunk Enterprise a Splunk cloudu testovacího uživatele

V této části vytvoříte uživatele volat Britta Simon ve Splunk podniku a Splunk cloudu. Spojte se s Splunk Enterprise a tým podpory Splunk cloudu, přidejte uživatele v platformě Splunk Enterprise a Splunk cloudu.


### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure SSOy udělení jeho přístupu Splunk Enterprise a Splunk cloudu.

![Přiřadit uživatele][200] 

**Přiřadit Britta Simon do firemní sítě Splunk a Splunk cloudu, proveďte následující kroky:**

1. Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Splunk Enterprise a Splunk cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. V nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203]

4. V seznamu uživatelů vyberte **Britta Simon**.

5. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.

    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části je otestovat vaše Azure AD SSOonfiguration pomocí přístupového panelu.

Když kliknete na tlačítko Splunk podniku a Splunk cloudu dlaždici na přístupovém panelu, jste měli získat automaticky přihlášení k Splunk Enterprise a Splunk cloudové aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
