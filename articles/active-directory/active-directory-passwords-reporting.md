---
title: "Vytváření sestav: Azure AD SSPR | Microsoft Docs"
description: "Zprávy o hesla pomocí samoobslužné služby Azure AD resetovat události"
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
ms.openlocfilehash: e4524704c6db0d21388ea407870c65d4f69a6323
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Možnosti zasílání zpráv o správou hesel Azure AD

Po nasazení mnoho organizací bude chtít vědět, jak nebo pokud SSPR je doopravdy používá. Azure AD poskytuje vytváření sestav funkce, které pomáhají při odpovědi na otázky pomocí specifických sestav a pokud máte správně licenci, umožňují vytvářet vlastní dotazy.

![Vytváření sestav][Reporting]

Sestavy, které existují v [portál Azure] může odpovědět na následující otázky (https://portal.azure.com/).

> [!NOTE]
> Musí být [globální správce](active-directory-assign-admin-roles-azure-portal.md) a musí výslovný souhlas pro tato data shromáždit jménem vaší organizace, navštivte reporting kartě nebo auditu protokoly alespoň jednou. Až to uděláte, nebudou shromažďovány dat pro vaši organizaci

* Kolik lidí registrovali pro resetování hesla?
* Kdo má zaregistrovat pro resetování hesla?
* Jaká data jsou osoby registraci?
* Kolik uživatelů resetovat vlastní hesla v posledních sedmi dnů?
* Jaké jsou nejběžnější metody uživatelé nebo správci použít k resetování hesla?
* Jaké jsou běžné problémy uživatelé nebo správci setkávají při pokusu o použití resetování hesla?
* Co správci jsou často resetovat vlastní hesla?
* Je k dispozici podezřelé aktivity přejdete k resetování hesla?

## <a name="power-bi-content-pack"></a>Balíček obsahu Power BI

Pokud uživatelé Power BI je balíček obsahu pro Azure AD, která obsahuje snadno použitelný vytváření sestav pro SSPR. Najít další informace o tom, jak používat a nasaďte balíček obsahu v článku [postup použití Azure Active Directory Power BI obsah Pack](active-directory-reporting-power-bi-content-pack-how-to.md). Potom můžete vytvořit vlastní řídicí panely a sdílet je s ostatními uživateli ve vaší organizaci.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Postup zobrazení sestav správy hesel na portálu Azure

V portálu Azure prostředí budeme mít lepší způsob zobrazení resetování hesla a registrace aktivita resetování hesla.  Použijte následující postup a zjistit, že resetování hesla a heslo resetovat registrace události:

1. Přejděte na [ **portal.azure.com**](https://portal.azure.com)
2. Klikněte na tlačítko **další služby** na hlavní Azure portálu levé navigační nabídky
3. Vyhledejte **Azure Active Directory** v seznamu služeb a vyberte ho
4. Klikněte na tlačítko **uživatelé a skupiny** v navigační nabídce Azure Active Directory
5. Klikněte **protokoly auditu** navigační položka v navigační nabídce Uživatelé a skupiny. Toto jsou zobrazeny všechny události auditu, ke kterým dochází u všech uživatelů ve vašem adresáři. Toto zobrazení zobrazíte všechny související s hesly události, také můžete filtrovat.
6. Chcete-li filtrovat tohoto zobrazení a jen heslo resetovat související události, klikněte na tlačítko **filtru** tlačítka v horní části okna.
7. Z **filtru** nabídce vyberte možnost **kategorie** rozevíracího seznamu a změňte ji na **Samoobslužná správa hesel** typ kategorie.
8. Volitelně další filtrování seznamu výběrem konkrétní **aktivity** vás zajímá

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Jak načíst heslo správy události z sestav Azure AD a události rozhraní API

Sestavy služby Azure AD a události rozhraní API podporuje načítání všechny informace, které jsou zahrnuty v sestavách registrace resetování hesla pro obnovení a heslo. Pomocí tohoto rozhraní API si můžete stáhnout resetování hesla jednotlivých a registrace události pro integraci v rámci technologie vytváření sestav podle svého výběru pro resetování hesla.

### <a name="how-to-get-started-with-the-reporting-api"></a>Jak začít pracovat s rozhraním API pro generování sestav

Pro přístup k těmto datům, budete muset zápisu malých aplikace nebo skriptu načíst z našich serverech. [Zjistěte, jak začít pracovat s rozhraním API pro Azure AD Reporting](active-directory-reporting-api-getting-started.md).

Až budete mít skript práci, budete dále chcete prověřit heslo resetování a registraci události, které můžete načíst do splňují vaše scénáře.

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): seznam sloupců, které jsou k dispozici pro resetování hesla události
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): seznam sloupců, které jsou k dispozici pro události registrace pro resetování hesla

### <a name="reporting-api-data-retrieval-limitations"></a>Omezení generování sestav načtení dat rozhraní API

V současné době sestav Azure AD a rozhraní API událostí načte až **než 75 000 jednotlivé události** z [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) a [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) typů pokrývání uzlů **posledních 30 dní**.

Pokud budete potřebovat načíst nebo ukládání dat nad rámec tohoto okna, doporučujeme uchování v externí databáze a dotaz rozdílů, kterých se pomocí rozhraní API. Naše doporučení je zahájit načítání tato data po spuštění pomocí SSPR ve vaší organizaci, je zachování externě a poté pokračujte ke sledování se rozdíly od tohoto okamžiku.

## <a name="description-of-report-columns-in-azure-portal"></a>Popis sloupce sestavy na portálu Azure

Následující seznam popisuje všechny sloupce sestavy podrobně:

* **Uživatel** – uživatel, který se pokusil heslo resetovat operace Registrování.
* **Role** – role uživatele v adresáři.
* **Datum a čas** – datum a čas pokus.
* **Data registrovaná** – registrace pro resetování jaká data ověřování uživatele zadali během heslo.

## <a name="description-of-report-values-in-azure-portal"></a>Popis hodnoty sestavy na portálu Azure

Následující tabulka popisuje různé hodnoty pro každý sloupec povolená:

| Sloupec | Povolené hodnoty a jejich významů |
| --- | --- |
| Data zaregistrován |**Alternativní e-mailu** – uživatel používá k ověření alternativní e-mailu nebo ověřování e-mailu<p><p>**Telefon do kanceláře**– telefonní číslo do kanceláře uživatel použitý k ověření<p>**Mobilní telefon** -uživatele použít mobilní telefon nebo telefon pro ověření k ověření<p>**Bezpečnostní otázky** – uživatel používá k ověření bezpečnostních otázek<p>**Libovolnou kombinaci výše (například alternativní e-mailovou + mobilní telefon)** – nastane, když je zadán zásadu 2 brány a ukazuje které dvě metody uživatel použitý k ověření žádost o resetování hesla. |

## <a name="self-service-password-management-activity-types"></a>Samoobslužné služby typy aktivit správy hesel

Následující typy aktivit v zobrazí **Samoobslužná správa hesel** kategorie události auditu.  Popis pro každou způsobem.

* [**Blokovat samoobslužné resetování hesla** ](#activity-type-blocked-from-self-service-password-reset) -Určuje, uživatel se pokusil obnovit heslo, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* [**Změnit heslo (samoobslužné)** ](#activity-type-change-password-self-service) -označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* [**Resetovat heslo (správcem)** ](#activity-type-reset-password-by-admin) -označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* [**Resetovat heslo (samoobslužné)** ](#activity-type-reset-password-self-service) -určuje uživatel úspěšně resetovat heslo služby [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* [**Resetování hesla pomocí samoobslužné služby průběh aktivity toku** ](#activity-type-self-serve-password-reset-flow-activity-progress) -označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* [**Odemknutí uživatelský účet (samoobslužné)** ](#activity-type-unlock-user-account-self-service) -označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pomocí služby AD odemknutí účtu bez resetování funkce.
* [**Uživatel zaregistrovat pro resetování hesla pomocí samoobslužné služby** ](#activity-type-user-registered-for-self-service-password-reset) -označuje uživatele registrován všechny požadované informace, abyste mohli obnovit své heslo v souladu s zásady resetování hesel aktuálně zadaného klienta.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Typ aktivity: blokovat samoobslužné resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byl proveden pokus o resetování hesla, použijte bránu konkrétní nebo ověřit telefonní číslo více než 5 výskyty celkový za 24 hodin.
* **Aktivita objektu Actor** -uživatel, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který byl omezeny provedení další operace resetování. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla omezena provádět žádné další nastavení, pokus žádné další ověřovací metody nebo ověření žádné další telefonní čísla pro dalších 24 hodin.
* **Důvod selhání stavu aktivity** – není k dispozici

### <a name="activity-type-change-password-self-service"></a>Typ aktivity: Změna hesla (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel provést dobrovolná nebo vynutit (z důvodu vypršení platnosti) změna hesla.
* **Aktivita objektu Actor** -uživatel, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který změnit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně změnit své heslo
  * _Selhání_ -označuje uživatele se nepodařilo změnit své heslo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** - 
  * _FuzzyPolicyViolationInvalidPassword_ -uživatel vybral hesla, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-reset-password-by-admin"></a>Typ aktivity: resetování hesla (správcem)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje správce provést heslo resetovat jménem uživatele z portálu Azure.
* **Aktivita objektu Actor** -správce, který provedl resetování jménem jiného koncový uživatel nebo správce hesel. Musí být buď globální správce, heslo správce, Správce uživatelů nebo správce technické podpory.
* **Cíl aktivity** -uživatele, jehož heslo byl resetován. Může být koncovým nebo jiný správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje správce úspěšně resetovat heslo uživatele
  * _Selhání_ -označuje správce se nepodařilo změnit heslo uživatele. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-reset-password-self-service"></a>Typ aktivity: resetování hesla (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatel úspěšně obnovit své heslo z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com).
* **Aktivita objektu Actor** -uživatel, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který obnovit své heslo. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -určuje uživatel úspěšně resetovat vlastní hesla
  * _Selhání_ -udává uživatele se nepodařilo obnovit své vlastní heslo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Důvod selhání stavu aktivity** -
  * _FuzzyPolicyViolationInvalidPassword_ -správce vybrané heslo, který byl automaticky zakázán z důvodu společnosti Microsoft zakázané heslo možností detekce hledání je moc známé nebo zejména slabé.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Typ aktivity: vlastní sloužit průběh aktivity toku resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje každou konkrétní krok uživatele pokračuje prostřednictvím (například předávání konkrétní heslo resetovat bránu ověřování) jako součást heslo proces obnovení.
* **Aktivita objektu Actor** -uživatele, který provedl součástí heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatele, který provedl součástí heslo resetovat toku. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatel úspěšně dokončil konkrétní krok v procesu resetování hesla.
  * _Selhání_ -určuje konkrétní krok pro heslo resetovat toku se nezdařilo. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.
* **Povolené důvody stavu aktivity**
  * V následující tabulce najdete v části [všechny povolené aktivita resetování důvody stavu](#allowed-values-for-details-column)

### <a name="activity-type-unlock-user-account-self-service"></a>Typ aktivity: odemčení uživatelského účtu (samoobslužné)

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele byla úspěšně odemknuta účtu služby Active Directory bez resetování hesla z [portálu pro resetování hesel služby Azure AD](https://passwordreset.microsoftonline.com) pod účtem AD odemčení bez resetování funkce.
* **Aktivita objektu Actor** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který odemknuli svůj účet bez resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele byla úspěšně odemknuta vlastní účet.
  * _Selhání_ -udává uživatele se nepodařilo odemknout svůj účet. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Typ aktivity: uživatel zaregistrován pro samoobslužné resetování hesla

Následující seznam popisuje tato aktivita podrobně:

* **Popis aktivity** – označuje uživatele registrován všechny požadované informace, abyste mohli obnovit své heslo v souladu s zásady resetování hesel aktuálně zadaného klienta. 
* **Aktivita objektu Actor** -uživatel, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Cíl aktivity** -uživatel, který zaregistrovat pro resetování hesla. Může být koncový uživatel nebo správce.
* **Stavy, které jsou povolené aktivity**
  * _Úspěch_ -označuje uživatele úspěšně registrován pro v souladu s aktuální zásady resetování hesel. 
  * _Selhání_ -udává uživatele se nepodařilo zaregistrovat pro resetování hesla. Klikněte na řádek, vám umožní vidět **důvod stavu aktivity** kategorie Další informace o proč došlo k chybě. Všimněte si – to neznamená, uživatel se nemůže resetovat vlastní heslo, právě, že nebyla dokončena proces registrace. Pokud je neověřené data na jejich účtu, který je správný (například telefonní číslo, které není ověřený), i když jejich nebyly ověřeny toto telefonní číslo, můžete stále používají ho resetovat heslo. Další informace najdete v tématu [co se stane, když se uživatel zaregistruje?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers)

## <a name="next-steps"></a>Další kroky

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a která data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Příklad audit aktivity SSPR protokoly ve službě Azure AD"
