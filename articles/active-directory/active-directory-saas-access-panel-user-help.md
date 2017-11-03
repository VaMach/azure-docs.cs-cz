---
title: "Potřebujete pomoc s portálem Moje aplikace v Azure Active Directory | Microsoft Docs"
description: "Získáte pokyny k provádění běžných úloh při práci s na přístupovém panelu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 9e2f8aa6ad7534ff822907285aa9fe290a4db586
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>Potřebujete pomoci s portálem Moje aplikace?

Pravděpodobně bylo dosaženo této stránce vzhledem k tomu, že je bohužel běžely problém při použití aplikace My portálu. Když ale existují případy, které vyžadují, abyste obraťte se na helpdesk nebo správce získat problém vyřešit, tady jsou některé řešení potíží témata, která může být schopen pomoci nejprve.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Mám problémy při přihlášení k portálu Moje aplikace

Obecné problémy zkontrolujte:

- Zkontrolujte, pokud se přihlašujete do správnou adresu URL: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Zkuste přidat adresu URL do prohlížeče důvěryhodných serverů.

- Zajistěte, aby není platnost nebo zapomenete heslo. Zkontrolujte [sem](active-directory-passwords-update-your-own-password.md) další podrobnosti o tom, jak aktualizovat heslo.

- Zkontrolujte, jestli si kontaktní údaje ověřování je aktuální a není blokuje přístup. Zkontrolujte [sem](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user) další podrobnosti o nastavení informace o vašem ověřování.

- Vymažte soubory cookie v prohlížeči a zkuste se znovu přihlásit.

Pokud jsou stále dochází k problémům při pokusu o přihlášení, kontaktujte prosím správce o další pomoc.


## <a name="how-do-i-update-my-password"></a>Jak aktualizovat hesla?

Pokud jste heslo zapomněli, nikdy přijaté z pracovníci IT, byl uzamčen z vašeho účtu a chcete ho změnit, najdete v článku [nápovědy, zapomenuté heslo Azure AD](active-directory-passwords-update-your-own-password.md) další podrobnosti.

## <a name="how-do-i-register-for-password-reset"></a>Jak zaregistrovat pro resetování hesla?

Jako koncový uživatel můžete resetovat heslo nebo odemknout účet bez nutnosti řeči na uživatele, kteří používají samoobslužné resetování hesla (SSPR). Před použitím této funkce si budete muset zaregistrovat metody ověřování nebo potvrdit předdefinované metody ověřování, které připravil správce. Další podrobnosti najdete v tématu [registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-portal-browser-extension"></a>Mám problémy při instalaci rozšíření portálu prohlížeče Moje aplikace

Zkontrolujte, pokud byly splněny požadavky na prohlížeč:

- Na portálu vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. Používáte-li heslo aplikace založené na jednom přihlášení, doprovodné rozšíření musí být nainstalována také. Toto rozšíření je stažen automaticky při spuštění aplikace, který je nakonfigurován pro heslo aplikace založené na jednom přihlášení.

- Požadavky na prohlížeč pro rozšíření jsou:
    - Internet Explorer 8, 9, 10, 11, Windows 7 nebo novější.
    - Hraniční Windows 10 Anniversary Edition nebo novější.
    - Chrome na systému Windows 7 nebo novější a v systému MacOS X nebo novější
    - Firefox 26.0 nebo novější na systému Windows XP SP2 nebo novější a na Mac OS X 10.6 nebo novější

Můžete také stáhnout rozšíření pro Chrome a hraniční z přímé odkazy níže:

- [Rozšíření Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Rozšíření Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Po instalaci zkuste následující kroky, pokud narazíte na problémy:

- Zkontrolujte v nastavení prohlížeče rozšíření povolení rozšíření.

- Restartujte prohlížeč a přihlaste se k portálu Moje aplikace.

- Vymažte soubory cookie prohlížeče a přihlaste se k portálu Moje aplikace.

## <a name="how-do-i-add-a-new-app"></a>Jak lze přidat novou aplikaci?

1.  Na **aplikace** klikněte na tlačítko **přidat aplikaci**.

2.  Hledat aplikace, kterou chcete přidat a pak klikněte na **přidat**.

**Poznámky:**

- Pouze máte přístup k tuto možnost, pokud správce povolil to pro váš účet.

- Pokud aplikace vyžaduje oprávnění, budete muset čekat na schválení správce.


## <a name="how-do-i-manage-my-group-memberships"></a>Jak lze spravovat Moje členství ve skupině?

1. Klikněte na dlaždici aplikace skupiny. 
2. Chcete-li vytvořit skupinu, v rámci skupin vlastní, klikněte na vytvořit skupinu a potom postupujte podle pokynů.
3. O připojení ke skupině, v rámci skupiny jsem v, klikněte na tlačítko Připojit se ke skupině a potom postupujte podle pokynů.

**Poznámky:**

- Pouze máte přístup k tuto možnost, pokud správce povolil to pro váš účet.

- Skupiny, které jsou členy umožňují zobrazit podrobnosti a nechte skupinu.

- Skupiny, které jste vlastníkem vám umožní zobrazit podrobnosti, přidat nebo odebrat členy a nechte skupinu.


## <a name="next-steps"></a>Další kroky

Související informace o odstraňování potíží, najdete v části [problémů pomocí aplikace přístup k panelu webu nebo mobilní aplikace](active-directory-application-access-panel-content-map.md)

