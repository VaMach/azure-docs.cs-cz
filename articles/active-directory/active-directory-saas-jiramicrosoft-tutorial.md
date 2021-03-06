---
title: "Kurz: Azure Active Directory integrace s Microsoft Azure Active Directory jednotné přihlašování pro JIRA | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Microsoft Azure Active Directory jednotné přihlašování pro JIRA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: jeedes
ms.openlocfilehash: 710aa59fb3cc69cb1f5a20389eca13b1be93d223
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-azure-active-directory-integration-with-microsoft-azure-active-directory-single-sign-on-for-jira"></a>Kurz: Azure Active Directory integrace s Microsoft Azure Active Directory jednotné přihlašování pro JIRA

V tomto kurzu zjistěte, jak integrovat Microsoft Azure Active Directory jednotné přihlašování pro JIRA s Azure Active Directory (Azure AD).

Integrace služby Microsoft Azure Active Directory jednotné přihlašování pro JIRA s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k Microsoft Azure Active Directory jednotné přihlašování pro JIRA.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Microsoft Azure Active Directory jednotné přihlašování pro JIRA (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Popis

Pomocí účtu Microsoft Azure Active Directory se serverem Atlassian JIRA umožňující jednotného přihlašování. Tímto způsobem všichni uživatelé vaší organizace slouží k přihlášení do aplikace JIRA přihlašovací údaje Azure AD. Tento modul plug-in používá SAML 2.0 pro federaci.

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Microsoft Azure Active Directory jednotné přihlašování pro JIRA, potřebujete následující položky:

- Předplatné služby Azure AD
- JIRA jádra a 6.0 softwaru do 7.2.0 nebo JIRA služby podpory 3.0 3.2 by měl nainstalováno a nakonfigurováno na verzi Windows 64-bit
- JIRA server je povolen protokol HTTPS
- Všimněte si, že podporované verze pro modul plug-in JIRA jsou uvedené v následující části.
- JIRA server je dostupný na Internetu, zvláště na Azure AD přihlašovací stránku pro ověřování a měli schopný přijímat tokenu z Azure AD
- Přihlašovací údaje správce se nastavují v JIRA
- V JIRA je zakázáno WebSudo
- Testovací uživatel vytvořené v aplikaci JIRA server

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí JIRA. Nejdřív otestovat integrace v vývoj nebo pracovní prostředí aplikace a pak pomocí produkčního prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>Podporované verze systému JIRA

*   Základní JIRA a Software: 6.0 na 7.2.0
*   Služby podpory JIRA 3.0 k 3.2

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Microsoft Azure Active Directory jednotné přihlašování pro JIRA z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-microsoft-azure-active-directory-single-sign-on-for-jira-from-the-gallery"></a>Přidání Microsoft Azure Active Directory jednotné přihlašování pro JIRA z Galerie
Při konfiguraci integrace služby Microsoft Azure Active Directory jednotné přihlašování pro JIRA do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS Microsoft Azure Active Directory jednotné přihlašování pro JIRA z galerie.

**Pokud chcete přidat Microsoft Azure Active Directory jednotné přihlašování pro JIRA z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Microsoft Azure Active Directory jednotné přihlašování pro JIRA**, vyberte **Microsoft Azure Active Directory jednotné přihlašování pro JIRA** z panelu výsledků klikněte **přidat**  tlačítko Přidat aplikaci.

    ![Microsoft Azure Active Directory jednotné přihlašování pro JIRA v seznamu výsledků](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování s Microsoft Azure Active Directory jednotné přihlašování pro JIRA podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Microsoft Azure Active Directory jednotné přihlašování pro JIRA je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v Microsoft Azure Active Directory jednotné přihlašování pro JIRA musí navázat.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Microsoft Azure Active Directory jednotné přihlašování pro JIRA, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření Microsoft Azure Active Directory jednotné přihlašování pro testovací uživatele JIRA](#create-a-microsoft-azure-active-directory-single-sign-on-for-jira-test-user)**  – Pokud chcete mít protějšek Britta Simon v Microsoft Azure Active Directory jednotné přihlašování pro JIRA propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší služby Microsoft Azure Active Directory jednotné přihlašování pro aplikace JIRA.

**Ke konfiguraci Azure AD jednotné přihlašování s Microsoft Azure Active Directory jednotné přihlašování pro JIRA, proveďte následující kroky:**

1. Na portálu Azure na **Microsoft Azure Active Directory jednotné přihlašování pro JIRA** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_samlbase.png)

3. Na **Microsoft Azure Active Directory jednotné přihlašování JIRA domény a adresy URL** část, proveďte následující kroky:

    ![Microsoft Azure Active Directory jednotné přihlašování JIRA domény a adresy URL jeden přihlašování informace](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/`

    c. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL. Port je volitelný, v případě, že je adresa URL s názvem. Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Jira, který je vysvětlen později v tomto kurzu.
 
4. Ke generování **Metadata** adresu url, proveďte následující kroky:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\endpointicon.png)

    c. Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\endpoint.png)
     
    d. Nyní přejděte na stránku vlastností **Microsoft Azure Active Directory jednotné přihlašování pro JIRA** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\appid.png)

    e. Vygenerovat **adresu URL metadat** pomocí následujícího vzorce: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` a zkopírujte tuto hodnotu v poznámkovém bloku, jak se později používá pro konfiguraci modulu plug-in.

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_400.png)

6. V okně prohlížeče jiný web Přihlaste se k instanci JIRA jako správce.

7. Pozastavte ukazatel myši na ikonu a klikněte na **doplňky**.
    
    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\addon1.png)

8. Stáhnout modul plug-in z [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56506). Ručně odeslat modulu plug-in poskytnout pomocí Microsoft **nahrát rozšíření** nabídky.

    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\addon12.png)

9. Po instalaci modulu plug-in se zobrazí v **uživatel nainstaloval** doplňky části **spravovat rozšíření** části. Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.

    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\addon13.png)

10. Proveďte následující kroky na stránce konfigurace:

    ![Konfigurovat jednotné přihlašování](.\media\active-directory-saas-msaadssojira-tutorial\addon52.png)

    > [!TIP]
    > Zkontrolujte, zda je namapovaný na aplikaci tak, aby se nezobrazí žádná chyba při řešení metadata jen jeden certifikát. Pokud máte víc certifikátů, při řešení metadata, získá správce k chybě.
 
    a. V **adresu URL metadat** vložit **adresu URL metadat** generované z Azure AD a klikněte na tlačítko **vyřešit** tlačítko. Přečte adresu URL metadat IdP a naplní všechny informace o pole.

    b. Kopírování **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložte je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textových polí v uvedeném pořadí v **Microsoft Azure Active Directory jednotné přihlašování JIRA domény a adresy URL** části na portálu Azure.

    c. V **přihlašovací jméno tlačítko** zadejte název vaší organizace chce, aby se uživatelům zobrazit na přihlašovací obrazovku tlačítka.

    d. V **SAML uživatele ID umístění** vyberte buď **ID uživatele je v elementu NameIdentifier příkaz subjektu** nebo **ID uživatele je v elementu atribut**.  Toto ID musí být JIRA id uživatele. Pokud neodpovídá id uživatele, systém nedovolí přihlášení uživatelů. 

    > [!Note]
    > Výchozí umístění SAML ID uživatele je identifikátor jména. Můžete to změnit na možnost atribut a zadejte odpovídající název.

    e. Pokud vyberete **ID uživatele je v elementu atribut** možnost, pak v **název atributu** textového pole zadejte název atributu, kde je očekávána Id uživatele. 

    f. Pokud používáte federované domény (například služby AD FS atd.) s Azure AD, potom klikněte na **povolit zjišťování domovské sféry** řádku a nakonfigurovat **název domény**.
    
    g. V **název domény** zadejte název domény zde v případě přihlášení na základě služby AD FS.

    h. Zkontrolujte **povolit jednotné přihlašování se** Pokud se chcete odhlásit z Azure AD, když se uživatel neodhlásí z JIRA. 

    i. Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](.\media\active-directory-saas-msaadssojira-tutorial\create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-microsoft-azure-active-directory-single-sign-on-for-jira-test-user"></a>Vytvoření Microsoft Azure Active Directory jednotné přihlašování pro JIRA testovacího uživatele

Pokud chcete povolit uživatelům Azure AD přihlášení k JIRA místní server, musí být zřízená do Microsoft Azure Active Directory jednotné přihlašování pro JIRA. Pro Microsoft Azure Active Directory jednotné přihlašování pro JIRA zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k serveru na místní JIRA jako správce.

2. Pozastavte ukazatel myši na ikonu a klikněte na **Správa uživatelů**.

    ![Můžete přidat zaměstnance](.\media\active-directory-saas-msaadssojira-tutorial\user1.png) 

3. Budete přesměrováni na stránku přístup správce k zadání **heslo** a klikněte na tlačítko **potvrdit** tlačítko.

    ![Můžete přidat zaměstnance](.\media\active-directory-saas-msaadssojira-tutorial\user2.png) 

4. V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživatele**.

    ![Můžete přidat zaměstnance](.\media\active-directory-saas-msaadssojira-tutorial\user3.png) 

5. Na **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky:

    ![Můžete přidat zaměstnance](.\media\active-directory-saas-msaadssojira-tutorial\user4.png) 

    a. V **e-mailová adresa** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    b. V **úplný název** textovému poli, úplný název typu uživatele jako Britta Simon.

    c. V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.

    d. V **heslo** textovému poli, zadejte heslo uživatele.

    e. Klikněte na tlačítko **vytvořit uživateli**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k Microsoft Azure Active Directory jednotné přihlašování pro JIRA Azure jednotné přihlašování.

![Přiřadit role uživatele][200] 

**Britta Simon přiřadit k Microsoft Azure Active Directory jednotné přihlašování pro JIRA, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Microsoft Azure Active Directory jednotné přihlašování pro JIRA**.

    ![Microsoft Azure Active Directory jednotné přihlašování pro JIRA odkaz v seznamu aplikací](.\media\active-directory-saas-msaadssojira-tutorial\tutorial_singlesign-onforjira_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete Microsoft Azure Active Directory jednotné přihlašování pro dlaždici JIRA na přístupovém panelu, můžete by měl získat automaticky přihlášení k vaší služby Microsoft Azure Active Directory jednotné přihlašování pro aplikace JIRA.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_01.png
[2]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_02.png
[3]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_03.png
[4]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_04.png

[100]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_100.png

[200]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_200.png
[201]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_201.png
[202]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_202.png
[203]: .\media\active-directory-saas-msaadssojira-tutorial\tutorial_general_203.png
