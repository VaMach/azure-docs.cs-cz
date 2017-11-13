---
title: "Zavedení: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Tipy pro úspěšné zavedení samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0be1616f5df915e566dc73c15dbea2e53177aa1c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="how-to-successfully-rollout-self-service-password-reset"></a>Úspěšné zavedení samoobslužného resetování hesla

Většina zákazníků pro zajištění bezproblémového zavedení funkce samoobslužného resetování hesla postupuje podle následujících kroků.

1. [Povolte resetování hesla ve svém adresáři](active-directory-passwords-getting-started.md).
2. [Nakonfigurujte oprávnění místní služby AD pro zpětný zápis hesla](active-directory-passwords-writeback.md#active-directory-permissions).
3. [Nakonfigurujte zpětný zápis hesel](active-directory-passwords-writeback.md#configuring-password-writeback), aby se hesla z Azure AD zapsala zpátky do místního adresáře.
4. [Přiřaďte a ověřte požadované licence](active-directory-passwords-licensing.md).
5. Pokud chcete SSPR zavádět postupně, můžete omezit přístup ke skupině uživatelů a provést s nimi pilotní nasazení. Pokud to chcete provést, nastavte přepínač **Samoobslužné resetování hesla je povoleno** na **Vybráno** a vyberte skupinu zabezpečení, pro kterou chcete resetování hesla povolit. 
6. Naplňte [ověřovací data](active-directory-passwords-data.md) pro uživatele, například jejich telefonní číslo do kanceláře, mobilní telefon a alternativní e-mailovou adresu.
7. [Upravte prostředí přihlášení Azure AD, aby obsahovalo značku vaší společnosti](active-directory-passwords-customize.md).
8. Naučte vaše uživatele používat samoobslužné resetování hesla tím, že jim pošlete pokyny s vysvětlením postupu registrace a resetování.
9. Můžete zvolit, aby se vždy vynucovala registrace, a aby se po určité době po uživateli vyžadovalo potvrzení ověřovacích informací.
10. V průběhu času můžete uživatele, kteří se registrují a používají tuto funkci, kontrolovat zobrazením [sestav, které poskytuje Azure AD](active-directory-passwords-reporting.md).
11. Až budete připravení, povolte resetování hesla pro všechny uživatele tak, že přepnete **Samoobslužné resetování hesla povoleno** na **Všichni**. 

   > [!IMPORTANT]
   > Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesel správců najdete v našem [článku o zásadách hesel](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>Zavedení přes e-mail

Pro mnoho zákazníků je nejjednodušším způsobem, jak uživatele přimět používat samoobslužné resetování hesla, e-mailová kampaň se snadno použitelnými pokyny. [Vytvořili jsme tři jednoduché e-maily, které můžete použít jako šablony pro pomoc se zaváděním.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* Šablonu e-mailu **Připravuje se** použijte během týdnů nebo dnů před zavedením, abyste uživatelům sdělili, že musí něco udělat.
* Šablonu e-mailu **Nyní dostupné** použijte v den spuštění, abyste přiměli uživatele k registraci a potvrzení ověřovacích dat, aby v případě potřeby mohli použít samoobslužné resetování hesla.
* Šablonu e-mailu **Připomenutí registrace** použijte několik dnů nebo týdnů po nasazení pro připomenutí uživatelům, aby se zaregistrovali a potvrdili ověřovací data.

![E-mail][Email]

## <a name="creating-your-own-password-portal"></a>Vytvoření vlastního portálu hesel

Řada našich větších zákazníku volí hostování webové stránky a vytvoření kořenového záznamu DNS, například https://passwords.contoso.com. Tuto stránku naplní odkazy na portály pro resetování hesla Azure AD, registraci k resetování hesla, změnu hesla a další informace specifické pro organizaci. Do e-mailů nebo letáků, které budete rozesílat, pak můžete vložit zapamatovatelnou adresu URL obsahující značku, na kterou uživatelé můžou přejít v případě, že tyto služby potřebují využít.

* Portál pro resetování hesla – https://aka.ms/sspr
* Portál pro registraci k resetování hesla – http://aka.ms/ssprsetup
* Portál pro změnu hesla – https://account.activedirectory.windowsazure.com/ChangePassword.aspx

Vytvořili jsme pro vás ukázkovou stránku, kterou můžete použít a přizpůsobit potřebám vaší organizace a kterou si můžete stáhnout z [GitHubu](https://github.com/ajamess/password-reset-page).

## <a name="using-enforced-registration"></a>Použití vynucené registrace

Pokud chcete, aby se vaši uživatelé zaregistrovali k resetování hesla, může je k registraci přinutit při přihlášení pomocí Azure. Tuto možnost můžete povolit v okně **Resetování hesla** vašeho adresáře tak, že na kartě **Registrace** povolíte možnost **Při přihlášení vyžadovat registraci uživatelů**.

Správci můžou nastavením možnosti **Počet dní před vyzváním uživatelů k potvrzení ověřovacích údajů** na hodnotu mezi 0 až 730 dny vyžadovat, aby se uživatelé po určité době zaregistrovali znovu.

Po povolení této možnosti se uživatelům, kteří se přihlašují, zobrazí zpráva s informací, že po nich správce vyžaduje potvrzení ověřovacích informací.

## <a name="populate-authentication-data"></a>Naplnění ověřovacích dat

Pokud [pro své uživatele naplníte ověřovací data](active-directory-passwords-data.md), nebudou se muset registrovat k resetování hesla, aby mohli využívat samoobslužné resetování hesla. Dokud uživatelé budou mít definovaná ověřovací data splňující zásady resetování hesla, které jste definovali, budou si moct resetovat svá hesla.

## <a name="disabling-self-service-password-reset"></a>Zakázání samoobslužného resetování hesla

Zakázání samoobslužného resetování hesla je snadné – stačí otevřít vašeho tenanta Azure AD, přejít na **Resetování hesla**, **Vlastnosti** a v části **Samoobslužné resetování hesla je povoleno** zvolit **Nikdo**.

## <a name="next-steps"></a>Další kroky

* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a jaká data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která nebyla zodpovězena jinde](active-directory-passwords-faq.md)

[Email]: ./media/active-directory-passwords-best-practices/sspr-emailtemplates.png "Přizpůsobení těchto e-mailových šablon podle požadavků vaší organizace"