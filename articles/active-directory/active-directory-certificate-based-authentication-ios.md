---
title: "Azure Active Directory ověřování prostřednictvím certifikátu v systému iOS | Microsoft Docs"
description: "Další informace o podporované scénáře a požadavky na konfiguraci ověřování pomocí certifikátů v řešeních s zařízení s iOS"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 8e9610fee635ec24f8f7eaeba0c6a19a421c0456
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Azure Active Directory ověřování prostřednictvím certifikátu v systému iOS

Ověřování pomocí certifikátů (CBA) umožňuje službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online:

* Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word   
* Klienti Exchange ActiveSync (EAS)

Konfigurace tato funkce eliminuje potřebu zadejte kombinace uživatelského jména a hesla do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení.

Toto téma poskytuje požadavky a podporované scénáře konfigurace CBA pro zařízení s iOS(Android) pro uživatele klientů v Office 365 Enterprise, Business, Education, US Government, Čína, a plány Německu.

Tato funkce je dostupná ve verzi preview v Office 365 US Government obrany a federální plány.




## <a name="microsoft-mobile-applications-support"></a>Podpora mobilních aplikací Microsoft

| Aplikace | Podpora |
| --- | --- |
| Azure Information Protection aplikace |![Zaškrtnout][1] |
| Portál společnosti Intune |![Zaškrtnout][1] |
| Microsoft Teams |![Zaškrtnout][1] |
| OneNote |![Zaškrtnout][1] |
| OneDrive |![Zaškrtnout][1] |
| Outlook |![Zaškrtnout][1] |
| Power BI |![Zaškrtnout][1] |
| Skype pro firmy |![Zaškrtnout][1] |
| Aplikace Word / Excel / PowerPoint |![Zaškrtnout][1] |
| Yammer |![Zaškrtnout][1] |


## <a name="requirements"></a>Požadavky

Verze operačního systému zařízení musí být iOS 9 a novějším

Federační server musí být nakonfigurované.  

Microsoft Authenticator je vyžadována pro aplikace Office v systému iOS.  

Pro Azure Active Directory k odvolání certifikátu klienta musí mít tokenu služby AD FS následující deklarace identity:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (Sériové číslo certifikátu klienta)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (String pro vystavitele certifikátu klienta)

Azure Active Directory přidá tyto deklarace do tokenu obnovení, pokud jsou k dispozici v tokenu služby AD FS (nebo jiné tokenu SAML). Pokud token obnovení, musí se ověřit, tyto informace slouží ke kontrole odvolání.

Jako osvědčený postup by měl aktualizovat chybové stránky služby AD FS s následujícími službami:

* Požadavek na instalaci Microsoft Authenticator v systému iOS
* Pokyny, jak získat certifikát uživatele.

Další podrobnosti najdete v tématu [přizpůsobení stránek přihlášení AD FS](https://technet.microsoft.com/library/dn280950.aspx).

Odeslat některé aplikace Office (s povolené moderní ověřování),*řádku = přihlášení*se do služby Azure AD v jejich požadavku. Ve výchozím nastavení, Azure AD to převede v požadavku na služby AD FS pro "*wauth = usernamepassworduri*' (požádá ADFS udělat ověřování U/P) a '*wfresh = 0*' (požádá ADFS ignorovat stavu jednotné přihlašování a provádět ověřování obnovit). Pokud chcete povolit ověřování pomocí certifikátů pro tyto aplikace, budete muset upravit výchozí chování Azure AD. Stačí nastavit "*PromptLoginBehavior*"v nastavení federované domény k'*zakázané*'.
Můžete použít [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) rutina k provedení této úlohy:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`


## <a name="exchange-activesync-clients-support"></a>Podporu klientů protokolu Exchange ActiveSync
V systému iOS 9 nebo novější je podporovaná nativní aplikace pro iOS e-mailového klienta. Pro všechny ostatní aplikace Exchange ActiveSync Pokud chcete zjistit, pokud je tato funkce podporována, obraťte se na vývojáře vaší aplikace.  


## <a name="next-steps"></a>Další kroky

Pokud chcete nakonfigurovat ověřování pomocí certifikátů ve vašem prostředí, přečtěte si téma [Začínáme s ověřováním na základě certifikátu v systému Android](active-directory-certificate-based-authentication-get-started.md) pokyny.


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
