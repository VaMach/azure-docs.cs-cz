---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Další informace o použití izolovaného prostoru Salesforce službou Azure Active Directory umožňující jednotné přihlašování, automatické zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 32835e79188806bb2ff319eea23b1b52ab585ab1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce

Cílem tohoto kurzu je zobrazit integraci Azure a izolovaného prostoru služby Salesforce.  

>[!TIP]
>Pro zpětnou vazbu, najdete v článku [stránky podpory Azure](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Izolovaných prostorů získáte možnost vytvářet více kopií vaší organizace v samostatných prostředí pro jiné účely, například vývoj, testování a cvičení, aniž by to ohrozilo datům a aplikacím v organizaci produkční Salesforce.  

Další podrobnosti najdete v tématu [izolovaného prostoru Přehled](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Izolovaného prostoru v Salesforce.com

Pokud nemáte platný izolovaného prostoru v Salesforce.com ještě, budete muset kontaktovat Salesforce.

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikací pro izolovaný prostor Salesforce
2. Konfigurace jednotného přihlašování (SSO)
3. Povolení vaší doméně
4. Konfiguraci zřizování uživatelů
5. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "scénář")

## <a name="enable-the-application-integration-for-salesforce-sandbox"></a>Povolit integraci aplikace pro izolovaný prostor Salesforce
Cílem této části se popisují postup povolení integrace aplikací pro izolovaný prostor Salesforce.

**Pokud chcete povolit integraci aplikací pro izolovaný prostor Salesforce, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "aplikace")
4. Otevřete **galerii aplikací**, klikněte na tlačítko **přidat aplikaci**a potom klikněte na **přidat aplikaci pro Moje organizace používat**.
   
   ![Co chcete udělat? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcete udělat?")
5. V **vyhledávacího pole**, typ **izolovaného prostoru Salesforce**.
   
   ![Galerie aplikací](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "galerii aplikací")
6. V podokně výsledků vyberte **izolovaného prostoru Salesforce**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "izolovaného prostoru Salesforce")
   
## <a name="configur-single-sign-on-sso"></a>Configur jednotné přihlašování (SSO)

Cílem této části se popisují, jak uživatelům povolit ověřování do služby Salesforce ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit izolovaného prostoru Salesforce** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "izolovaného prostoru Salesforce")
3. Na **konfigurace adresy URL aplikace** stránky v **přihlašovací adresa URL** textovému poli, zadejte URL pomocí následujícího vzorce `http://company.my.salesforce.com`a potom klikněte na **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurovat adresu URL aplikace")
4. Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je rovněž nutné nakonfigurovat **identifikátor** do mají stejnou hodnotu jako **přihlásit na adrese URL**. 
 * **Identifikátor** pole naleznete kontrolou **zobrazit upřesňující nastavení** zaškrtávací políčko je na **konfigurace adresy URL aplikace** stránky dialogového okna.
5. Na **nakonfigurovat jednotné přihlašování v izolovaném prostoru Salesforce** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu v počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "nakonfigurovat jednotné přihlašování")
6. V okně prohlížeče jiný web Přihlaste se jako správce do izolovaného prostoru vaší služby Salesforce.
7. V nabídce v horní části, klikněte na tlačítko **instalační program**.
   
   ![Instalační program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "instalační program")
8. V navigačním podokně na levé straně klikněte na **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "jednotné přihlašování v nastavení")
9. V části Nastavení jednotného přihlašování proveďte následující kroky:
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "jednotné přihlašování v nastavení")  
 1.  Vyberte **povoleno SAML**. 
 2.  Klikněte na možnost **Nové**.
10. V části SAML jeden přihlašování nastavení proveďte následující kroky:
    
    ![SAML jeden nastavení přihlášení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML jeden nastavení přihlášení")  
 1. Do textového pole Název zadejte název konfigurace (např: *SPSSOWAAD\_Test*). 
 2. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Salesforce izolovaného prostoru** dialogu stránky, kopie **URL vystavitele** hodnotu a vložte ji do **vystavitele** textové pole.
 3. V **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud se jedná o první instance Salesforce izolovaného prostoru, který chcete přidat do vašeho adresáře. Pokud jste již přidali instance Salesforce izolovaného prostoru, pak pro **Entity ID** zadejte **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`   
 4. Klikněte na tlačítko **Procházet** nahrát stažený certifikát.  
 5. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje ID federace z objektu uživatele**. 
 6. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier příkaz subjektu**.
 7. Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v Salesforce izolovaného prostoru** dialogu stránky, kopie **vzdálené adresy URL pro přihlášení** hodnotu a vložte ji do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole. 
 8. SFDC nepodporuje SAML odhlášení.  Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' ji do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.
 9. Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**. 
 10. Klikněte na **Uložit**.
11. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "nakonfigurovat jednotné přihlašování")

## <a name="enable-your-domain"></a>Povolit doménu
V této části se předpokládá, že jste již vytvořili domény.  Další podrobnosti najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Pokud chcete povolit doménu, proveďte následující kroky:**

1. V levém navigačním podokně klikněte na tlačítko **Správa domén**a pak klikněte na tlačítko **Moje domény.**
   
   ![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Moje doména")
   
   >[!NOTE]
   >Přesvědčte se, že doménu je správně nakonfigurováno. 
   > 
2. V **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název SAML jeden přihlašování nastavení z předchozí části a nakonec klikněte na tlačítko **Uložit**.
   
   ![Moje doména](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Moje doména")

Jakmile máte doménu nakonfigurovat, uživatelé měli používat adresa URL domény k přihlášení k izolovanému prostoru služby Salesforce.  

Chcete-li získat hodnotu adresy URL, klikněte na tlačítko jednotného přihlašování k profilu, který jste vytvořili v předchozí části.

## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů
Cílem této části se popisují postup povolení zřizování uživatelů služby Active Directory uživatelských účtů do izolovaného prostoru služby Salesforce.

**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**

1. Na portálu služby Salesforce v horním navigačním panelu vyberte název rozšíření nabídky vaše uživatele:
   
   ![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "nastavení")
2. Z nabídky uživatele, vyberte **Moje nastavení** otevřete váš **Moje nastavení** stránky.
3. V levém podokně klikněte na **osobní** rozbalte část osobní a potom klikněte na **resetovat Moje zabezpečení tokenu**:
   
   ![Nastavení](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "nastavení")
4. Na **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** požádat o e-mailu, který obsahuje váš token zabezpečení Salesforce.com.
   
   ![Nový Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "nový Token")
5. Zkontrolujte Doručená pošta e-mailu z Salesforce.com s "**Salesforce.com zabezpečení potvrzení**" jako předmět.
6. Přečtěte si tento e-mail a zkopírujte hodnotu tokenu zabezpečení.
7. Na portálu Azure classic na **salesforce izolovaného prostoru** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů** otevřete **konfiguraci zřizování uživatelů** dialogové okno.
   
   ![Konfiguraci zřizování uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfiguraci zřizování uživatelů")
8. Na **zadejte své přihlašovací údaje služby Salesforce izolovaného prostoru zapněte automatické uživatele zřizování** stránky, zadejte následující nastavení konfigurace:
   
   ![Izolovaný prostor Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "izolovaného prostoru Salesforce")   
 1. V **uživatelské jméno správce izolovaného prostoru Salesforce** textovému poli, zadejte název, který má účet služby Salesforce izolovaném prostoru **správce systému** profil v Salesforce.com přiřazen.
 2. V **heslo správce izolovaného prostoru Salesforce** textovému poli, zadejte heslo pro tento účet.
 3. V **tokenu zabezpečení uživatele** textovému poli, vložte hodnotu tokenu zabezpečení.
 4. Klikněte na tlačítko **ověřením** k ověření vaší konfigurace.
 5. Klikněte **Další** tlačítko Otevřít **potvrzení** stránky.
9. Na **potvrzení** klikněte na tlačítko **Complete** uložte konfiguraci.
   
## <a name="assigning-users"></a>Přiřazení uživatelů

Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k izolovanému prostoru služby Salesforce, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na ** izolovaného prostoru Salesforce ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ano")

Teď by měla Počkejte 10 minut a ověřte, že účet umístění byl synchronizován do izolovaného prostoru služby Salesforce.

Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).

