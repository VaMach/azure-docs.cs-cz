---
title: "Problém instalace přístup k aplikaci panelu rozšíření prohlížeče | Microsoft Docs"
description: "Jak opravit běžné chyby při instalaci rozšíření přístup k panelu prohlížeče"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 26dc5d5ffce84206450123132c0633c2aa323e9f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Rozšíření prohlížeče panelu problém instalace přístup k aplikaci

Přístupový Panel je webový portál, který uživatel, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) umožňuje zobrazovat a spouštět cloudové aplikace, které je správce Azure AD udělil přístup. Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu. Přístupový Panel je nezávislý na portálu Azure a nevyžaduje, aby uživatelé měli předplatné Azure.

Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Splňuje požadavky na prohlížeč pro přístup k panelu

Přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Pro jednotné přihlašování založené na heslech může být prohlížeče koncového uživatele:

-   Hraniční Windows 10 Anniversary Edition nebo novější. 

-   Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

-   Internet Explorer 8, 9, 10, 11 – na Windows 7 nebo novější (omezená podpora)
## <a name="how-to-install-the-access-panel-browser-extension"></a>Postup instalace rozšíření přístup k panelu prohlížeče

Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:

1.  Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.

3.  V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči budete přesměrováni na odkaz ke stažení. **Přidat** rozšíření do prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Rozšíření pro Chrome a hraniční může také stáhnout z přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu přístup Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

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
3. Rozšíření by měl změnit stav a umožňují znát heslo je k dispozici, klikněte na **rozšíření ikonu** k přihlášení.

Spustí aplikaci z rozšíření
1. Po instalaci rozšíření, přihlaste se do rozšíření výběrem **Přihlaste se k Začínáme**.
2. Klikněte na ikonu rozšíření otevřete jeho **nabídky**.
3. **Hledání** pro aplikaci na portálu Moje aplikace k dispozici.
4. Klikněte na aplikaci **výsledky hledání** spustíte.
5. Poslední tři aplikace spustí se také zobrazí v **nedávno použité** zástupce seznamu

> [!NOTE]
> Tyto možnosti jsou pouze k dispozici pro Edge, Chrome, Firefox.

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Nastavení zásad skupiny pro Internet Explorer

Můžete nastavit zásady skupiny, které vám umožní vzdáleně instalovat rozšíření Panel přístupu pro Internet Explorer na počítače uživatelů.

Požadavky patří:

-   Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a vaši uživatelé počítačů se připojili k vaší doméně.

-   Musíte mít oprávnění "Upravit nastavení" k úpravě objektu zásad skupiny (GPO). Ve výchozím nastavení, toto oprávnění mají členy z těchto skupin zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners. [Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Postupujte podle kurzu [nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md) pro podrobné informace o tom, jak nakonfigurovat zásady skupiny a nasadit je uživatelům.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Řešení potíží s příponou přístup k panelu v aplikaci Internet Explorer

Postupujte podle [řešení potíží s příponou Panel přístupu pro Internet Explorer](active-directory-saas-ie-troubleshooting.md) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci rozšíření pro aplikaci Internet Explorer.

> [!NOTE]
> Aplikace Internet Explorer je na omezenou podporu a už obdrží nové aktualizace softwaru. Okraj je doporučené prohlížeče.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Pokud tyto kroky řešení potíží problém nevyřeší

Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další postup
[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
