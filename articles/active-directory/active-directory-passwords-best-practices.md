---
title: "Zavedení: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Tipy pro úspěšné zavedení samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: f8cd7e68-2c8e-4f30-b326-b22b16de9787
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 5f3900aef2b432527454da72f3ff15e533543758
ms.contentlocale: cs-cz
ms.lasthandoff: 06/07/2017

---
# Zavedení resetování hesla pro uživatele
<a id="roll-out-password-reset-for-users" class="xliff"></a>

Většina zákazníků pro zajištění bezproblémového zavedení funkce samoobslužného resetování hesla postupuje podle následujících kroků.

1. [Povolte resetování hesla ve svém adresáři](active-directory-passwords-getting-started.md).
2. [Nakonfigurujte oprávnění místní služby AD pro zpětný zápis hesla](active-directory-passwords-how-it-works.md#active-directory-permissions).
3. [Nakonfigurujte zpětný zápis hesel](active-directory-passwords-writeback.md#configuring-password-writeback), aby se hesla z Azure AD zapsala zpátky do místního adresáře.
4. [Přiřaďte a ověřte požadované licence](active-directory-passwords-licensing.md).
5. Chcete-li funkci zavádět postupně, můžete volitelně omezit resetování hesla na skupinu uživatelů. Pokud to chcete provést, nastavte přepínač **Samoobslužné resetování hesla je povoleno** z **Všichni** na **Skupina** a vyberte skupinu zabezpečení, pro kterou chcete resetování hesla povolit. Všichni členové této skupiny musí mít přiřazené licence. Skvělým způsobem, jak to zařídit, je povolit [Licencování na základě skupiny](active-directory-passwords-licensing.md#enable-group-or-user-based-licensing).
6. V závislosti na vašich zásadách naplňte minimální sadu [ověřovacích dat](active-directory-passwords-data.md).
7. Naučte vaše uživatele používat samoobslužné resetování hesla tím, že jim pošlete pokyny s vysvětlením postupu registrace a resetování.
    > [!NOTE]
    > Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesel správců najdete v našem [podrobném článku](active-directory-passwords-how-it-works.md).

8. Můžete zvolit, aby se vždy vynucovala registrace, a aby se po určité době po uživateli vyžadovalo potvrzení ověřovacích informací. Pokud nechcete, aby se vaši uživatelé museli registrovat, můžete [nasadit resetování hesla bez vyžadování registrace koncového uživatele](active-directory-passwords-data.md).
9. V průběhu času můžete uživatele, kteří se registrují a používají tuto funkci, kontrolovat zobrazením [sestav, které poskytuje Azure AD](active-directory-passwords-reporting.md).

## Zavedení přes e-mail
<a id="email-based-rollout" class="xliff"></a>

Pro mnoho zákazníků je nejjednodušším způsobem, jak uživatele přimět používat samoobslužné resetování hesla, e-mailová kampaň se snadno použitelnými pokyny. [Vytvořili jsme tři jednoduché e-maily, které můžete použít jako šablony pro pomoc se zaváděním.](https://onedrive.live.com/?authkey=%21AD5ZP%2D8RyJ2Cc6M&id=A0B59A91C740AB16%2125063&cid=A0B59A91C740AB16)

* Šablonu e-mailu **Připravuje se** použijte během týdnů nebo dnů před zavedením, abyste uživatelům sdělili, že musí něco udělat.
* Šablonu e-mailu **Nyní dostupné** použijte v den spuštění, abyste přiměli uživatele k registraci a potvrzení ověřovacích dat, aby v případě potřeby mohli použít samoobslužné resetování hesla.
* Šablonu e-mailu **Připomenutí registrace** použijte několik dnů nebo týdnů po nasazení pro připomenutí uživatelům, aby se zaregistrovali a potvrdili ověřovací data.

## Vytvoření vlastního portálu hesel
<a id="creating-your-own-password-portal" class="xliff"></a>

Řada našich větších zákazníku volí hostování webové stránky a vytvoření kořenového záznamu DNS, například https://passwords.contoso.com. Tuto stránku naplní odkazy na portály pro resetování hesla Azure AD, registraci k resetování hesla, změnu hesla a další informace specifické pro organizaci. Do e-mailů nebo letáků, které budete rozesílat, pak můžete vložit zapamatovatelnou adresu URL obsahující značku, na kterou uživatelé můžou přejít v případě, že tyto služby potřebují využít.

* Portál pro resetování hesla – https://passwordreset.microsoftonline.com/
* Portál pro registraci k resetování hesla – http://aka.ms/ssprsetup
* Portál pro změnu hesla – https://account.activedirectory.windowsazure.com/ChangePassword.aspx

## Použití vynucené registrace
<a id="using-enforced-registration" class="xliff"></a>

Pokud chcete, aby se vaši uživatelé zaregistrovali k resetování hesla, může je k registraci přinutit při přihlášení pomocí Azure. Tuto možnost můžete povolit v okně **Resetování hesla** vašeho adresáře tak, že na kartě **Registrace** povolíte možnost **Při přihlášení vyžadovat registraci uživatelů**.

Správci můžou nastavením možnosti **Počet dní před vyzváním uživatelů k potvrzení ověřovacích údajů** na hodnotu mezi 0 až 730 dny vyžadovat, aby se uživatelé po určité době zaregistrovali znovu.

Po povolení této možnosti se uživatelům, kteří se přihlašují, zobrazí zpráva s informací, že po nich správce vyžaduje potvrzení ověřovacích informací.

## Naplnění ověřovacích dat
<a id="populate-authentication-data" class="xliff"></a>

Pokud [pro své uživatele naplníte ověřovací data](active-directory-passwords-data.md), nebudou se muset registrovat k resetování hesla, aby mohli využívat samoobslužné resetování hesla. Dokud uživatelé budou mít definovaná ověřovací data splňující zásady resetování hesla, které jste definovali, budou si moct resetovat svá hesla.

## Zakázání samoobslužného resetování hesla
<a id="disabling-self-service-password-reset" class="xliff"></a>

Zakázání samoobslužného resetování hesla je snadné – stačí otevřít vašeho tenanta Azure AD, přejít na **Resetování hesla**, **Vlastnosti** a v části **Samoobslužné resetování hesla je povoleno** zvolit **Nikdo**.

## Další kroky
<a id="next-steps" class="xliff"></a>

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? – Odpovědi na otázky, na které jste se vždy chtěli zeptat.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.
