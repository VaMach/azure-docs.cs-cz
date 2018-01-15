---
title: "Potřebujete pomoc s portálem Moje aplikace v Azure Active Directory | Microsoft Docs"
description: "Získáte pokyny k provádění běžných úloh při práci s na přístupovém panelu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 9bec51e1d49308baecc76143ec80902d2da418e8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>Potřebujete pomoci s portálem Moje aplikace?

Pravděpodobně bylo dosaženo této stránce vzhledem k tomu, že je bohužel běžely problém při použití aplikace My portálu. Když ale existují případy, které vyžadují, abyste obraťte se na helpdesk nebo správce získat problém vyřešit, tady jsou některé řešení potíží témata, která může být schopen pomoci nejprve.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Mám problémy při přihlášení k portálu Moje aplikace

Obecné problémy zkontrolujte:

- Zkontrolujte, pokud se přihlašujete do správnou adresu URL: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Zkuste přidat adresu URL do prohlížeče důvěryhodných serverů.

- Zajistěte, aby není platnost nebo zapomenete heslo. Zkontrolujte [sem](active-directory-passwords-update-your-own-password.md) další podrobnosti o tom, jak aktualizovat heslo.

- Zkontrolujte, jestli si kontaktní údaje ověřování je aktuální a není blokuje přístup. Zkontrolujte [sem](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user) další podrobnosti o nastavení informace o vašem ověřování.

- Vymažte soubory cookie v prohlížeči a zkuste se znovu přihlásit.

Pokud jsou stále dochází k problémům při pokusu o přihlášení, kontaktujte prosím správce o další pomoc.


## <a name="how-do-i-update-my-password"></a>Jak aktualizovat hesla?

Pokud jste heslo zapomněli, nikdy přijaté z pracovníci IT, byl uzamčen z vašeho účtu a chcete ho změnit, najdete v článku [nápovědy, zapomenuté heslo Azure AD](active-directory-passwords-update-your-own-password.md) další podrobnosti.

## <a name="how-do-i-register-for-password-reset"></a>Jak zaregistrovat pro resetování hesla?

Jako koncový uživatel můžete resetovat heslo nebo odemknout účet bez nutnosti řeči na uživatele, kteří používají samoobslužné resetování hesla (SSPR). Před použitím této funkce si budete muset zaregistrovat metody ověřování nebo potvrdit předdefinované metody ověřování, které připravil správce. Další podrobnosti najdete v tématu [registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Mám problémy při instalaci rozšíření Moje aplikace zabezpečené přihlašování

Zkontrolujte, pokud byly splněny požadavky na prohlížeč:

- Na portálu vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. Používáte-li heslo aplikace založené na jednom přihlášení, doprovodné rozšíření musí být nainstalována také. Toto rozšíření je stažen automaticky při spuštění aplikace, který je nakonfigurován pro heslo aplikace založené na jednom přihlášení.

- Požadavky na prohlížeč pro rozšíření jsou:
    - Hraniční Windows 10 Anniversary Edition nebo novější.
    - Chrome na systému Windows 7 nebo novější a v systému MacOS X nebo novější
    - Firefox 26.0 nebo novější na systému Windows XP SP2 nebo novější a na Mac OS X 10.6 nebo novější
    - Internet Explorer 8, 9, 10, 11 na Windows 7 nebo novější (omezená podpora)

Můžete také stáhnout rozšíření pro Chrome a hraniční z přímé odkazy níže:

- [Rozšíření Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Rozšíření Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Po instalaci zkuste následující kroky, pokud narazíte na problémy:

- Zkontrolujte v nastavení prohlížeče rozšíření povolení rozšíření.

- Restartujte prohlížeč a přihlaste se k portálu Moje aplikace.

- Vymažte soubory cookie prohlížeče a přihlaste se k portálu Moje aplikace.
- Postupujte podle [řešení potíží s příponou Panel přístupu pro Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) Průvodce pro přístup k nástroj pro diagnostiku a podrobné pokyny ke konfiguraci rozšíření pro aplikaci Internet Explorer.

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Použití rozšíření Moje aplikace zabezpečené přihlašování
Změna výchozí URL pro rozšíření Moje aplikace

Pokud používáte jinou adresu URL Moje aplikace než https://myapps.microsoft.com musíte nakonfigurovat výchozí adresa URL, když následující kroky:
1. Když nejste do rozšíření, **klikněte pravým tlačítkem na** ikonu rozšíření.
2. Klikněte na **vyberte adresu URL Moje aplikace** z nabídky.
3. **Vyberte** výchozí adresa URL.
4. Klikněte na ikonu rozšíření.
5. Přihlaste se do rozšíření výběrem **Přihlaste se k Začínáme**.

Přihlaste se přímo do aplikace z prohlížeče
1. Po instalaci rozšíření, přihlaste se do rozšíření výběrem **Přihlaste se k Začínáme**.
2. Přejděte na **adresa URL přihlašování** chcete přihlásit k aplikaci, to je obvykle adresu URL aplikace, zobrazí přihlašovací formulář.
3. Rozšíření by měl změnit stav a umožňují znát heslo je k dispozici, klikněte na **rozšíření ikonu** k přihlášení

Spustí aplikaci z rozšíření
1. Po instalaci rozšíření, přihlaste se do rozšíření výběrem **Přihlaste se k Začínáme**.
2. Klikněte na ikonu rozšíření otevřete nabídku.
3. **Hledání** pro aplikaci na portálu Moje aplikace k dispozici.
4. Klikněte na aplikaci **výsledky hledání** spustíte.
5. Poslední tři aplikace spustí se také zobrazí v **nedávno použité** zástupce seznamu

> [!NOTE]
> Tyto možnosti jsou pouze k dispozici pro Edge, Chrome, Firefox.

## <a name="how-do-i-add-a-new-app"></a>Jak lze přidat novou aplikaci?

1.  Na **aplikace** klikněte na tlačítko **přidat aplikaci**.

2.  Hledat aplikace, kterou chcete přidat a pak klikněte na **přidat**.

**Poznámky:**

- Pouze máte přístup k tuto možnost, pokud správce povolil to pro váš účet.

- Pokud aplikace vyžaduje oprávnění, budete muset čekat na schválení správce.


## <a name="how-do-i-manage-my-group-memberships"></a>Jak lze spravovat Moje členství ve skupině?

1. Klikněte **skupiny** dlaždici. 
2. Chcete-li vytvořit skupinu, v rámci skupiny I vlastní, klikněte na tlačítko **vytvořit skupinu**a pak postupujte podle pokynů.
3. O připojení ke skupině, v rámci skupiny jsem v, klikněte na tlačítko **připojit se ke skupině**a pak postupujte podle pokynů.

**Poznámky:**

- Pouze máte přístup k tuto možnost, pokud správce povolil to pro váš účet.

- Skupiny, které jsou členy umožňují zobrazit podrobnosti a nechte skupinu.

- Skupiny, které jste vlastníkem vám umožní zobrazit podrobnosti, přidat nebo odebrat členy a nechte skupinu.


## <a name="next-steps"></a>Další postup

Související informace o odstraňování potíží, najdete v části [problémů pomocí aplikace přístup k panelu webu nebo mobilní aplikace](active-directory-application-access-panel-content-map.md)

