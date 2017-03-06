---
title: "Sestavy aktivit přihlašování na portálu Azure Active Directory ve verzi Preview | Dokumentace Microsoftu"
description: "Seznámení se sestavami aktivit přihlašování na portálu Azure Active Directory ve verzi Preview"
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
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 78617dc7e3a4b6eb4fc32d32b6850b8c0832d6d8
ms.openlocfilehash: 9819c5f6a3aea53664d86e3a23b25946c0f2b731
ms.lasthandoff: 02/22/2017


---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal---preview"></a>Sestavy aktivit přihlašování na portálu Azure Active Directory ve verzi Preview

Díky možnosti vytváření sestav v Azure Active Directory ve verzi [Preview](active-directory-preview-explainer.md) získáte všechny informace, které potřebujete ke zjištění stavu vašeho prostředí.

Architektuře generování sestav v Azure Active Directory se skládá z těchto součástí:

- **Aktivita** 
    - **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Riziková přihlášení** –Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

V toto tématu najdete přehled aktivit přihlašování.

## <a name="signs-in-activities"></a>Aktivity přihlašování

Na základě informací poskytnutých sestavou přihlašování uživatelů najdete odpovědi na otázky tohoto typu:

* Jaký je vzorec přihlašování uživatele?
* Kolik uživatelů se přihlásilo za týden?
* Jaký je stav těchto přihlášení?

Vaším vstupním bodem pro tato data je graf přihlašování uživatelů v oddílu **Přehled** v části **Uživatelé a skupiny**.

 ![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/05.png "Vytváření sestav")

V grafu přihlašování uživatelů jsou znázorněny týdenní agregace přihlášení všech uživatelů za dané časové období. Výchozí časové období je 30 dnů.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/02.png "Vytváření sestav")

Když v grafu přihlašování kliknete na konkrétní den, zobrazí se podrobný seznam aktivit přihlašování.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/03.png "Vytváření sestav")

Každý řádek v seznamu aktivit přihlašování obsahuje podrobné informace o vybraném přihlášení. Příklad:

* Kdo se přihlásil?
* Jaký byl související hlavní název uživatele (UPN)?
* Která aplikace byla cílem přihlášení?
* Jaká je IP adresa přihlášení?
* Jaký byl stav přihlášení?

## <a name="usage-of-managed-applications"></a>Použití spravovaných aplikací

S použitím zobrazení dat přihlašování zaměřeného na aplikace můžete odpovídat na otázky tohoto typu:

* Kdo používá mé aplikace?
* Které jsou 3 nejpoužívanější aplikace v organizaci?
* Nedávno jsem zpřístupnil aplikaci. Jak to s ní vypadá?

Vaším vstupním bodem k těmto datům jsou 3 nejpoužívanější aplikace v organizaci v rámci sestavy za posledních 30 dnů v oddílu **Přehled** v části **Podnikové aplikace**.

 ![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/06.png "Vytváření sestav")

Graf využívání aplikací s týdenními agregacemi přihlašování pro 3 nejpoužívanější aplikace v daném časovém období. Výchozí časové období je 30 dnů.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/78.png "Vytváření sestav")

Pokud chcete, můžete se zaměřit na konkrétní aplikaci.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Vytváření sestav")

Když v grafu využívání aplikací kliknete na konkrétní den, zobrazí se podrobný seznam aktivit přihlašování.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/top_app_sign_ins.png "Vytváření sestav")

Možnost **Přihlášení** poskytuje úplný přehled o všech událostech přihlašování pro vaše aplikace.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/85.png "Vytváření sestav")

Pomocí voliče sloupců můžete vybrat datová pole, která chcete zobrazit.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/column_chooser.png "Vytváření sestav")

## <a name="filtering-sign-ins"></a>Filtrování přihlášení
Přihlášení můžete filtrovat pomocí následujících polí a omezit tak množství zobrazených dat:

* Datum a čas 
* Název objektu zabezpečení User
* Název aplikace
* Název klienta
* Stav přihlášení

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/293.png "Vytváření sestav")

Jinou metodou filtrování záznamů aktivit přihlašování je vyhledání konkrétních položek.
Metoda hledání umožňuje omezit obor přihlášení na konkrétní **uživatele**, **skupiny** nebo **aplikace**.

![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/84.png "Vytváření sestav")


## <a name="next-steps"></a>Další kroky
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).


