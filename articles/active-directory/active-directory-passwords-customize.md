---
title: "Resetování hesla pomocí samoobslužné služby přizpůsobení – Azure Active Directory"
description: "Resetování hesla pomocí samoobslužné služby Azure AD možnosti přizpůsobení"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: a5b4056f3198e998fa7fb127fd2c4bcda90e4bbb
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Přizpůsobení funkce služby Azure AD pro samoobslužné resetování hesla

Odborníci v oblasti IT, kteří chtějí nasadit samoobslužné resetování hesla (SSPR) v Azure Active directory (Azure AD) můžete přizpůsobit prostředí tak, aby odpovídaly potřebám své uživatele.

## <a name="customize-the-contact-your-administrator-link"></a>Přizpůsobení na odkaz "Obraťte se na správce"

I v případě, že SSPR není povolena, uživatelé stále mít "Získáte od správce" odkaz na heslo resetovat portálu. Pokud uživatel vybere tento odkaz se buď:
   * E-maily vašimi správci a je požádá o pomoc při změně hesla uživatele. 
   * Odešle uživatelům na adresu URL, kterou zadáte pro pomoc. 

Doporučujeme nastavit tento kontakt na něco jako e-mailová adresa nebo web, který uživatelé již používat pro dotazy týkající se podpory.

![Obraťte se na][Contact]

Kontaktní e-mail je odeslán následujícím příjemcům v následujícím pořadí:

1. Pokud **heslo správce** role je přiřazená, správci k této roli jsou upozorněni.
2. Pokud žádní správci heslo přiřazené, pak správcům, kteří mají **správce uživatele** role jsou upozorněni.
3. Pokud žádná z předchozích role přiřazené, pak se **globální správci** jsou upozorněni.

Ve všech případech jsou maximálně 100 příjemce oznámení.

Další informace o rolích jiný správce a jejich přiřazení, najdete v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>Zakázat "Obraťte se na správce" e-mailů

Pokud vaše organizace nechce oznámí, že správci o heslo resetovat požadavky, můžete povolit následující konfiguraci:

* Povolte samoobslužné resetování hesla pro všechny uživatele. Tato možnost je v části **resetování hesla** > **vlastnosti**.
  
  Pokud nechcete, aby uživatelům resetovat vlastní hesla, můžete určit obor přístupu k prázdnou skupinou. *Tato možnost nedoporučujeme.*
* Přizpůsobení odkaz helpdesk poskytnout adresu URL webového nebo mailto: adresa, kterou mohou uživatelé získat pomoc. Tato možnost je v části **resetování hesla** > **přizpůsobení** > **vlastní technické podpory e-mailu nebo adresa URL**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>Přizpůsobit přihlašovací stránku služby AD FS pro SSPR

Správci Active Directory Federation Services (AD FS) můžete přidat odkaz na jejich přihlašovací stránku pomocí pokyny v nalezen [přidat přihlašovací stránku popis](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) článku.

Chcete-li přidat odkaz na přihlašovací stránku služby AD FS, použijte následující příkaz na serveru služby AD FS. Uživatelé mohou pomocí této stránky k zadání pracovního postupu SSPR.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Přizpůsobit přihlašovací stránku a přístup k panelu vzhledu a chování

Můžete přizpůsobit přihlašovací stránku. Můžete přidat logo, které se zobrazí spolu s bitovou kopií, které vyhovuje značky společnosti.

Grafické objekty, které zvolíte jsou uvedeny v následujících případech:

* Poté, co uživatel zadá své uživatelské jméno
* Pokud uživatel přistoupí k vlastní adresu URL:
    * Předáním *Wh* parametru heslo resetovat stránky, jako je "https://login.microsoftonline.com/?whr=contoso.com"
    * Předáním *uživatelské jméno* parametru heslo resetovat stránky, jako je třeba "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Podrobnosti o grafiky

Chcete-li změnit vizuální vlastnosti na přihlašovací stránku, použijte následující nastavení. Přejděte na **Azure Active Directory** > **firemní branding** > **úpravy firemního brandingu**:

* Obrázek přihlašovací stránky musí být soubor .png nebo .jpg 1420 × 1200 pixelů a nesmí být větší než 500 KB. Nejlepších výsledků dosáhnete doporučujeme, abyste je přibližně 200 KB.
* Barva pozadí přihlašovací stránce se používá v s vysokou latencí připojení a musí být v šestnáctkovém formátu RGB.
* Bitové kopie banner by měl být soubor .png nebo .jpg, 60 x 280 pixelů a být větší než 10 KB.
* Odmocnina logo (normální a tmavým motivem) by měl být soubor .png nebo .jpg, 240 x 240 pixelů (s možností změny velikosti) a nesmí být větší než 10 KB.

### <a name="sign-in-text-options"></a>Možnosti přihlášení textu

Chcete-li přidat text na stránku přihlášení, které jsou důležité pro vaši organizaci použijte následující nastavení. Přejděte na **Azure Active Directory** > **firemní branding** > **úpravy firemního brandingu**:

* **Pomocný parametr název uživatele**: nahradí text příklad  *someone@example.com*  s něco vhodnější pro vaše uživatele. Doporučujeme ponechat výchozí pomocný parametr, pokud podporujete interních a externích uživatelů.
* **Text přihlašovací stránky**: můžete mít maximálně 256 znaků. Tento text se zobrazí všude, kde uživatelé přihlašovat v online režimu a v prostředí připojení k pracovišti Azure AD na Windows 10. Použijte tento text pro podmínky použití, pokyny a tipy pro vaše uživatele. 

   >[!IMPORTANT]
   >Přihlašovací stránku, takže neposkytují žádné citlivé informace zde obsah uvidí kdokoliv.
   >

### <a name="the-keep-me-signed-in-disabled-setting"></a>Nastavení "Zakázat udržování mi přihlášení"

Pomocí **zůstat přihlášeni zakázáno** možnost, uživatelé mohou zůstat přihlášeného, při jejich zavřete a znovu otevřete okno jejich prohlížeče. Tato možnost nemá negativní vliv na dobu platnosti relace. Přejděte na **Azure Active Directory** > **firemní branding** > **úpravy firemního brandingu**.

Některé funkce služby SharePoint Online a Office 2010 jsou závislé na uživatelé budou moci toto políčko zaškrtněte. Pokud je skrýt tuto možnost, mohou uživatelé získat další a neočekávané přihlášení výzvy.

### <a name="directory-name"></a>Název adresáře

Můžete změnit atribut názvu adresáře v rámci **Azure Active Directory** > **vlastnosti**. Organizace popisný název, který se může zobrazit v portálu a automatizované komunikace. Tato možnost je nejvíce viditelné v automatizovaných e-mailů ve formulářích, které následují:

* Popisný název e-mailu, například "Microsoft jménem ukázkové společnosti CONTOSO"
* Řádek předmětu e-mailu, například "CONTOSO ukázkový e-mailu ověřovací kód účtu"

## <a name="next-steps"></a>Další postup

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
