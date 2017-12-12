---
title: 'Kurz: Azure Active Directory integrace s Atlassian cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Atlassian cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: 98623b6734726d43136703cbfee0b7ed0d82f815
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Kurz: Azure Active Directory integrace s Atlassian cloudu

V tomto kurzu zjistěte, jak integrovat Atlassian cloudu s Azure Active Directory (Azure AD).

Integrace Atlassian cloudu s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup do cloudu Atlassian.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Atlassian cloudu (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s cloudem Atlassian, potřebujete následující položky:

- Předplatné služby Azure AD
- Chcete-li povolit SAML jednotné přihlašování pro produkty Atlassian cloudu budete muset nastavit Identity Manager. Další informace o [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Atlassian cloudu z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Přidání Atlassian cloudu z Galerie
Při konfiguraci integrace Atlassian cloudu do služby Azure AD, potřebujete přidat Atlassian cloudu z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Atlassian cloudu z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Atlassian cloudu**, vyberte **Atlassian cloudu** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Atlassian cloudu v seznamu výsledků](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s cloudem Atlassian podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu Atlassian je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v cloudu Atlassian musí navázat.

V cloudu Atlassian přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atlassian cloudu, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s Atlassian Cloud](#create-an-atlassian-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon Atlassian cloudu, který je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Atlassian cloudu.

**Ke konfiguraci Azure AD jednotné přihlašování s Atlassian cloudu, proveďte následující kroky:**

1. Na portálu Azure na **Atlassian cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Na **Atlassian cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:

    ![Atlassian cloudové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. V **identifikátor** textovému poli, zadejte adresu URL:`https://auth.atlassian.com/saml/<unique ID>`
    
    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL:`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. V **předávání stavu** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.atlassian.net`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![Atlassian cloudové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    V **přihlašovací adresa URL textbox**, zadejte adresu URL, pomocí následujícího vzorce:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL. Tyto hodnoty můžete získat z obrazovky konfigurace SAML Atlassian cloudu, který je vysvětlen v dalších krocích tohoto kurzu.

5. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Aplikace Atlassian cloudu očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML. Ve výchozím nastavení je uživatelský identifikátor namapována na user.userprincipalname. Změňte a mapovat s **user.mail**. Můžete také další odpovídající hodnotu za instalaci vaší organizace, ale ve většině případů by měla fungovat e-mailu.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Na **konfigurace cloudu Atlassian** klikněte na tlačítko **konfigurace cloudu Atlassian** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**

    ![Konfigurace cloudu Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

9. Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přihlášení k portálu Atlassian pomocí oprávnění správce.

10. Přejděte na **Správa webu Atlassian** > **organizace a zabezpečení**. Pokud jste to ještě neudělali, vytvořte a název vaší organizace. V levém navigačním panelu, klikněte na tlačítko **domény**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

11. Vyberte způsob, jakým chcete ověřte svoji doménu - **DNS** nebo **HTTPS**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

12. Pro ověření DNS, vyberte **DNS** na kartě **domén** stránky a udělejte toto:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Klikněte na tlačítko **kopie** zkopírujte hodnotu pro vaše záznam TXT.

    b. Ze serveru DNS najděte stránku nastavení pro přidávání nového záznamu.

    c. Vyberte možnost pro přidávání nového záznamu a vložte hodnotu, kterou jste zkopírovali z **domén** stránky k **hodnotu** pole. DNS také odkaz k němu jako **odpovědí** nebo **popis**.

    d. Vaše záznam DNS může také zahrnovat následující pole:
    
    * **Typ záznamu**: Zadejte **TXT**
    * **Název nebo hostitele nebo Alias**: ponechte výchozí nastavení (@ nebo je prázdný)
    * **Doba provozu (TTL)**: Zadejte **86400**
    
    e.  Záznam uložte.

13. Vraťte se do **domén stránky** Správa organizace a klikněte na **ověřit doménu** tlačítko. Zadejte název domény v místní nabídce a klikněte na **ověřit doménu** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Může trvat až 72 hodin, než se projeví změny záznam TXT, vy nepoznáte hned zda ověření domény byla úspěšná. Zkontrolujte vaše **domén** stránky krátce po provedení těchto kroků pro váš stav ověření. Zobrazí následující obrazovka s aktualizovaný stav jako **ověřeno**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

14. Pro ověření protokol HTTPS, vyberte **HTTPS** na kartě **domén** stránky a proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Klikněte na tlačítko **stažení souboru** ke stažení souboru HTML.

    b.  Nahrajte soubor HTML do kořenového adresáře vaší domény.

15. Vraťte se do **domén** Správa organizace a klikněte na tlačítko **ověřit doménu** tlačítko. Zadejte vaše **název domény** v místní nabídce a klikněte na **ověřit doménu** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

16. Pokud proces ověření můžete vyhledat soubor, který jste nahráli do kořenového adresáře, stav domény aktualizace **ověřeno**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Další informace o ověření domény, najdete v části [na Atlassian domény ověření dokumentace](https://confluence.atlassian.com/cloud/domain-verification-873871234.html)

17. V levém navigačním panelu klikněte na **SAML jednotné přihlašování**. Pokud jste to ještě neudělali, přihlásit k odběru do Atlassian na Identity Manager.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

18. V **konfigurace přidat SAML** Dialogový pole, přidejte nastavení zprostředkovatele Identity následujícím způsobem:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. V **zprostředkovatele Identity Entity ID** textové pole, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.

    b. V **zprostředkovatele Identity URL jednotného přihlašování k** textové pole, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. Otevřete certifikát stažený z portálu Azure v programu Poznámkový blok, zkopírujte hodnoty bez řádky certifikát Begin a End certifikát a vložte jej do **X509 veřejný certifikát** pole.
    
    d. Klikněte na tlačítko **uložte konfiguraci**.
     
19. Aktualizujte nastavení služby Azure AD a ujistěte se, že jste nastavili správné adresy URL.
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopírování **SP Identity ID** z SAML obrazovky a vložte hodnotu v **identifikátor** pole na portálu Azure v cloudu Atlassian **domény a adresy URL** části.
    
    b. Kopírování **adresa URL služby SP Assertion příjemce** z SAML obrazovky a vložte hodnotu v **adresa URL odpovědi** pole na portálu Azure v cloudu Atlassian **domény a adresy URL** oddíl.
    
    c. Přihlašovací adresa URL je adresa URL klienta ve vašem cloudu Atlassian. 

    > [!NOTE]
    > Stávající zákazníky služby muset klikněte na **Ano, aktualizace konfigurace** po aktualizaci **SP Identity ID** a **adresa URL služby SP Assertion příjemce** hodnoty v portálu Azure. Nové zákazníky nepotřebují k provedení tohoto kroku. 
    
20. Na portálu Azure klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Vytvořit testovací uživatele s Atlassian cloudu

Pokud chcete povolit uživatelům Azure AD přihlášení do cloudu Atlassian, musí být zřízená do Atlassian cloudu. V případě cloudu Atlassian zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. V části Správa lokality, klikněte **uživatelé** tlačítko

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klikněte **pozvání uživatele** tlačítko pro vytvoření uživatele v cloudu Atlassian.

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Zadejte uživatele **e-mailovou adresu** a přiřadit přístup k aplikaci. 

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klikněte na tlačítko **uživatele pozvat** tlačítko odešle e-mailová pozvánka pro uživatele a uživatel se bude po přijetí pozvánky v systému aktivní. 

>[!NOTE] 
>Můžete také vytvořit uživatele hromadné kliknutím **vytvořit hromadné** tlačítko v části uživatelé.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup do cloudu Atlassian Azure jednotné přihlašování.

![Přiřadit role uživatele][200] 

**Přiřadit Britta Simon Atlassian cloudu, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Atlassian cloudu**.

    ![V seznamu aplikací na odkaz Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Atlassian cloudu na přístupovém panelu, jste měli získat automaticky přihlášení k Atlassian cloudové aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

