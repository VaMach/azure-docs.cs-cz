---
title: 'Kurz: Azure Active Directory integrace s Cisco Webex | Microsoft Docs'
description: "Další informace o použití Cisco Webex s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Kurz: Azure Active Directory integrace s Cisco Webex
Cílem tohoto kurzu je zobrazit integraci Azure a Cisco Webex.  
Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Klient Cisco Webex

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace na serveru Cisco Webex vaší společnosti (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Cisco Webex [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

* Povolení integrace aplikace pro Cisco Webex
* Konfigurace jednotného přihlašování (SSO)
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "scénář")

## <a name="enable-the-application-integration-for-cisco-webex"></a>Povolit integraci aplikace pro Cisco Webex
Cílem této části se popisují postup povolení integrace aplikace pro Cisco Webex.

**Pokud chcete povolit integraci aplikací pro Cisco Webex, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
   ![Přidat aplikaci](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **Cisco Webex**.
   
   ![Galerie aplikací](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "galerii aplikací")
7. V podokně výsledků vyberte **Cisco Webex**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování na Cisco Webex ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.  

V rámci tohoto postupu je nutné vytvořit certifikát kódováním base-64. Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit Cisco Webex** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "nakonfigurovat jednotné přihlašování")
3. Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurovat adresu URL aplikace")   
   1. V **Sing – adresa URL** textovému poli, zadejte adresu URL klienta Cisco Webex (např: *http://contoso.webex.com*).
   2. V **adresa URL odpovědi Webex Cisco** textovému poli, typ vaše **Cisco Webex AssertionConsumerService URL** (např: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).
4. Na **nakonfigurovat jednotné přihlašování v Cisco Webex** stránky, stáhněte si certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu v počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "nakonfigurovat jednotné přihlašování")
5. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Cisco Webex.
6. V nabídce v horní části, klikněte na tlačítko **Správa webu**.
   
   ![Správa lokalit](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "lokality správy")
7. V **spravovat lokality** klikněte na tlačítko **Konfigurace jednotného přihlašování k**.
   
   ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Konfigurace jednotného přihlašování")
8. V federovaného jednotného přihlašování konfiguraci webové části proveďte následující kroky:
   
   ![Konfigurace jednotného přihlašování federovaného](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federovaného jednotného přihlašování k konfigurace")  
   1. Z **Federation protokol** seznamu, vyberte **SAML 2.0**.
   2. Vytvoření **kódování Base-64** soubor stažený certifikát.  
    >[!TIP]
    >Další podrobnosti najdete v tématu [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Otevření kódovaného certifikátu kódování base-64 v programu Poznámkový blok a zkopírujte obsah ho.
   4. Klikněte na tlačítko **importovat Metadata SAML**a potom vložte vaše kódování base-64 kódovaného certifikátu.
   5. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **URL vystavitele** hodnotu a vložte ji do **vystavitele pro SAML (IdP ID)** textové pole.
   6. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **vzdálené adresy URL pro přihlášení** hodnotu a vložte ji do **zákazníka jednotného přihlašování služby přihlašovací adresa URL** textové pole.
   7. Z **NameID formátu** seznamu, vyberte **e-mailová adresa**.
   8. V **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.
   9. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie **vzdálené adresy URL odhlašovací** hodnotu a vložte ji do **adresy URL odhlašovací zákazníka jednotného přihlašování služby** textové pole.
   10. Klikněte na tlačítko **aktualizace**.
9. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlášení do Cisco Webex, musí být zřízená do Cisco Webex.  

* V případě Cisco Webex zřizování je ruční úloha.

**Ke zřízení uživatelských účtů, proveďte následující kroky:**

1. Přihlaste se k vaší **Cisco Webex** klienta.
2. Přejděte na **Správa uživatelů \> přidat uživatele**.
   
   ![Přidání uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "přidat uživatele")
3. V části přidat uživatele proveďte následující kroky:
   
   ![Přidat uživatele](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "přidat uživatele")   
   1. Jako **typ účtu**, vyberte **hostitele**.
   2. Zadejte informace o stávajícího uživatele Azure AD do následujících textových polí: **křestní jméno, příjmení**, **uživatelské jméno**, **e-mailu**, **heslo**, **Potvrdit heslo**.
   3. Klikněte na tlačítko **Přidat**.

>[!NOTE]
>Můžete použít všechny ostatní Cisco Webex uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cisco Webex zřizovat AAD uživatelské účty. 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k Cisco Webex, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

