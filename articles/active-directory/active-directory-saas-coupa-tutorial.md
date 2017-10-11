---
title: 'Kurz: Azure Active Directory integrace s Coupa | Microsoft Docs'
description: "Další informace o použití Coupa s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Kurz: Azure Active Directory integrace s Coupa
Cílem tohoto kurzu je zobrazit integraci Azure a Coupa.  
Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Coupa jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili Coupa [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

* Povolení integrace aplikace pro Coupa
* Konfigurace jednotného přihlašování
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-coupa-tutorial/IC791897.png "scénář")

## <a name="enable-the-application-integration-for-coupa"></a>Povolit integraci aplikace pro Coupa
Cílem této části se popisují postup povolení integrace aplikace pro Coupa.

**Pokud chcete povolit integraci aplikací pro Coupa, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-coupa-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
   ![Přidat aplikaci](./media/active-directory-saas-coupa-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **Coupa**.
   
   ![Galerie aplikací](./media/active-directory-saas-coupa-tutorial/IC791898.png "galerii aplikací")
7. V podokně výsledků vyberte **Coupa**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování na Coupa ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.  

Konfigurace jednotného přihlašování pro Coupa vyžaduje, abyste načíst hodnotu kryptografický otisk z certifikátu. Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak načíst hodnoty kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Přihlásit se k serveru vaší společnosti Coupa jako správce.
2. Přejděte na **instalace \> řízení zabezpečení**.
   
   ![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")
3. Chcete-li stáhnout soubor metadat Coupa do počítače, klikněte na tlačítko **stahování a import metadat SP**.
   
   ![Metadata Coupa SP](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadat")
4. V okně jiný prohlížeč Přihlaste se k portálu Azure classic.
5. Na **Coupa** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791902.png "nakonfigurovat jednotné přihlašování")
6. Na **jak chcete uživatelům se přihlásit Coupa** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791903.png "nakonfigurovat jednotné přihlašování")
7. Na **konfigurace adresy URL aplikace** proveďte následující kroky:
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurovat adresu URL aplikace")   
   1. V **přihlašovací adresa URL** textovému poli, používá k přihlášení do aplikace Coupa vaši uživatelé zadat adresu URL (například: "*http://company.Coupa.com*").
   2. Otevřete váš stažený soubor metadat Coupa a zkopírujte **AssertionConsumerService index nebo adresa URL**.
   3. V **adresa URL odpovědi Coupa** textovému poli, Vložit **AssertionConsumerService index nebo adresa URL** hodnotu.
   4. Klikněte na **Další**.
8. Na **nakonfigurovat jednotné přihlašování v Coupa** klikněte na stránce pro stažení souboru metadat **stáhnout metadata**a potom uložte soubor místně na vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791905.png "nakonfigurovat jednotné přihlašování")
9. Na webu společnosti Coupa, přejděte na **instalace \> řízení zabezpečení**.
   
   ![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")
10. V **přihlásit pomocí přihlašovacích údajů Coupa** část, proveďte následující kroky:  

   ![Přihlaste se pomocí přihlašovacích údajů Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "přihlásit pomocí přihlašovacích údajů Coupa") 
   1. Vyberte **Přihlaste se pomocí SAML**.
   2. Klikněte na tlačítko **Procházet** k odeslání stažené Azure Active metadata souboru.
   3. Klikněte na **Uložit**.
11. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
    
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791907.png "nakonfigurovat jednotné přihlašování")
    
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlášení do Coupa, musí být zřízená do Coupa.  

* V případě Coupa zřizování je ruční úloha.

**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**

1. Přihlaste se k vaší **Coupa** společnosti lokality jako správce.
2. V nabídce v horní části, klikněte na tlačítko **instalace**a potom klikněte na **uživatelé**.
   
   ![Uživatelé](./media/active-directory-saas-coupa-tutorial/IC791908.png "uživatelů")
3. Klikněte na možnost **Vytvořit**.
   
   ![Vytvoření uživatelů](./media/active-directory-saas-coupa-tutorial/IC791909.png "vytvoření uživatelů")
4. V **vytvořit uživateli** část, proveďte následující kroky:
   
   ![Podrobné informace o uživateli](./media/active-directory-saas-coupa-tutorial/IC791910.png "podrobné informace o uživateli")
   
   1. Typ **přihlášení**, **křestní jméno**, **příjmení**, **ID přihlášení**, **e-mailu** atributy platný účet služby Azure Active Directory, který má být zahrnuty do související textových polí.
   2. Klikněte na možnost **Vytvořit**.   
   >[!NOTE]
   >Držitel účtu Azure Active Directory dostane e-mail s odkazem pro potvrzení účtu před stane aktivní. 
   > 

>[!NOTE]
>Můžete použít všechny ostatní Coupa uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Coupa zřídit AAD uživatelské účty. 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k Coupa, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na ** Coupa ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-coupa-tutorial/IC791911.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

