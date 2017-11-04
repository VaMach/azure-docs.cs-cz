---
title: "Azure Active Directory podmíněného přístupu technické informace | Microsoft Docs"
description: "Další informace o použití podmíněného řízení přístupu v Azure Active Directory. Zadejte podmínky pro ověřování uživatelů a řízení přístupu k vaší aplikaci. Při splnění zadané podmínky, ověření uživatele a udělit přístup k vaší aplikaci."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 4bcca3ee5862a0455b6064d0f9c0a91388db9516
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory podmíněného přístupu technické reference

Můžete použít [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) a systém doladit jak oprávněným uživatelům můžete přístup k prostředkům.  

Toto téma obsahuje informace o podporu pro následující možnosti konfigurace pro zásady podmíněného přístupu: 

- Přiřazení cloudové aplikace

- Podmínka platformy zařízení 

- Stav aplikace klienta

- Požadavek schválené klientské aplikace



## <a name="cloud-apps-assignments"></a>Přiřazení cloudové aplikace

Až budete konfigurovat zásadu podmíněného přístupu, budete muset [vyberte cloudové aplikace, které používají zásady](active-directory-conditional-access-azure-portal.md#who). 

![Vyberte zásady pro cloudové aplikace](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft cloudové aplikace

Zásady podmíněného přístupu můžete přiřadit následující cloudových aplikací od společnosti Microsoft:

- Azure Information Protection – [Další informace](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- Azure RemoteApp

- Microsoft Dynamics 365

- Aplikace Microsoft Office 365 Yammer

- Aplikace Microsoft Office 365 Exchange Online

- Aplikace Microsoft Office 365 SharePoint Online (zahrnuje Onedrivu pro firmy)

- Microsoft Power BI 

- Microsoft Visual Studio Team Services

- Microsoft Teams


### <a name="other-applications"></a>Ostatní aplikace 

Kromě cloudových aplikacích Microsoft můžete přiřadit zásady podmíněného přístupu pro následující typy cloudových aplikací:

- Aplikace Azure AD připojení

- Předem integrovaných federované aplikace software jako služba (SaaS)

- Aplikace, které používají heslo jednotné přihlašování (SSO)

- Obchodní aplikace

- Aplikace, které používají Azure AD Application Proxy


## <a name="device-platform-condition"></a>Podmínka platformy zařízení

V zásadách podmíněného přístupu můžete nakonfigurovat podmínky platformy zařízení ke svázání zásady tak, aby operační systém na klientovi.

![Tie – zásady přístupu do klientského operačního systému](./media/active-directory-conditional-access-technical-reference/41.png)

Podmíněný přístup pro Azure AD podporuje tyto platformy zařízení:

- Android

- iOS

- telefon se systémem Windows

- Windows

- systému macOS (preview)



## <a name="client-apps-condition"></a>Stav aplikace klienta 

Pokud budete konfigurovat zásadu podmíněného přístupu, můžete [vyberte klientské aplikace](active-directory-conditional-access-azure-portal.md#client-apps) pro podmínku klientské aplikace. Nastavení klienta aplikace podmínku, která má udělit nebo blokovat přístup, když je proveden pokus o přístup z následujících typů klientských aplikací:

- Prohlížeč
- Mobilní aplikace a aplikace klasické pracovní plochy

![Řízení přístupu pro klientské aplikace](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Podporované prohlížeče 

Řídit přístup z prohlížeče pomocí **prohlížeče** možnost v požadované zásady podmíněného přístupu. Udělí přístup pouze při pokusu o přístup pomocí podporovaného prohlížeče. Když je proveden pokus o přístup pomocí nepodporovaný prohlížeč, je blokována pokus.

![Řízení přístupu pro podporované prohlížeče](./media/active-directory-conditional-access-technical-reference/05.png)

Zásady podmíněného přístupu podporuje následující prohlížeče: 


| Operační systém                     | Prohlížeče                    | Podpora     |
| :--                    | :--                         | :-:         |
| Windows 10             | Internet Explorer a okraje     | ![Zaškrtnout][1] |
| Windows 10             | Chrome                      | ![Zaškrtnout][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome   | ![Zaškrtnout][1] |
| Windows 7              | Internet Explorer, Chrome   | ![Zaškrtnout][1] |
| iOS                    | Safari, Intune Managed Browser                      | ![Zaškrtnout][1] |
| Android                | Chrome, Intune Managed Browser                      | ![Zaškrtnout][1] |
| telefon se systémem Windows          | Internet Explorer a okraje     | ![Zaškrtnout][1] |
| Windows Server 2016    | Internet Explorer a okraje     | ![Zaškrtnout][1] |
| Windows Server 2016    | Chrome                      | Již brzy |
| Windows Server 2012 R2 | Internet Explorer, Chrome   | ![Zaškrtnout][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome   | ![Zaškrtnout][1] |
| macOS                  | Safari                      | ![Zaškrtnout][1] |
| macOS                  | Chrome                      | Již brzy |

> [!NOTE]
> Pro podporu Chrome muse použití Windows 10 Creators Update (verze 1703) nebo novější.<br>
> Můžete nainstalovat [toto rozšíření](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="supported-mobile-applications-and-desktop-clients"></a>Podporovaná mobilní aplikace a klienti vzdálené plochy

Řízení přístupu aplikace a klienta pomocí **mobilní aplikace a klienti vzdálené plochy** možnost v požadované zásady podmíněného přístupu. Udělí přístup pouze při pokusu o přístup pomocí podporovaných mobilní aplikace nebo plochy klienta. Když je proveden pokus o přístup pomocí nepodporované aplikace nebo klienta, je blokována pokus.

![Řízení přístupu pro podporovaných mobilních aplikací nebo klienti vzdálené plochy](./media/active-directory-conditional-access-technical-reference/06.png)

Následující mobilní aplikace a klienti vzdálené plochy podporují podmíněný přístup pro Office 365 a další služby Azure AD připojené aplikace:


| Klientské aplikace| Cílová služba| Platforma |
| --- | --- | --- |
| Vzdálené aplikace Azure| Služba vzdálené aplikace Azure| Windows 10, Windows 8.1, Windows 7, iOS, Android a Mac OS X|
| Aplikaci Dynamics CRM| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS a Android|
| E-mailu, kalendáři nebo osoby aplikace Outlook 2016, aplikace Outlook 2013 (s moderní ověřování)| Office 365 Exchange Online| Windows 10|
| MFA a umístění zásady pro aplikace. Zásady zařízení na základě nejsou podporovány.| Všechny služby app service pro Moje aplikace| Android a iOS|
| Microsoft týmy služby – tato volba určuje všechny služby, které podporují Teams společnosti Microsoft a všechny jeho klientských aplikací – Windows Desktop, iOS, Android, webové části a webového klienta| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS a Android|
| Aplikace Office 2016, Office 2013 (s moderní ověřování), Onedrivu synchronizovat klienta (viz [poznámky](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))| Office 365 Sharepointu Online| Windows 8.1, Windows 7|
| Aplikace Office 2016, Office univerzální aplikace, Office 2013 (s moderní ověřování), OneDrive synchronizace klienta (v tématu [poznámky k](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), podporu skupiny Office je plánované v budoucnosti, podpora aplikací služby SharePoint je plánované v budoucnosti| Office 365 Sharepointu Online| Windows 10|
| Office 2016 pro systému macOS (Word, Excel, PowerPoint, OneNote pouze). OneDrive pro firmy podporu plánované v budoucnosti| Office 365 Sharepointu Online| Mac OS X|
| Mobilní aplikace Office| Office 365 Sharepointu Online| iOS, Android|
| Aplikace Yammer Office| Yammer Office 365| Windows 10, iOS, Android|
| Outlook 2016 (Office pro systému macOS)| Office 365 Exchange Online| Mac OS X|
| Outlook 2016, aplikace Outlook 2013 (s moderní ověřování), Skype pro firmy (s moderní ověřování)| Office 365 Exchange Online| Windows 8.1, Windows 7|
| Mobilní aplikace Outlook| Office 365 Exchange Online| iOS|
| Aplikaci PowerBI. Power BI aplikace pro Android v současné době nepodporuje podmíněného přístupu podle zařízení.| Služba PowerBI| Windows 10, Windows 8.1, Windows 7 a iOS|
| Skype pro firmy| Office 365 Exchange Online| Android, IOS|
| Visual Studio Team Services aplikace| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS a Android|



## <a name="approved-client-app-requirement"></a>Požadavek schválené klientské aplikace 

Řízení připojení klienta pomocí **vyžadují schválené klientskou aplikaci** možnost v požadované zásady podmíněného přístupu. Udělí přístup pouze při pokusu o připojení klienta schválených aplikací.

![Řízení přístupu pro schválené klientské aplikace](./media/active-directory-conditional-access-technical-reference/21.png)

Následující aplikace klienta lze použít s požadavků aplikace schválené klienta:

- Microsoft Excel

- Microsoft OneDrive

- Aplikace Microsoft Outlook

- Microsoft OneNote

- Microsoft PowerPoint

- Microsoft SharePoint

- Microsoft Skype pro firmy

- Microsoft Teams

- Aplikace Microsoft Visio

- Aplikace Microsoft Word


**Poznámky**

- Schválené klientské aplikace podporují funkce správy mobilních aplikací Intune.

- **Vyžadují schválené klientskou aplikaci** požadavek:

    - Podporuje jenom iOS a Android pro [podmínku platformy zařízení](#device-platforms-condition).

    - Nepodporuje **prohlížeče** možnost [klienta aplikace podmínku](#supported-browsers).
    
    - Nahrazuje **mobilní aplikace a klienti vzdálené plochy** možnost [klienta aplikace podmínku](#supported-mobile-apps-and-desktop-clients) když je tato možnost vybraná.


## <a name="next-steps"></a>Další kroky

- Přehled podmíněného přístupu najdete v tématu [podmíněného přístupu v Azure Active Directory](active-directory-conditional-access-azure-portal.md).
- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu v prostředí, najdete v článku [doporučené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md).



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


