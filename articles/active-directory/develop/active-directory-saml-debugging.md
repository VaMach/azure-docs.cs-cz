---
title: "Postup ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak ladit na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 246709effcff1c38d14db3848fe2fad836ad90da
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Postup ladění na základě SAML jednotného přihlašování k aplikacím v Azure Active Directory
Při ladění aplikace založené na SAML integrace, je často vhodné použít nástroj, jako je [Fiddler](http://www.telerik.com/fiddler) zobrazíte žádost SAML, odpověď SAML a skutečný tokenu SAML, který byl vydán pro aplikace. Prozkoumáním tokenu SAML, lze zajistit, že všechny povinné atributy, uživatelské jméno v subjektu SAML a vystavitele URI jsou procházejícího podle očekávání.

![][1]

Odpověď z Azure AD, který obsahuje SAML token je obvykle ten, který nastane po přesměrování HTTP 302 z https://login.windows.net a je odeslána do konfigurovaného **adresa URL odpovědi** aplikace. 

Můžete zobrazit výběrem tohoto řádku a potom výběrem tokenu SAML **inspektoři > WebForms** kartě v pravém panelu. Zde, klikněte pravým tlačítkem myši **SAMLResponse** hodnotu a vyberte **poslat TextWizard**. Potom vyberte **z formátu Base64** z **transformace** nabídky k dekódování tokenu a zobrazit jeho obsah.

**Poznámka:**: Pokud chcete zobrazit obsah této žádosti HTTP, Fiddler můžete být vyzváni ke konfiguraci dešifrování přenos HTTPS, který budete muset provést.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikací Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Postup přizpůsobení deklarace identity vystavené v tokenu SAML pro předběžně integrované aplikace](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png