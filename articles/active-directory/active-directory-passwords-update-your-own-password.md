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
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c95f52f028cdfaed35821275d816a24035ff02fb
ms.lasthandoff: 04/25/2017


---
# <a name="help-i-forgot-my-azure-ad-password"></a>Nápověda k zapomenutému heslu Azure AD

Pokud jste zapomněli své heslo, nikdy jste ho od pracovníků IT neobdrželi, chcete ho změnit nebo vám byl zablokován účet, můžeme vám pomoct.

## <a name="reset-or-unlock-my-password-for-a-work-or-school-account"></a>Resetování nebo odemknutí hesla pro pracovní nebo školní účet

Pokud se chcete dostat do svého pracovního nebo školního účtu, pomocí následujících pokynů přejděte k samoobslužnému resetování hesla Azure AD, neboli SSPR, jak tomu říkáme my.

1. Na kterékoli pracovní nebo školní přihlašovací stránce klikněte na odkaz **Nejde se dostat na váš účet** a pak na **Pracovní nebo školní účet** nebo přejděte přímo na [stránku pro resetování hesla](https://passwordreset.microsoftonline.com/).

   > [!NOTE]
   > Pokud se pokoušíte znovu získat přístup k osobního účtu, jako je hotmail.com nebo outlook.com, vyzkoušejte [návrhy uvedené v tomto článku](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
   >
    ![Nejde se dostat na váš účet][Login]

2. Zadejte své pracovní nebo školní **ID uživatele**, zadáním znaků zobrazených na obrazovce prokažte, že nejste robot, a klikněte na **Další**.

   > [!NOTE]
   > Pokud vaši pracovníci IT tuto funkci nepovolili, objeví se odkaz „Obraťte se na správce“, aby vám vaši pracovníci IT mohli pomoci přes e-mail nebo vlastní webový portál.
   >

3. V závislosti na tom, jakou konfiguraci SSPR vaši pracovníci IT vytvořili, se zobrazí jedna nebo více následujících možností. Buď vy nebo vaši pracovníci IT jste vyplnili některé z těchto údajů před použitím informací uvedených [tady](active-directory-passwords-reset-register.md).
    * **Zaslat e-mail na mou alternativní e-mailovou adresu**
    * **Poslat mi SMS na mobil**
    * **Zavolat mi na mobilní telefon**
    * **Zavolat mi na telefon do kanceláře**
    * **Odpověď na mé bezpečnostní otázky**

    Zvolte některou z možností, zadejte správné odpovědi a klikněte na **Další**.

    ![Ověření ověřovacích dat][Verification]

4. Může se stát, že vaši pracovníci IT budou potřebovat další ověření – v takovém případě budete muset opakovat krok 3 a zvolit jinou možnost.
5. Na stránce **Vyberte nové heslo** zadejte nové heslo, potvrďte ho a klikněte na **Dokončit**. Doporučujeme, abyste pro své heslo použili 8–16 znaků, mezi nimiž budou velká a malá písmena, číslice a speciální znaky.

   > [!NOTE]
   > Pokud jste potřebovali odemknout svůj účet, v tomto okamžiku zvolte možnost pouhého odemknutí, nebo změňte své heslo a odemkněte účet.
   >

6. Až se zobrazí zpráva **Vaše heslo bylo resetováno**, můžete se přihlásit pomocí nového hesla.

    ![Vaše heslo bylo resetováno][Complete]

Teď byste měli mít přístup ke svému účtu. Pokud ne, měli byste požádat o pomoc pracovníky IT vaší organizace.

Možná obdržíte potvrzovací e-mail odeslaný z účtu jako „Microsoft jménem společnosti \<vaše společnost>\". Pokud obdržíte takový e-mail a nepoužili jste samoobslužné resetování hesla k opětnému získání přístupu k účtu, kontaktujte pracovníky IT vaší organizace.

## <a name="change-my-password"></a>Změna hesla

Pokud své heslo již znáte a chcete ho změnit, proveďte to pomocí následujících kroků.

### <a name="change-your-password-from-the-office-365-portal"></a>Změna hesla z portálu Office 365

Tuto metodu použijte, pokud obvykle přistupujete k aplikacím pomocí portálu Office.

1. Přihlaste se ke svému [účtu Office 365](https://www.office.com).
2. Klikněte na svůj profil vpravo nahoře a klikněte na **Zobrazit účet**.
3. Klikněte na **Zabezpečení a ochrana osobních údajů** > **Heslo**.
4. Zadejte staré heslo, nastavte a potvrďte nové heslo a klikněte na **Odeslat**.

### <a name="change-your-password-from-the-azure-access-panel"></a>Změna hesla na panelu Azure pro přístup

Tuto metodu použijte, pokud obvykle přistupujete k aplikacím z portálu Azure pro přístup.

1. Přihlaste se k [portálu Azure pro přístup](https://myapps.microsoft.com/) pomocí stávajícího hesla.
2. Klikněte na svůj profil vpravo nahoře a pak klikněte na **Profil**.
3. Klikněte na**Změnit heslo**.
4. Zadejte staré heslo, nastavte a potvrďte nové heslo a klikněte na **Odeslat**.

## <a name="next-steps"></a>Další kroky

* [Registrace pro použití samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)
* [Nejde se přihlásit k vašemu účtu Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Přihlašovací stránka Nejde se dostat na váš účet"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Ověření ověřovacích dat"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Změna hesla"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Vaše heslo bylo resetováno"

