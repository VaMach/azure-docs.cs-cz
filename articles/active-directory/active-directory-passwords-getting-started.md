---
title: "Rychlý start: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Rychlé nasazené samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d97fad04a0aa549d0e3a182282f898302e8e41a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Rychlé nasazení samoobslužného resetování hesla Azure AD

> [!IMPORTANT]
> **Jste tady, protože máte potíže s přihlášením?** Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).

Samoobslužné resetování hesla nabízí správcům IT jednoduchý způsob, jak umožnit uživatelům resetovat nebo odemknout svá hesla nebo účty. Tento systém zahrnuje vytváření podrobných sestav, pomocí kterých můžete sledovat, kdy uživatelé systém používají, spolu s oznámeními, která upozorňují na zneužití.

Tento průvodce předpokládá, že již máte funkčního tenanta Azure AD ve zkušební verzi nebo s licencí. Pokud potřebujete pomoc s nastavením Azure AD, přečtěte si článek [Začínáme s Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>Povolení SSPR pro tenanta Azure AD

1. Ve svém existujícím tenantovi Azure AD vyberte **Resetování hesla**.

2. Na obrazovce **Vlastnosti** v části Samoobslužné resetování hesla povoleno zvolte jednu z následujících možností:
    * Žádné – Funkci samoobslužného resetování hesla nemůže využívat nikdo.
    * Vybrané – Funkci samoobslužného resetování hesla mohou využívat jenom členové určité skupiny Azure AD, kterou zvolíte. Doporučujeme definovat skupinu uživatelů a toto nastavení použít při nasazení pro testování konceptu.
    * Všichni – Funkci samoobslužného resetování hesla mohou využívat všichni uživatelé s účty ve vašem tenantovi Azure AD. Toto nastavení doporučujeme použít, až budete po dokončení testování konceptu připraveni nasadit tuto funkci do celého tenanta.

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
            
    ![Ověřování][Authentication]

4. DOPORUČENÉ: **Přizpůsobení** umožňuje změnit odkaz „Kontaktujte správce“, aby odkazoval na stránku nebo e-mailovou adresu, kterou zadáte. Doporučujeme nastavit tento odkaz na e-mailovou adresu nebo web, které jsou vaši uživatelé zvyklí používat pro podporu.

5. VOLITELNÉ: Obrazovka **Registrace** poskytuje správcům tyto možnosti:
    * Při přihlášení vyžadovat registraci uživatelů
    * Počet dní před vyzváním uživatelů k potvrzení ověřovacích informací

6. VOLITELNÉ: Obrazovka **Oznámení** poskytuje správcům tyto možnosti:
    * Upozornit uživatele na resetování hesla
    * Upozornit všechny správce na resetování hesla jiného správce

**Právě jste pro svého tenanta Azure AD nakonfigurovali samoobslužné resetování hesla**. Uživatelé teď mohou použít pokyny uvedené v článcích [Registrace pro samoobslužné resetování hesla](active-directory-passwords-reset-register.md) a [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md) a aktualizovat svoje heslo bez zásahu správce. Tady můžete skončit, pokud pracujete jenom v cloudu, nebo můžete pokračovat a nakonfigurovat synchronizaci hesel s místní doménou AD.

> [!IMPORTANT]
> Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování. Další informace týkající se zásad hesel správců najdete v našem [článku o zásadách hesel](active-directory-passwords-policy.md#administrator-password-policy-differences).

## <a name="configure-synchronization-to-existing-identity-source"></a>Konfigurace synchronizace s existujícím zdrojem identit

Pokud chcete povolit místní synchronizaci identit s Azure AD, je nutné na serveru ve vaší organizaci nainstalovat a nakonfigurovat službu [Azure AD Connect](./connect/active-directory-aadconnect.md). Tato aplikace se stará o synchronizaci uživatelů a skupin mezi vaším existujícím zdrojem identit a vaším tenantem Azure AD.

* [Upgrade z nástroje DirSync nebo z Azure AD Sync na Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Začínáme se službou Azure AD Connect s použitím expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md)
* [Nakonfigurujte zpětný zápis hesel](active-directory-passwords-writeback.md#configuring-password-writeback), aby se hesla z Azure AD zapsala zpátky do místního adresáře.

### <a name="on-premises-policy-change"></a>Změna místních zásad

Pokud synchronizujete uživatele z místní domény služby Active Directory a chcete jim okamžitě povolit resetováních vlastních hesel, v místních zásadách hesel proveďte následující změnu:

**Konfigurace počítače** > **Zásady** > **Nastavení systému Windows** > **Nastavení zabezpečení**  >  **Zásady účtů** > **Zásady hesel**

**Minimální stáří hesla** – 0 dní

Toto nastavení zabezpečení určuje dobu (ve dnech), po kterou se heslo musí používat, než ho uživatel může změnit. Nastavení na hodnotu **0 dní** umožňuje uživatelům použít SSPR, pokud jejich hesla změní týmy podpory.

![Zásady][Policy]

## <a name="disabling-self-service-password-reset"></a>Zakázání samoobslužného resetování hesla

Zakázání samoobslužného resetování hesla je snadné – stačí otevřít vašeho tenanta Azure AD, přejít na **Resetování hesla > Vlastnosti** a v části **Samoobslužné resetování hesla povoleno** zvolit **Žádné**.

### <a name="learn-more"></a>Další informace
Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

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

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak pro vaše uživatele nakonfigurovat samoobslužné resetování hesla. Pokud chcete pokračovat na Azure Portal a dokončit tyto kroky, použijte následující odkaz na portál.

> [!div class="nextstepaction"]
> [Povolení samoobslužného resetování hesla](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Dostupné metody ověřování Azure AD a požadované množství"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Nastavení místních zásad hesel na 0 dnů"
