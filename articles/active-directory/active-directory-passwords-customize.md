---
title: "Přizpůsobení: Azure AD SSPR | Microsoft Docs"
description: "Možnosti přizpůsobení pro Azure AD samoobslužném resetování hesla služby"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: f2b172208185e343c9c10d55036c20d60346778c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Přizpůsobení funkce služby Azure AD pro samoobslužného resetování hesel

Odborníci v oblasti IT chtějí nasadit samoobslužné resetování hesla můžete přizpůsobit prostředí tak, aby odpovídaly uživatelů.

## <a name="customize-the-contact-your-administrator-link"></a>Přizpůsobení kontakt odkaz na vaši správce

I když není povolená SSPR uživatelé stále "obraťte se na správce" odkaz na heslo resetovat portálu.  Kliknutím na tento odkaz e-maily správce žádostí o pomoc při změně hesla nebo odešle uživatelům na adresu URL, který určíte. Doporučujeme vám, že nastavíte něco jako e-mailová adresa nebo web, který uživatelé používají k použití pro podporu.

![Obraťte se na][Contact]

Tento e-mail je odeslán následujícím příjemcům v následujícím pořadí:

1. Pokud **heslo správce** role je přiřazená, správci k této roli jsou upozorněni.
2. Pokud žádní správci heslo přiřazené, pak správcům, kteří mají **správce uživatele** role jsou upozorněni.
3. Pokud žádná z předchozích role byly přiřazeny, pak **globální správci** jsou oznámení

Ve všech případech jsou maximálně 100 příjemce oznámení.

Chcete-li získat další informace o různých správce rolí a jejich přiřazení naleznete v dokumentu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

### <a name="disable-contact-your-administrator-emails"></a>Obraťte se na vaši e-mailů správce zakázat

Pokud vaše organizace nechce, že správci oznámení o heslo resetovat požadavky, můžete povolit následující konfigurace

* Povolte samoobslužné resetování hesla pro všechny uživatele. Tato možnost je v části **resetování hesla > vlastnosti**.
    * Pokud nechcete, aby uživatelům resetovat vlastní hesla, můžete určit obor přístupu do prázdné skupiny **nedoporučujeme tuto možnost**.
* Přizpůsobení odkaz helpdesk poskytnout adresu URL webového nebo mailto: adresa, kterou mohou uživatelé získat pomoc. Tato možnost je v části **resetování hesla > Přizpůsobení > vlastní technické podpory e-mailu nebo adresa URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Přizpůsobit přihlašovací stránku služby AD FS pro SSPR

Správci služby AD FS můžete přidat odkaz na jejich přihlašovací stránku pomocí pokyny naleznete v článku [přidat přihlašovací stránku popis](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Pomocí příkazu, který odpovídá na serveru AD FS přidá odkaz na přihlašovací stránku služby AD FS umožňuje uživatelům zadat heslo samoobslužné služby pracovního postupu pro obnovení přímo.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Přizpůsobení přihlašování a přístup k panelu vzhledu a chování

Pokud vaši uživatelé přístup k přihlašovací stránky, můžete přizpůsobit logo, které se zobrazí spolu s bitovou kopií přihlašovací stránce podle značky společnosti.

Tato grafika jsou uvedeny v následujících případech:

* Poté, co uživatel zadá své uživatelské jméno
* Uživatel získá přístup k vlastní adresu url
    * Předáním "Wh" parametru heslo resetovat stránky, jako je "https://login.microsoftonline.com/?whr=contoso.com"
    * Předáním "username" parametru heslo resetovat stránky, jako je třeba "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Podrobnosti o grafiky

Tato nastavení umožňují změnit vizuální vlastnosti na přihlašovací stránku a najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**

* Obrázek přihlašovací stránky by měl být PNG nebo JPG souboru 1420 × 1200 pixelů a ne větší než 500KB. Doporučujeme, abyste ho na přibližně 200 KB pro dosažení co nejlepších výsledků.
* Barva pozadí přihlašovací stránky se používá v s vysokou latencí připojení a musí být v šestnáctkovém formátu RGB.
* Obrázek hlavičky by měl být PNG nebo JPG souboru 60 x 280 pixelů a ne větší než 10 KB.
* Odmocnina logo (normální a tmavým motivem) PNG nebo JPG 240 x 240 (s možností změny velikosti) nesmí být větší než 10 KB.

### <a name="sign-in-text-options"></a>Možnosti přihlášení textu

Tato nastavení umožňují přidat text na přihlašovací stránku pro vaši organizaci relevantní. Tato nastavení najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**

* **Pomocný parametr název uživatele** nahradí text příklad someone@example.com s něco vhodnější pro vaše uživatele, doporučuje ponechat výchozí při podpoře interních a externích uživatelů
* **Text přihlašovací stránky** je maximálně 256 znaků. Tento text se zobrazí odkudkoli přihlašování uživatelů online a v prostředí Azure AD Join ve Windows 10. Použijte tento text pro podmínky použití, pokyny a tipy pro vaše uživatele. **Kdo, zobrazit stránku přihlášení, neposkytují žádné citlivé informace v tomto poli.**

### <a name="keep-me-signed-in-disabled"></a>Zůstat přihlášeni zakázáno

Možnost "Keep mi přihlášení zakázáno" umožňuje uživatelům zůstanou přihlášeného při jejich zavřete a znovu otevřete okno jejich prohlížeče. Tato možnost neměla vliv trvání relace. Toto nastavení je v části **Azure Active Directory > firemní branding > Upravit firemního brandingu**.

Některé funkce služby SharePoint Online a Office 2010 jsou závislé na uživatelé moct toto políčko zaškrtněte. Pokud je skrýt tuto možnost, mohou uživatelé získat další a neočekávané přihlášení výzvy.

### <a name="directory-name"></a>Název adresáře

Můžete změnit atribut názvu pod **Azure Active Directory > vlastnosti** zobrazíte organizace popisný název vidět v portálu a automatizované komunikaci. Tato možnost je nejvíce viditelné ve formě automatické e-maily ve formulářích, které následují

* Popisný název v e-mailu "Microsoft jménem ukázkové společnosti CONTOSO"
* Řádek předmětu e-mailem "CONTOSO ukázkový e-mailu ověřovací kód účtu"

## <a name="next-steps"></a>Další kroky

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a která data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Požádejte správce o pomoc resetovat vaše heslo e-mailu příklad"