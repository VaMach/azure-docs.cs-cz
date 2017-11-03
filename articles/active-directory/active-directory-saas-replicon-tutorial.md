---
title: 'Kurz: Azure Active Directory integrace s Replicon | Microsoft Docs'
description: "Další informace o použití Replicon s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Kurz: Azure Active Directory integrace s Replicon
Cílem tohoto kurzu je zobrazit integraci Azure a Replicon. Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Replicon klienta

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace ve vaší lokalitě společnosti Replicon (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Replicon [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikace pro Replicon
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-replicon-tutorial/IC777798.png "scénář")

## <a name="enable-the-application-integration-for-replicon"></a>Povolit integraci aplikace pro Replicon
Cílem této části se popisují postup povolení integrace aplikace pro Replicon.

**Pokud chcete povolit integraci aplikací pro Replicon, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
    ![Aplikace](./media/active-directory-saas-replicon-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Přidat aplikaci](./media/active-directory-saas-replicon-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **Replicon**.
   
    ![Galerie aplikací](./media/active-directory-saas-replicon-tutorial/IC777799.png "galerii aplikací")
7. V podokně výsledků vyberte **Replicon**a potom klikněte na **Complete** tuto aplikaci přidat.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování na Replicon ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **Replicon** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777801.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit Replicon** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777802.png "nakonfigurovat jednotné přihlašování")
3. Na **konfigurace adresy URL aplikace** proveďte následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurovat adresu URL aplikace")
  1. V **Replicon přihlašovací adresa URL** textovému poli, zadejte adresu URL Replicon klienta (například: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. V **adresa URL odpovědi Replicon** textovému poli, zadejte vaše Replicon **AssertionConsumerService** adresa URL (např: *https://global.replicon.com/! / typu saml2 nebo společnosti nebo jednotného přihlašování nebo pozálohovacího*).  
      
     >[!NOTE]
     >Můžete získat metadata Replicon na adresu URL: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Klikněte na **Další**.

4. Na **nakonfigurovat jednotné přihlašování v Replicon** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte metadata ve vašem počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777804.png "nakonfigurovat jednotné přihlašování")
5. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Replicon.

6. Pokud chcete konfigurovat SAML 2.0, proveďte následující kroky:
   
    ![Povolit ověřování SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "ověřování povolit SAML")
  
  1. K zobrazení **EnableSAML Authentication2** dialogové okno, připojte na adresu URL, následující po vašeho klíče společnosti: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Následující obrázek znázorňuje schéma úplnou adresu URL:  
   **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Klikněte na tlačítko  **+**  rozšířit **v20Configuration** části.
   3. Klikněte na tlačítko  **+**  rozšířit **metaDataConfiguration** části.
   4. Klikněte na tlačítko **zvolit soubor**, a vyberte soubor XML identity zprostředkovatele metadat, klikněte na tlačítko **odeslání**.

7. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC778418.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlášení do Replicon, musí být zřízená do Replicon.  

V případě Replicon zřizování je ruční úloha.

**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**

1. V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Replicon.
2. Přejděte na **správy \> uživatelé**.
   
    ![Uživatelé](./media/active-directory-saas-replicon-tutorial/IC777806.png "uživatelů")
3. Klikněte na tlačítko **+ přidat uživatele**.
   
    ![Přidat uživatele](./media/active-directory-saas-replicon-tutorial/IC777807.png "přidat uživatele")
4. V **profil uživatele** část, proveďte následující kroky:
   
    ![Profil uživatele](./media/active-directory-saas-replicon-tutorial/IC777808.png "profil uživatele")
   
  1. V **přihlašovací jméno** textovému poli, e-mailovou adresu uživatele Azure AD, které chcete zřídit typu Azure AD.
  2. Jako **typ ověřování**, vyberte **jednotného přihlašování k**.
  3. V **oddělení** textovému poli, zadejte uživatele oddělení.
  4. Jako **typ zaměstnance**, vyberte **správce**.
  5. Klikněte na tlačítko **uložit profil uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní Replicon uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Replicon zřídit AAD uživatelské účty.
> 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k Replicon, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.

2. Na **Replicon** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
    ![Přiřazení uživatelů](./media/active-directory-saas-replicon-tutorial/IC777809.png "přiřazení uživatelů")

3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
    ![Ano](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

