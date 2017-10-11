---
title: "Kurz: Azure Active Directory integrace s centrální plochy | Microsoft Docs"
description: "Další informace o použití centrální plochy s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Kurz: Azure Active Directory integrace s centrální plochy
Cílem tohoto kurzu je zobrazit integraci Azure a centrální plochy. Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Centrální plochy jednotné přihlašování v předplatném povolené / centrální plochy klienta

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

* Povolení integrace aplikace pro centrální plochy
* Konfigurace jednotného přihlašování (SSO)
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "scénář")

## <a name="enable-the-application-integration-for-central-desktop"></a>Povolit integraci aplikace pro centrální plochu
Cílem této části se popisují postup povolení integrace aplikací pro centrální plochu.

**Pokud chcete povolit integraci aplikací pro centrální plochu, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
   ![Přidat aplikaci](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **centrální plochy**.
   
   ![Galerie aplikací](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "galerii aplikací")
7. V podokně výsledků vyberte **centrální plochy**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Centrální plochy](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrální plochy")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování centrální plochu do svého účtu ve službě Azure AD využívající federaci na základě protokolu SAML.

V rámci tohoto postupu je nutné odeslat kódování base-64 kódovaného certifikátu klienta centrální plochy.  
Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete ** nakonfigurovat jednotné přihlašování ** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit ploše centrální** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "nakonfigurovat jednotné přihlašování")
3. Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**: 
   
   1. V **centrální plochy přihlašovací v adrese URL** textovému poli, zadejte adresu URL vašeho klienta centrální Desktop (např: *http://contoso.centraldesktop.com*).
   2. Do textového pole Adresa URL odpovědi centrální plochy zadejte vaše adresa URL centrální plochy AssertionConsumerService (např: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Můžete získat hodnotu z centrální plochy metadat (např: *http://contoso.centraldesktop.com*).
   >  
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurovat adresu URL aplikace")
4. Na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, stáhněte si certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu v počítači.
   
  ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "nakonfigurovat jednotné přihlašování")
5. Přihlaste se k vaší **centrální plochy** klienta.
6. Přejděte na **nastavení**, klikněte na tlačítko **Upřesnit**a potom klikněte na **jednotné přihlašování**.
   
  ![Instalační program – pokročilé](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "instalační program – Rozšířená")
7. Na **jeden znak v nastavení** proveďte následující kroky:
   
  ![Jednotné přihlašování v nastavení](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "jednotné přihlašování v nastavení")
   
  1. Vyberte **povolit SAML v2 jednotného přihlašování**.
  2. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **URL vystavitele** hodnotu a vložte ji do **URL jednotného přihlašování k** textové pole.
  3. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **vzdálené adresy URL pro přihlášení** hodnotu a vložte ji do **adresu URL pro přihlášení SSO** textové pole .
  4. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v centrální plochy** stránky, zkopírujte **jednu adresu URL služby Sign-Out** hodnotu a vložte ji do **adresy URL odhlašovací jednotného přihlašování k**textové pole.
8. V **metodu ověřování podpisu zpráva** část, proveďte následující kroky:
   
   ![Zpráva metodu ověřování podpisu](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "zprávy metodu ověřování podpisu")
   
  1. Vyberte **certifikát**.
  2. Z **certifikát jednotného přihlašování k** seznamu, vyberte **RSH SHA256**.
  3. Vytvořte textový soubor z stažený certifikát, kopírovat obsah textového souboru a vložte ji do **certifikát jednotného přihlašování k** pole.  
     >[!TIP]
     >Další podrobnosti najdete v tématu [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Vyberte **zobrazí odkaz na přihlašovací stránku vaší SAMLv2**.
9. Klikněte na tlačítko **aktualizace**.
10. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "nakonfigurovat jednotné přihlašování")
    
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

AAD uživatelé moci přihlásit musí být zřízená aplikaci Centrální plochy. Tato část popisuje postup vytvoření AAD uživatelské účty v centrální plochy.

**Ke zřízení uživatelských účtů do centrální plochy:**
1. Přihlaste se ke klientovi centrální plochy.
2. Přejděte na **osoby \> vnitřní členy**.
3. Klikněte na tlačítko **přidat vnitřní členy**.
   
  ![Lidé](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "osoby")
4. V **e-mailovou adresu nové členy** textovému poli, zadejte účet AAD určené ke zřízení a potom klikněte na **Další**.
   
  ![E-mailové adresy nové členy](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-mailové adresy nové členy")
5. Klikněte na tlačítko **přidejte interní členy**.
   
  ![Přidání interní člena](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "přidat vnitřní člen")
   
   >[!NOTE]
   >Uživatelé, pro které jste přidali obdrží e-mail, který obsahuje odkaz potvrzení, které potřebují k klikněte na možnost aktivovat účet.
   > 

>[!NOTE]
>Můžete použít jakékoli jiné centrální plochy uživatele účtu vytvoření nástroje nebo rozhraní API poskytované centrální plochy zřídit AAD uživatelské účty
>  

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k centrální plochy, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na **centrální plochy** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

