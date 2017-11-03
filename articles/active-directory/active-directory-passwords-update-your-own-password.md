---
title: "Azure AD: Resetování hesla | Dokumentace Microsoftu"
description: "Použití samoobslužného resetování hesla a získat přístup k pracovního nebo školního účtu uživatele"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: gahug
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8f175f5711b9798153d209f6645aa8bae9f2a00b
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="help-i-forgot-my-azure-ad-password"></a>Potřebujete pomoc, zapomenuté heslo Azure AD

Pokud jste zapomněli své heslo, nikdy jste ho od pracovníků IT neobdrželi, chcete ho změnit nebo vám byl zablokován účet, můžeme vám pomoct. Pokud znáte heslo a právě potřebovat změnit jeho pokračovat dolů k části [změnu hesla](#change-my-password) níže.

   > [!NOTE]
   > Pokud se pokoušíte získat zpátky váš osobní účet jako outlook.com, Xbox a hotmail.com zkuste [návrhy najít v tomto článku](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)
   >

## <a name="reset-or-unlock-my-password-for-a-work-or-school-account"></a>Resetování nebo odemknutí hesla pro pracovní nebo školní účet

Pokud jste nelze získat přístup k účtu vlivem jednoho z následujících akcí:

* Heslo není funkční a chcete ho resetovat.
* Znáte heslo, ale váš účet uzamčen a chcete odemknout svůj účet.

Postupujte podle kroků, které následují pro přístup k resetování hesla pomocí samoobslužné služby Azure AD, SSPR, jak jsme chtěli volání a získat zpátky váš účet.

1. Na kterékoli pracovní nebo školní přihlašovací stránce klikněte na odkaz **Nejde se dostat na váš účet** a pak na **Pracovní nebo školní účet** nebo přejděte přímo na [stránku pro resetování hesla](https://passwordreset.microsoftonline.com/).

    ![Nejde se dostat na váš účet][Login]

2. Zadejte své pracovní nebo školní **ID uživatele**, zadáním znaků zobrazených na obrazovce prokažte, že nejste robot, a klikněte na **Další**.

   > [!NOTE]
   > Pokud vaši pracovníci IT tuto funkci nepovolili, objeví se odkaz „Obraťte se na správce“, aby vám vaši pracovníci IT mohli pomoci přes e-mail nebo vlastní webový portál.
   >
   > Pokud potřebujete k odemknutí účtu, v tomto okamžiku zvolte možnost "I vím svoje heslo, ale stále nemůžete přihlásit".
   >

3. V závislosti na tom, jak pracovníci IT nakonfiguroval SSPR můžete zobrazit jedna nebo více z následujících metod ověřování. Vy nebo pracovníci IT naplnění některé z těchto informací před použitím článku [registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).

   * **Zaslat e-mail na mou alternativní e-mailovou adresu**
   * **Poslat mi SMS na mobil**
   * **Zavolat mi na mobilní telefon**
   * **Zavolat mi na telefon do kanceláře**
   * **Odpověď na mé bezpečnostní otázky**

   Zvolte některou z možností, zadejte správné odpovědi a klikněte na **Další**.

   ![Ověření ověřovacích dat][Verification]

4. Může se stát, že vaši pracovníci IT budou potřebovat další ověření – v takovém případě budete muset opakovat krok 3 a zvolit jinou možnost.
5. Na stránce **Vyberte nové heslo** zadejte nové heslo, potvrďte ho a klikněte na **Dokončit**. Doporučujeme, abyste pro své heslo použili 8–16 znaků, mezi nimiž budou velká a malá písmena, číslice a speciální znaky.
6. Až se zobrazí zpráva **Vaše heslo bylo resetováno**, můžete se přihlásit pomocí nového hesla.

    ![Vaše heslo bylo resetováno][Complete]

Teď byste měli mít přístup ke svému účtu. Pokud ne, měli byste požádat o pomoc pracovníky IT vaší organizace.

Můžete obdržet e-mail s potvrzením, který jste dostali z účtu, jako je "Microsoft jménem \<vaší organizace >". Pokud obdržíte takový e-mail a nepoužili jste samoobslužné resetování hesla k opětnému získání přístupu k účtu, kontaktujte pracovníky IT vaší organizace.

## <a name="change-my-password"></a>Změna hesla

Pokud své heslo již znáte a chcete ho změnit, proveďte to pomocí následujících kroků.

### <a name="change-your-password-from-the-office-365-portal"></a>Změna hesla z portálu Office 365

Tuto metodu použijte, pokud obvykle přistupujete k aplikacím pomocí portálu Office.

1. Přihlaste se k vaší [účtu Office 365](https://www.office.com) pomocí stávající heslo.
2. Klikněte na svůj profil vpravo nahoře a klikněte na **Zobrazit účet**.
3. Klikněte na **Zabezpečení a ochrana osobních údajů** > **Heslo**.
4. Zadejte staré heslo, nastavte a potvrďte nové heslo a klikněte na **Odeslat**.

### <a name="change-your-password-from-the-azure-access-panel"></a>Změna hesla na panelu Azure pro přístup

Tuto metodu použijte, pokud obvykle přistupujete k aplikacím z portálu Azure pro přístup.

1. Přihlaste se k [portálu Azure pro přístup](https://myapps.microsoft.com/) pomocí stávajícího hesla.
2. Klikněte na svůj profil vpravo nahoře a pak klikněte na **Profil**.
3. Klikněte na**Změnit heslo**.
4. Zadejte staré heslo, nastavte a potvrďte nové heslo a klikněte na **Odeslat**.

## <a name="common-problems-and-their-solutions"></a>Běžné problémy a jejich řešení

 Zde jsou některé běžné případy chyba a jejich řešení:

| Chyb| Jaké chyby se zobrazí?| Řešení |
| --- | --- | --- |
| Zobrazí na stránce ", kontaktujte prosím správce" po zadání ID uživatele | Obraťte se na svého správce <br> <br> Zjistili jsme, že heslo vašeho uživatelského účtu nespravuje společnost Microsoft. Snažíme se v důsledku toho nelze automaticky resetovat heslo. <br> <br> Budete muset všechny další pomoc požádejte pracovníci IT. | Tato zpráva se zobrazuje, protože pracovníci oddělení IT spravuje heslo v místním prostředí a neumožňuje resetování hesla z nemůže získat přístup k odkaz na vaši účtu. <br> <br> Pokud chcete resetovat heslo, požádejte pracovníci IT přímo o pomoc a dát jim vědět, že chcete resetovat heslo, takže se můžete povolit tuto funkci můžete.|
| Zobrazí chybu "váš účet není povolen pro resetování hesla" po zadání ID uživatele | Váš účet není povolen pro resetování hesla <br> <br> Je nám líto, ale pracovníci IT nenastavil váš účet pro použití s touto službou. <br> <br> Pokud chcete, můžeme kontaktovat správce ve vaší organizaci k resetování hesla. | Tato zpráva se zobrazuje, protože pracovníci IT nepovolil resetování hesla pro vaší organizace nemůže získat přístup k účtu odkaz na vaši nebo není licencovaná vám umožní používat funkci. <br> <br> Pokud chcete resetovat heslo, klikněte na tlačítko kontakt správce odkaz Odeslat e-mail pro vaši společnost je pracovníky IT a dát jim vědět, které chcete resetovat heslo, takže se můžete povolit tuto funkci můžete. |
| Zobrazí chybu "jsme nemohl ověřit váš účet" po zadání ID uživatele | Nepodařilo se ověřit váš účet <br> <br> Pokud chcete, můžeme kontaktovat správce ve vaší organizaci k resetování hesla. | Tato zpráva se zobrazuje, protože jsou povolené pro resetování hesla, ale nejsou zaregistrované pro používání služby. Chcete-li zaregistrovat pro resetování hesla, přejděte na http://aka.ms/ssprsetup po jste znovu získali přístup ke svému účtu. <br> <br> Pokud chcete resetovat heslo, klikněte na tlačítko kontakt správce odkaz Odeslat e-mail pro vaši společnost zaměstnanců IT. |

## <a name="next-steps"></a>Další kroky

* [Registrace pro použití samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)
* [Nejde se přihlásit k vašemu účtu Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Přihlašovací stránka Nejde se dostat na váš účet"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Ověření ověřovacích dat"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Změna hesla"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Vaše heslo bylo resetováno"
