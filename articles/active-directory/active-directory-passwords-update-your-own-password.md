---
title: "Azure AD: Resetování hesla | Dokumentace Microsoftu"
description: "Prostřednictvím samoobslužného resetování hesla můžete znovu získat přístup ke svému účtu."
services: active-directory
keywords: "správa hesel služby Active Directory, správa hesel, samoobslužné resetování hesla Azure AD"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: joflore
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 9b7735bbe8d3b2ffc78eba8f1489d70a02fe7342
ms.lasthandoff: 04/12/2017


---
# <a name="help-i-forgot-my-password"></a>Nápověda k zapomenutému heslu

Krok 1: Zachovejte klid.

Pokud se vás týkají následující scénáře, můžeme vám pomoct.

* Nejste si jistí, jak získat přístup k účtu, a zapomněli jste heslo.
* Nebylo vám přiřazeno heslo a správce vás poslal sem.

## <a name="unlock-your-account"></a>Odemknutí účtu

Pokud chcete odemknout účet, postupujte podle následujících kroků. Až se v kroku 6 níže zobrazí výzva **Vyberte nové heslo**, můžete heslo odemknout nebo změnit a provést odemknutí.

## <a name="reset-my-password"></a>Resetování hesla

Chcete-li se opět přihlásit ke svému účtu, postupujte podle následujících kroků.
1. Na kterékoli pracovní nebo školní přihlašovací stránce klikněte na odkaz **Nejde se dostat na váš účet** a pak na **Pracovní nebo školní účet** nebo přejděte přímo na [stránku pro resetování hesla](https://passwordreset.microsoftonline.com/).

    ![Nejde se dostat na váš účet][Login]

2. Zadejte pracovní nebo školní **ID uživatele** a prokažte, že nejste robot, zadáním textu zobrazeného v rámci kontroly CAPTCHA a kliknutím na **Další**.
3. V závislosti na tom, jakou konfiguraci správce vytvořil, se zobrazí jedna nebo více následujících možností.
    * **Zaslat e-mail na mou alternativní e-mailovou adresu** – Odešle e-mail s 6ciferným kódem na vaši alternativní e-mailovou adresu nebo na vaši ověřovací e-mailovou adresu (můžete zvolit požadovanou variantu).
    * **Poslat mi SMS na mobil** – Odešle vám na mobilní telefon nebo na telefon pro ověření 6ciferný kód (můžete zvolit požadovanou variantu).
    * **Zavolat mi na mobilní telefon** – Zavolá na váš mobilní telefon nebo na telefon pro ověření (můžete zvolit požadovanou variantu). Volání ověříte stisknutím tlačítka #.
    * **Zavolat mi na telefon do kanceláře** – Zavolá na váš telefon do kanceláře. Volání ověříte stisknutím tlačítka #.
    * **Odpověď na mé bezpečnostní otázky** – Zobrazí vaše předem registrované bezpečnostní otázky, abyste na ně odpověděli.
4. Vyplňte do požadovaných polí odpovědi na výzvu a klikněte na **Další**.

    ![Ověření ověřovacích dat][Verification]

5. Správce může vyžadovat další krok ověřování, takže může být nutné opakovat krok 4 s jinou možností.
6. Na stránce **Vyberte nové heslo** zadejte nové heslo, které splňuje požadavky vaší organizace, potvrďte heslo a klikněte na **Dokončit**.

    ![Změna hesla][Change]

7. Až se zobrazí zpráva **Vaše heslo bylo resetováno**, můžete se přihlásit pomocí nového hesla.

    ![Vaše heslo bylo resetováno][Complete]

> [!NOTE]
> Pokud správce tuto funkci nepovolil, zobrazí se odkaz „kontaktujte správce“. Správce vám může poskytnout pomoc prostřednictvím e-mailu nebo vlastního webového portálu.
>

Po použití této metody pro odemknutí nebo resetování hesla můžete obdržet e-mail s potvrzením dokončení tohoto procesu odeslaný z účtu jako „Microsoft jménem společnosti vaše_společnost“. Pokud obdržíte takový e-mail a nepoužili jste samoobslužné resetování hesla k opětnému získání přístupu k účtu, kontaktujte správce.

## <a name="change-my-password"></a>Změna hesla

Pokud své heslo již znáte a potřebujete ho změnit, zkuste provést následující kroky.

### <a name="change-your-password-from-the-office-365-portal"></a>Změna hesla z portálu Office 365

1. Klikněte na svůj profil vpravo nahoře a pak klikněte na **Zobrazit účet**.
2. **Zabezpečení a ochrana osobních údajů**
3. **Heslo**
4. Zadejte staré heslo a pak nastavte a potvrďte nové heslo.
5. **Odeslat**

### <a name="change-your-password-from-the-azure-access-panel"></a>Změna hesla na panelu Azure pro přístup

1. Přihlaste se k [portálu Azure pro přístup](https://myapps.microsoft.com/) pomocí stávajícího hesla.
2. Klikněte na svůj profil vpravo nahoře a pak klikněte na **Profil**.
3. **Změnit heslo**
4. Zadejte staré heslo a pak nastavte a potvrďte nové heslo.
5. **Odeslat**

## <a name="next-steps"></a>Další kroky

* [Registrace pro použití samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Přihlašovací stránka Nejde se dostat na váš účet"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Ověření ověřovacích dat"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Změna hesla"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Vaše heslo bylo resetováno"

