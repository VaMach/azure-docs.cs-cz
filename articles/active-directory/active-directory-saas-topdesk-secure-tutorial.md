---
title: "Kurz: Azure Active Directory integrace s TOPdesk - zabezpečené | Microsoft Docs"
description: "Další informace o použití TOPdesk - zabezpečit pomocí služby Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Kurz: Azure Active Directory integrace s TOPdesk - zabezpečení
Cílem tohoto kurzu je zobrazit integraci Azure a TOPdesk - zabezpečené.  
Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* A TOPdesk - zabezpečené jednotné přihlašování povolené předplatné

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace v TOPdesk - zabezpečené podnikové lokality (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, který jste přiřadili k TOPdesk - zabezpečené [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikace pro TOPdesk - zabezpečení
2. Konfigurace jednotného přihlašování
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "scénář")

## <a name="enabling-the-application-integration-for-topdesk---secure"></a>Povolení integrace aplikace pro TOPdesk - zabezpečení
Cílem této části se popisují postup povolení integrace aplikace pro TOPdesk - zabezpečené.

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace pro TOPdesk – zabezpečení, proveďte následující kroky:
1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "služby Active Directory")

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "aplikace")

4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Přidat aplikaci](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "přidat aplikaci")

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "přidat aplikaci z gallerry")

6. V **vyhledávacího pole**, typ **TOPdesk - zabezpečené**.
   
    ![Galerie aplikací](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "galerii aplikací")

7. V podokně výsledků vyberte **TOPdesk - zabezpečené**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![TOPdesk - zabezpečené](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - zabezpečení")

## <a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
Cílem této části se popisují, jak uživatelům povolit ověřování na TOPdesk - zabezpečit pomocí svého účtu ve službě Azure AD využívající federaci na základě protokolu SAML.  
Konfigurace jednotného přihlašování pro TOPdesk – zabezpečení vyžaduje, abyste nahrát soubor logo ikonu. Chcete-li získat soubor ikony, kontaktujte tým podpory TOPdesk.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:
1. Přihlaste se k vaší **TOPdesk - zabezpečené** společnosti lokality jako správce.
2. V **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.
   
    ![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")

3. Klikněte na tlačítko **nastavení přihlášení**.
   
    ![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")

4. Rozbalte **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.
   
    ![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")

5. V **zabezpečeného** části **SAML přihlášení** konfigurace části, proveďte následující kroky:
   
    ![Technické nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "technické nastavení")
   
    a. Klikněte na tlačítko **Stáhnout** ke stažení souboru metadat veřejné a poté je uložit místně v počítači.
   
    b. Otevřete soubor metadat a vyhledejte **AssertionConsumerService** uzlu.
    
    ![Kontrolní výraz příjemce služby](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion příjemce služby")
   
    c. Kopírování **AssertionConsumerService** hodnotu.  
      
    > [!NOTE]
    > Budete potřebovat hodnotu v **konfigurace adresy URL aplikace** později v tomto kurzu.
    > 
    > 

6. V okně prohlížeče jiný web, přihlaste se k vaší **portál Azure classic** jako správce.

7. Na **TOPdesk - zabezpečené** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete ** nakonfigurovat jednotné přihlašování ** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "nakonfigurovat jednotné přihlašování")

8. Na **jak chcete uživatelům se přihlásit TOPdesk - zabezpečené** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "nakonfigurovat jednotné přihlašování")

9. Na **konfigurace adresy URL aplikace** proveďte následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurovat adresu URL aplikace")
   
    a. V **TOPdesk - zabezpečené přihlašovací adresa URL** textové pole, zadejte adresu URL používají vaši uživatelé pro přihlášení do vaší TOPdesk - zabezpečené aplikace (například: "*https://qssolutions.topdesk.net*").
   
    b. V **TOPdesk – veřejná adresa URL odpovědi** textovému poli, Vložit **TOPdesk - zabezpečené URL AssertionConsumerService** (například: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Klikněte na **Další**.

10. Na **nakonfigurovat jednotné přihlašování v TOPdesk - zabezpečené** klikněte na stránce pro stažení souboru metadat **stáhnout metadata**a potom uložte soubor místně na vašem počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "nakonfigurovat jednotné přihlašování")

11. Pokud chcete vytvořit soubor s certifikátem, proveďte následující kroky:
    
    ![Certifikát](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikátu")
    
    a. Otevřete soubor stažený metadat.
    b. Rozbalte **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.
    c. Zkopírujte hodnotu **certifikátu x 509** uzlu.
    d. Uložit zkopírovaný **certifikátu x 509** hodnota místně na vašem počítači v souboru.

12. Na vaše TOPdesk – zabezpečení společnosti lokality v **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.
    
    ![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")

13. Klikněte na tlačítko **nastavení přihlášení**.
    
    ![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")

14. Rozbalte **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.
    
    ![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")

15. V **veřejné** klikněte na tlačítko **přidat**.
    
    ![Přidat](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "přidat")

16. Na **pomocníka konfigurace SAML** dialogové okno proveďte následující kroky:
    
    ![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "pomocníka konfigurace SAML")
    
    a. K odeslání souboru stažené metadata, v části **federačních metadat**, klikněte na tlačítko **Procházet**.

    b. Nahrát soubor certifikátu, v části **certifikát (RSA)**, klikněte na tlačítko **Procházet**.

    c. Nahrát soubor logo jste získali od týmu podpory TOPdesk, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.

    d. V **atribut uživatelského jména** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. V **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.

    f. Klikněte na **Uložit**.

17. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "nakonfigurovat jednotné přihlašování")

## <a name="configuring-user-provisioning"></a>Konfiguraci zřizování uživatelů
Chcete-li povolit uživatelům Azure AD přihlášení do TOPdesk - bezpečný, se musí být zřízená do TOPdesk - zabezpečené.  
V případě TOPdesk - zabezpečený a zřizování je ruční úloha.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:
1. Přihlaste se k vaší **TOPdesk - zabezpečené** společnosti lokality jako správce.
2. V nabídce v horní části, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> operátor**.
   
    ![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "– operátor")

3. Na **operátor New** dialogové okno, proveďte následující kroky:
   
    ![Operátor new](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New – operátor")
   
    a. Klikněte na kartu Obecné.
   
    b. V **Přezdívka** textbox z **Obecné** zadejte příjmení chcete zřídit platný účet služby Azure Active Directory.
   
    c. Vyberte **lokality** pro účet v **umístění** části.
   
    d. V **přihlašovací jméno** textbox z **TOPdesk přihlášení** zadejte přihlašovací jméno uživatele.
   
    e. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít všechny ostatní TOPdesk – rozhraní API poskytované TOPdesk - zabezpečené zřídit AAD uživatelské účty nebo nástroje pro tvorbu účet zabezpečení uživatele.
> 
> 

## <a name="assigning-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Přiřazení uživatelů k TOPdesk - zabezpečit, proveďte následující kroky:
1. Na portálu Azure classic vytvořte zkušební účet.
2. Na ** TOPdesk - zabezpečené ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
    ![Přiřazení uživatelů](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "přiřazení uživatelů")

3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
    ![Ano](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

