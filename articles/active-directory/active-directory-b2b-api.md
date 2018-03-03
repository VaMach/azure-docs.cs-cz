---
title: "Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení | Microsoft Docs"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: b40dc42c1dfc8910f9be9242fee3beeade92d193
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení

Narazili jsme mnoho zákazníků, řekněte nám, chtějí-li k přizpůsobení procesu pozvánku způsobem, který je nejvhodnější pro jejich organizace. Naše rozhraním API můžete to udělat jen. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Možnosti pozvánku rozhraní API
Rozhraní API nabízí tyto možnosti:

1. Pozvěte externího uživatele s *žádné* e-mailovou adresu.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Přizpůsobte, kam chcete uživatelům zobrazovat po přijetí jejich pozvánku.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Vyberte možnost odesílat e-mailu standardní pozvánku prostřednictvím nám

    ```
    "sendInvitationMessage": true
    ```

  zpráva pro příjemce, který můžete přizpůsobit

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. A zvolte na kopii: osoby, které chcete zachovat ve smyčce o tento spolupracovník pozvání.

5. Nebo zcela přizpůsobit pozvánky a pracovní postup registrace výběrem nechcete odeslat oznámení prostřednictvím služby Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  V takovém případě můžete adresu URL se od získat rozhraní API, které můžete vložit šablonu e-mailu, zasílání rychlých zpráv nebo jiné metody distribuce podle svého výběru.

6. Nakonec pokud jste správce, můžete pozvat uživatele jako člen.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Modelu autorizace
Rozhraní API můžete spustit v následujících režimech ověřování:

### <a name="app--user-mode"></a>Aplikace + uživatelského režimu
V tomto režimu kdo používá rozhraní API musí mít oprávnění pro být vytvoření pozvánky B2B.

### <a name="app-only-mode"></a>Jenom režim aplikace
V kontextu pouze aplikace musí aplikace oboru User.Invite.All této pozvánky proběhla úspěšně.

Další informace najdete v části: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Nyní je možné použít PowerShell k přidání a snadno pozvat externí uživatele organizaci. Vytvoření pozvánky pomocí rutiny:

```
New-AzureADMSInvitation
```

Můžete použít následující možnosti:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Můžete se taky podívat na odkaz na pozvánku k rozhraní API v [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Další postup

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy e-mail pozvánku spolupráce B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
