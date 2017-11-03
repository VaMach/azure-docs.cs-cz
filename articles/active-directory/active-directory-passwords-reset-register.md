---
title: 'Azure AD: Registraci SSPR | Microsoft Docs'
description: "Zaregistrovat data ověření pro hesla pomocí samoobslužné služby Azure AD resetovat"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: b884f8bc3a20052fa0cb40772deef591b69d7b10
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="register-for-self-service-password-reset"></a>Registrace pro samoobslužné resetování hesla

> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).

Jako koncový uživatel můžete resetovat heslo nebo odemknout účet bez nutnosti řeči na uživatele, kteří používají samoobslužné resetování hesla (SSPR). Před použitím této funkce si budete muset zaregistrovat metody ověřování nebo potvrdit předdefinované metody ověřování, které připravil správce.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registrace nebo potvrzení ověřovacích dat pro samoobslužné resetování hesla

1. Otevřete v zařízení webový prohlížeč a přejděte na [registrační stránku pro resetování hesla](http://aka.ms/ssprsetup).
2. Zadejte uživatelské jméno a heslo získané od správce.
3. V závislosti na tom, jak pracovníci IT nakonfigurovali věcí jsou k dispozici pro konfiguraci a ověřte jeden nebo více z následujících možností. Správce může naplnění některé tohoto pro vás, pokud mají oprávnění pro použití informací.
    * Telefon do kanceláře je pouze moci nastavit správce
    * Jiné telefonní číslo, bude mít přístup k mobilní telefon, který může přijímat text nebo volání, jako je třeba nastavit telefon pro ověření.
    * Ověření e-mailu musí být nastaven na alternativní e-mailovou adresu, která mají přístup ke bez heslo, které budete potřebovat resetovat.
    * Bezpečnostní otázky vám poskytne seznam dotazů, které pro vás k odpovědi na schválení správce. Nemusí používat stejnou otázku nebo odpovědět více než jednou.
4. Zadejte a zkontrolujte informace vyžadované správcem. Pokud je k dispozici více než jednu možnost, doporučujeme zaregistrovat několik metod zajistit flexibilitu při další metodou, je k dispozici (Příklad: cestách a nelze získat přístup k váš telefon do kanceláře)

    ![Zaregistrujte si metody ověřování a klikněte na Dokončit][Register].

5. Po dokončení kroku 4 zvolte **Dokončit**. Od této chvíle můžete použít samoobslužné resetování hesla, pokud byste to v budoucnu potřebovali.

Pokud zadáte údaj do pole Telefon pro ověření nebo Ověřovací e-mail, nezobrazí se v globálním adresáři. Tyto údaje se zobrazí jen vám a vašim správcům. Odpovědi na vaše bezpečnostní otázky se zobrazí jen vám.

Správci mohou vyžadovat, abyste metody ověřování po určité době potvrdili a ověřili, že máte stále zaregistrované odpovídající metody.

## <a name="common-problems-and-their-solutions"></a>Běžné problémy a jejich řešení

 Zde jsou některé běžné případy chyba a jejich řešení:

| Chyb| Jaké chyby se zobrazí?| Řešení |
| --- | --- | --- |
| Zobrazí na stránce ", kontaktujte prosím správce" po zadání ID uživatele | Obraťte se na svého správce <br> <br> Zjistili jsme, že heslo vašeho uživatelského účtu nespravuje společnost Microsoft. Snažíme se v důsledku toho nelze automaticky resetovat heslo. <br> <br> Budete muset všechny další pomoc požádejte pracovníci IT. | Tato zpráva se zobrazuje, protože pracovníci oddělení IT spravuje heslo v místním prostředí a neumožňuje resetování hesla z nemůže získat přístup k odkaz na vaši účtu. <br> <br> Pokud chcete resetovat heslo, požádejte pracovníci IT přímo o pomoc a dát jim vědět, že chcete resetovat heslo, takže se můžete povolit tuto funkci můžete.|
| Zobrazí chybu "váš účet není povolen pro resetování hesla" po zadání ID uživatele | Váš účet není povolen pro resetování hesla <br> <br> Je nám líto, ale pracovníci IT nenastavil váš účet pro použití s touto službou. <br> <br> Pokud chcete, můžeme kontaktovat správce ve vaší organizaci k resetování hesla. | Tato zpráva se zobrazuje, protože pracovníci IT nepovolil resetování hesla pro vaší organizace nemůže získat přístup k účtu odkaz na vaši nebo není licencovaná vám umožní používat funkci. <br> <br> Pokud chcete resetovat heslo, klikněte na tlačítko kontakt správce odkaz Odeslat e-mail pro vaši společnost je pracovníky IT a dát jim vědět, které chcete resetovat heslo, takže se můžete povolit tuto funkci můžete. |
| Zobrazí chybu "jsme nemohl ověřit váš účet" po zadání ID uživatele | Nepodařilo se ověřit váš účet <br> <br> Pokud chcete, můžeme kontaktovat správce ve vaší organizaci k resetování hesla. | Tato zpráva se zobrazuje, protože jsou povolené pro resetování hesla, ale nejsou zaregistrované pro používání služby. Chcete-li zaregistrovat pro resetování hesla, přejděte na http://aka.ms/ssprsetup po jste znovu získali přístup ke svému účtu. <br> <br> Pokud chcete resetovat heslo, klikněte na tlačítko kontakt správce odkaz Odeslat e-mail pro vaši společnost zaměstnanců IT. |

## <a name="next-steps"></a>Další kroky

* [Postup při změně hesla prostřednictvím samoobslužného resetování hesla](active-directory-passwords-update-your-own-password.md)
* [Registrační stránka pro resetování hesla](http://aka.ms/ssprsetup)
* [Portál pro resetování hesla](https://passwordreset.microsoftonline.com/)
* [Nejde se přihlásit k vašemu účtu Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Registrační stránka pro resetování hesla se zobrazenými metodami a tlačítkem Dokončit"
