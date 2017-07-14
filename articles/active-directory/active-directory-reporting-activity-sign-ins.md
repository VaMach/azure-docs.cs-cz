---
title: "Sestavy aktivit přihlašování na portálu Azure Active Directory | Dokumentace Microsoftu"
description: "Seznámení se sestavami aktivit přihlašování na portálu Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: d9cf94252252094218036182c6dc08e8d0779fbe
ms.contentlocale: cs-cz
ms.lasthandoff: 07/06/2017


---
# Sestavy aktivit přihlašování na portálu Azure Active Directory
<a id="sign-in-activity-reports-in-the-azure-active-directory-portal" class="xliff"></a>

Generování sestav Azure Active Directory (Azure AD) na webu [Azure Portal](https://portal.azure.com) umožňuje získat všechny informace potřebné ke zjištění stavu vašeho prostředí.

Architektuře generování sestav v Azure Active Directory se skládá z těchto součástí:

- **Aktivita** 
    - **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Riziková přihlášení** –Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

V toto tématu najdete přehled aktivit přihlašování.

## Předpoklad
<a id="pre-requisite" class="xliff"></a>

### Kdo má přístup k datům?
<a id="who-can-access-the-data" class="xliff"></a>
* Uživatelé v roli Správce zabezpečení nebo Čtenář zabezpečení
* Globální správci
* Každý uživatel (bez oprávnění správce) může přistupovat k vlastnímu přihlašování. 

### Jaká licence Azure AD je potřeba pro přístup k přihlašovací aktivitě?
<a id="what-azure-ad-license-do-you-need-to-access-sign-in-activity" class="xliff"></a>
* Klient musí mít přiřazenou licenci Azure AD Premium, aby mohl zobrazit sestavu veškerých aktivit přihlašování.


## Aktivity přihlašování
<a id="signs-in-activities" class="xliff"></a>

Na základě informací poskytnutých sestavou přihlašování uživatelů najdete odpovědi na otázky tohoto typu:

* Jaký je vzorec přihlašování uživatele?
* Kolik uživatelů se přihlásilo za týden?
* Jaký je stav těchto přihlášení?

Váš první vstupní bod pro všechna data aktivit přihlašování je **Přihlášení** v části Aktivity služby **Azure Active**


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/61.png "Aktivita přihlašování")


Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- související uživatel
- aplikace, ke které se uživatel přihlásil
- stav přihlášení
- čas přihlášení

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")

Zobrazení seznamu můžete upravit kliknutím na **Sloupce** na panelu nástrojů.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/19.png "Aktivita přihlašování")

To umožňuje zobrazit další pole, nebo odebrat pole, která jsou už zobrazená.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/42.png "Aktivita přihlašování")

Kliknutím na položku v zobrazení seznamu k ní zobrazíte všechny dostupné podrobnosti.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/43.png "Aktivita přihlašování")


## Filtrování aktivit přihlašování
<a id="filtering-sign-in-activities" class="xliff"></a>

Abyste omezili zobrazovaná data na úroveň, která vám vyhovuje, můžete filtrovat data přihlašování s využitím následujících polí:

- Časový interval
- Uživatel
- Aplikace
- Klient
- Stav přihlášení

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/44.png "Aktivita přihlašování")


Filtr pro **časový interval** umožňuje zadat časový rámec pro vracená data.  
Možné hodnoty:

- 1 měsíc
- 7 dní
- 24 hodin
- Vlastní

Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.

Filtr pro **uživatele** umožňuje určit jméno nebo hlavní název uživatele (UPN) pro uživatele, o kterého vám jde.

Filtr pro **aplikaci** umožňuje určit název aplikace, o kterou vám jde.

Filtr pro **klienta** umožňuje určit informace o zařízení, o které vám jde.

Filtr pro **stav přihlášení** umožňuje vybrat jeden z následujících filtrů:

- Všechny
- Úspěch
- Selhání


## Zkratky pro aktivity přihlašování
<a id="sign-in-activities-shortcuts" class="xliff"></a>

Kromě Azure Active Directory poskytuje web Azure Portal dva další vstupní body k datům aktivit přihlašování:

- Uživatelé a skupiny
- Podnikové aplikace


### Aktivity přihlašování uživatelů a skupin
<a id="users-and-groups-sign-ins-activities" class="xliff"></a>

Na základě informací poskytnutých sestavou přihlašování uživatelů najdete odpovědi na otázky tohoto typu:

- Jaký je vzorec přihlašování uživatele?
- Kolik uživatelů se přihlásilo za týden?
- Jaký je stav těchto přihlášení?



Vaším vstupním bodem pro tato data je graf přihlašování uživatelů v oddílu **Přehled** v části **Uživatelé a skupiny**.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/45.png "Aktivita přihlašování")

V grafu přihlašování uživatelů jsou znázorněny týdenní agregace přihlášení všech uživatelů za dané časové období. Výchozí časové období je 30 dnů.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/46.png "Aktivita přihlašování")

Když v grafu přihlašování kliknete na konkrétní den, zobrazí se pro tento den podrobný seznam aktivit přihlašování.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")

Každý řádek v seznamu aktivit přihlašování obsahuje podrobné informace o vybraném přihlášení. Příklad:

* Kdo se přihlásil?
* Jaký byl související hlavní název uživatele (UPN)?
* Která aplikace byla cílem přihlášení?
* Jaká je IP adresa přihlášení?
* Jaký byl stav přihlášení?

Možnost **Přihlášení** poskytuje úplný přehled o všech přihlášeních uživatelů.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/51.png "Aktivita přihlašování")



## Použití spravovaných aplikací
<a id="usage-of-managed-applications" class="xliff"></a>

S použitím zobrazení dat přihlašování zaměřeného na aplikace můžete odpovídat na otázky tohoto typu:

* Kdo používá mé aplikace?
* Které jsou 3 nejpoužívanější aplikace v organizaci?
* Nedávno jsem zpřístupnil aplikaci. Jak to s ní vypadá?

Vaším vstupním bodem k těmto datům jsou 3 nejpoužívanější aplikace v organizaci v rámci sestavy za posledních 30 dnů v oddílu **Přehled** v části **Podnikové aplikace**.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/64.png "Aktivita přihlašování")

Graf využívání aplikací s týdenními agregacemi přihlašování pro 3 nejpoužívanější aplikace v daném časovém období. Výchozí časové období je 30 dnů.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/47.png "Aktivita přihlašování")

Pokud chcete, můžete se zaměřit na konkrétní aplikaci.


![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Vytváření sestav")

Když v grafu využívání aplikací kliknete na konkrétní den, zobrazí se podrobný seznam aktivit přihlašování.


![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/48.png "Aktivita přihlašování")


Možnost **Přihlášení** poskytuje úplný přehled o všech událostech přihlašování pro vaše aplikace.

![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/49.png "Aktivita přihlašování")



## Další kroky
<a id="next-steps" class="xliff"></a>
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).


