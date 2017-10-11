---
title: 'Kurz: Azure Active Directory integrace s SAP HANA | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: a7e73f6ee763d1005ad85935cf2d8f6b24ecf116
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Kurz: Azure Active Directory integrace s SAP HANA

V tomto kurzu zjistěte, jak integrovat SAP HANA s Azure Active Directory (Azure AD).

Integrace SAP HANA s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k SAP HANA
- Můžete povolit uživatelům, aby automaticky získat přihlášení k SAP HANA (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SAP HANA, potřebujete následující položky:

- Předplatné služby Azure AD
- SAP HANA jednotné přihlašování povolené předplatné
- Spuštěné HANA Instance buď na všechny veřejné IaaS, místní, virtuální počítače Azure nebo velké instance SAP v Azure
- Na rozhraní webové správy XSA a také HANA Studio instalovaného na instanci HANA.

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí SAP HANA. Nejdřív otestovat integrace v vývoj nebo pracovní prostředí aplikace a pak pomocí produkčního prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP HANA z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sap-hana-from-the-gallery"></a>Přidání SAP HANA z Galerie
Při konfiguraci integrace SAP HANA do služby Azure AD potřebujete přidat SAP HANA z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat SAP HANA z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **SAP HANA**, vyberte **SAP HANA** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci. 

    ![Nová aplikace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP HANA podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP HANA je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v SAP HANA.

V SAP HANA přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP HANA, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SAP HANA](#creating-a-sap-hana-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP HANA propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA.

**Ke konfiguraci Azure AD jednotné přihlašování s SAP HANA, proveďte následující kroky:**

1. Na portálu Azure na **SAP HANA** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Na **SAP HANA domény a adresy URL** část, proveďte následující kroky:

    ![Domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. V **identifikátor** textovému poli, typ jako:`HA100` 

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi. Obraťte se na [tým podpory SAP HANA klienta](https://cloudplatform.sap.com/contact.html) k získání těchto hodnot. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Pokud certifikát není aktivní pak nastavte ji jako aktivní kliknutím na zaškrtávací políčko "Aktivujte nový certifikát" ve službě Azure AD. 

5. Aplikace SAP HANA očekává SAML kontrolní výrazy ve specifickém formátu. Následující snímek obrazovky ukazuje příklad pro tento. Zde jsme jsou namapovány **uživatelský identifikátor** s **ExtractMailPrefix()** funkce **user.mail**. Díky tomu hodnotu předpony e-mailů uživatele, který je jedinečné ID uživatele. Ta se odešle do aplikace SAP HANA v každé úspěšné odpovědi.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno:

    a. V **uživatelský identifikátor** rozevíracího seznamu vyberte **ExtractMailPrefix**.
    
    b. V **e-mailu** rozevíracího seznamu vyberte **user.mail**.

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Konfigurace jednotného přihlašování na **SAP HANA** straně, přihlášení k vaší **HANA XSA webové konzole** procházením příslušných koncový bod HTTPS.

    > [!Note]
    > Ve výchozí konfiguraci adresa URL žádost přesměruje na přihlašovací obrazovka, která vyžaduje přihlašovací údaje ověřeného uživatele databáze SAP HANA dokončete proces přihlášení. Přihlášení uživatele musí mít oprávnění nezbytná k provádění úloh správy SAML.

9. V XSA webové rozhraní, přejděte na **poskytovatele Identity SAML** a z ní, klikněte na tlačítko **"+"** – tlačítko v dolní části obrazovky zobrazit v podokně Přidat informace o poskytovateli Identity a proveďte následující kroky pomocí následujících kroků:

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. V **přidat informace o poskytovateli Identity** podokně Vložit obsah XML metadat, který jste si stáhli z portálu Azure do **Metadata** textové pole.

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Pokud obsah dokumentu XML platný, proces analýzy extrahuje informace požadované pro vložení do **předmět, Entity ID a vystavitele** pole v oblasti obrazovky obecné dat a pole adresy URL na obrazovce cílové oblasti, například  **základní adresu URL a adresy URL SingleSignOn (*)**.

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Do pole název oblasti dat obecné obrazovce zadejte název nového poskytovatele identity SAML jednotné přihlašování.

    > [!Note]
    > Název rozšíření IDP SAML je povinná a musí být jedinečné; Zobrazí se v seznamu dostupných IDPs SAML, který se zobrazí, pokud vyberete jako metodu ověřování pro aplikace SAP HANA XS chcete použít, například v oblasti ověřování obrazovky nástroje správy Křížky artefaktů SAML.

10. Uložte podrobné informace o nové poskytovatele identity SAML. Zvolte **Uložit** uložit podrobnosti o poskytovatele identity SAML a přidat nové rozšíření IDP SAML do seznamu známých IDPs SAML.

    ![Tlačítko Uložit](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. V sadě HANA Studio ve vlastnostech systému **konfigurace** kartě, právě filtrovat nastavení **saml** a upravit **assertion_timeout** z **10 sekund**  k **120 sekundu**.

    ![nastavení assertion_timeout](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Tlačítko Přidat](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Dialogové okno uživatele](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sap-hana-test-user"></a>Vytvoření zkušebního uživatele SAP HANA

Pokud chcete povolit uživatelům Azure AD přihlášení k SAP HANA, musí být zřízená do SAP HANA.
SAP HANA podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Pokud potřebujete ručně vytvořit uživatele, proveďte následující kroky:

>[!Note]
>Externí ověřování použité uživatele, můžete změnit.
Externí uživatelé se ověřují pomocí externího systému, například systém protokolu Kerberos. Podrobné informace o externí identity, kontaktujte vašeho [správce domény](https://cloudplatform.sap.com/contact.html).

1. Otevřete [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) jako správce a povolení uživatele databáze pro jednotné přihlašování SAML.

    ![Vytvoření uživatele](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Osové neviditelná zaškrtávací políčko nalevo od **SAML** a pomocí odkazu konfigurace.

3. Klikněte na tlačítko **přidat** přidat rozšíření IDP SAML a klikněte na tlačítko **OK** výběrem příslušné rozšíření IDP SAML.

4. Přidat **externí Identity** (např. Zde BrittaSimon), nebo zvolte **"Žádné"** a klikněte na tlačítko **OK**.

    >[!Note]
    >Pokud není zaškrtnuto políčko "Žádné" zaškrtávací políčko, pak uživatelské jméno v HANA musí přesně shodovat s názvem uživatele v hlavní název uživatele před příponu domény (tj. BrittaSimon@contoso.com by se stala BrittaSimon v HANA).

5. Pro účely testování, přiřaďte všechny **"XS"** role pro uživatele.

    ![přiřazení rolí](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Tato oprávnění, které jsou vhodné pro vaše případy použití, jenom musíte získat.

6. Uložte uživatele.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k SAP HANA.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon SAP HANA, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SAP HANA**.

    ![Přiřadit uživatele](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici SAP HANA na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SAP HANA.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

