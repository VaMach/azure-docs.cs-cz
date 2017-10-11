---
title: 'Kurz: Azure Active Directory integrace s Qualtrics | Microsoft Docs'
description: "Další informace o použití Qualtrics s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Kurz: Azure Active Directory integrace s Qualtrics
Cílem tohoto kurzu je zobrazit integraci Azure a Qualtrics.  

Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:

* Platné předplatné Azure
* Qualtrics jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace ve vaší lokalitě společnosti Qualtrics (služba Zprostředkovatel iniciované přihlašování) nebo pomocí uživatele Azure AD, které jste přiřadili Qualtrics [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikace pro Qualtrics
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "scénář")

## <a name="enabling-the-application-integration-for-qualtrics"></a>Povolení integrace aplikace pro Qualtrics
Cílem této části se popisují postup povolení integrace aplikace pro Qualtrics.

**Pokud chcete povolit integraci aplikací pro Qualtrics, proveďte následující kroky:**

1. V portálu Azure classic, v levém navigačním podokně klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "služby Active Directory")
2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.
   
   ![Aplikace](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** v dolní části stránky.
   
   ![Přidat aplikaci](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "přidat aplikaci")
5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V **vyhledávacího pole**, typ **Qualtrics**.
   
   ![Galerie aplikací](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "galerii aplikací")
7. V podokně výsledků vyberte **Qualtrics**a potom klikněte na **Complete** tuto aplikaci přidat.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Cílem této části se popisují, jak uživatelům povolit ověřování na Qualtrics ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.

**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**

1. Na portálu Azure classic na **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "nakonfigurovat jednotné přihlašování")
2. Na **jak chcete uživatelům se přihlásit Qualtrics** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "nakonfigurovat jednotné přihlašování")
3. Na **konfigurace adresy URL aplikace** stránky v **Qualtrics přihlašovací adresa URL** textovému poli, zadejte adresu URL (například: "*https://ssotest2ut1.qualtrics.com*") a potom klikněte na **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurovat adresu URL aplikace")
4. Na **nakonfigurovat jednotné přihlašování v Qualtrics** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat ve vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "nakonfigurovat jednotné přihlašování")
5. Odešlete soubor metadat Qualtrics tým podpory.
   
   >[!NOTE]
   >Konfigurace jednotného přihlašování musí být provedena Qualtrics tým podpory. Zobrazí se oznámení a také konfigurace byla dokončena.
   > 
   > 
6. Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

Neexistuje žádná položka akce můžete nakonfigurovat na Qualtrics zřizování uživatelů. Když přiřazený uživatel se pokusí přihlásit pomocí přístupového panelu Qualtrics, Qualtrics ověří, zda uživatel existuje.  

Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Qualtrics.

## <a name="assign-users"></a>Přiřazení uživatelů
Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.

**Přiřazení uživatelů k Qualtrics, proveďte následující kroky:**

1. Na portálu Azure classic vytvořte zkušební účet.
2. Na **Qualtrics** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.
   
   ![Ano](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ano")

Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

