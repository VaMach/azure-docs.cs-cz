---
title: "Rychlý start: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Rychlé nasazené samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: c41a16a3c43064a094b0980bc1a8294b093c6130
ms.contentlocale: cs-cz
ms.lasthandoff: 08/08/2017

---
# <a name="quickstart-azure-ad-self-service-password-reset"></a>Rychlý start: Samoobslužné resetování hesla Azure AD

> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).

## <a name="rapidly-deploy-self-service-password-reset"></a>Rychlé nasazené samoobslužného resetování hesla

Samoobslužné resetování hesla nabízí správcům IT jednoduchý způsob, jak umožnit uživatelům resetovat nebo odemknout svá hesla nebo účty. Tento systém zahrnuje vytváření podrobných sestav, pomocí kterých můžete sledovat, kdy uživatelé systém používají, spolu s oznámeními, která upozorňují na zneužití.

Tento průvodce předpokládá, že již máte funkčního tenanta Azure AD ve zkušební verzi nebo s licencí. Pokud potřebujete pomoc s nastavením Azure AD, přečtěte si článek [Začínáme s Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Ve svém existujícím tenantovi Azure AD vyberte **Resetování hesla**.

2. Na obrazovce **Vlastnosti** v části „Samoobslužné resetování hesla je povoleno“ zvolte jednu z následujících možností:
    * Nikdo – Nikdo nemůže využívat funkci samoobslužného resetování hesla.
    * Skupina – Pouze členové určité skupiny Azure AD, kterou zvolíte, můžou využívat funkci samoobslužného resetování hesla.
    * Všichni – Všichni uživatelé s účty ve vašem tenantovi Azure AD můžou využívat funkci samoobslužného resetování hesla.

3. Na obrazovce **Metody ověření** zvolte
    * Počet metod požadovaných k resetování – Podporujeme minimálně jednu nebo maximálně dvě.
    * Metody dostupné pro uživatele – Potřebujeme alespoň jednu, ale nikdy neuškodí mít k dispozici jednu možnost navíc.
        * **E-mail** odešle e-mail s kódem na nastavenou ověřovací e-mailovou adresu uživatele.
        * **Mobilní telefon** umožňuje uživateli obdržet hovor nebo zprávu s kódem na nastavené číslo mobilního telefonu.
        * **Telefon do kanceláře** zavolá uživateli na nastavené telefonní číslo do kanceláře a sdělí mu kód.
        * **Bezpečnostní otázky** vyžadují, abyste zvolili:
            * Počet otázek požadovaných k registraci – Minimum pro úspěšnou registraci. To znamená, že uživatel může odpovědět na více otázek a vytvořit tak fond, ze kterého se otázky budou načítat. Tuto možnost můžete nastavit na 3 až 5 a tato hodnota musí být vyšší nebo rovna počtu otázek požadovaných k resetování.
                * Vlastní otázky můžete přidat kliknutím na tlačítko Vlastní při výběru bezpečnostních otázek.
            * Počet otázek požadovaných k resetování – Můžete nastavit na 3 až 5 otázek, které je třeba správně zodpovědět, než bude umožněno resetování nebo odemknutí hesla uživatele.

4. DOPORUČENÉ: **Přizpůsobení** umožňuje změnit odkaz „Kontaktujte správce“, aby odkazoval na stránku nebo e-mailovou adresu, kterou zadáte.

5. VOLITELNÉ: Obrazovka **Registrace** poskytuje správcům tyto možnosti:
    * Při přihlášení vyžadovat registraci uživatelů
    * Počet dní před vyzváním uživatelů k potvrzení ověřovacích informací

6. VOLITELNÉ: Obrazovka **Oznámení** poskytuje správcům tyto možnosti:
    * Upozornit uživatele na resetování hesla
    * Upozornit všechny správce na resetování hesla jiného správce

**Právě jste pro svého tenanta Azure AD nakonfigurovali samoobslužné resetování hesla**. Tady můžete skončit nebo pokračovat a nakonfigurovat synchronizaci hesel s místní doménou AD.

> [!NOTE]
> Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesel správců najdete v našem [článku o zásadách hesel](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurace synchronizace s existujícím zdrojem identit

Pokud chcete povolit místní synchronizaci identit s Azure AD, je nutné na serveru ve vaší organizaci nainstalovat a nakonfigurovat službu [Azure AD Connect](./connect/active-directory-aadconnect.md). Tato aplikace se stará o synchronizaci uživatelů a skupin mezi vaším existujícím zdrojem identit a vaším tenantem Azure AD.

* [Upgrade z nástroje DirSync nebo z Azure AD Sync na Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Začínáme se službou Azure AD Connect s použitím expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md)
* [Nakonfigurujte zpětný zápis hesel](active-directory-passwords-writeback.md#configuring-password-writeback), aby se hesla z Azure AD zapsala zpátky do místního adresáře.

## <a name="disabling-self-service-password-reset"></a>Zakázání samoobslužného resetování hesla

Zakázání samoobslužného resetování hesla je snadné – stačí otevřít vašeho tenanta Azure AD, přejít na **Resetování hesla > Vlastnosti** a v části **Samoobslužné resetování hesla povoleno** zvolit **Žádné**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak pro vaše uživatele nakonfigurovat samoobslužné resetování hesla. Pokud chcete pokračovat na Azure Portal a dokončit tyto kroky, použijte následující odkaz na portál.

> [!div class="nextstepaction"]
> [Povolení samoobslužného resetování hesla](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.
* [**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? – Odpovědi na otázky, na které jste se vždy chtěli zeptat.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.

