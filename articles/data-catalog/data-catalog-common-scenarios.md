---
title: "Běžné scénáře služby Azure Data Catalog | Microsoft Docs"
description: "Přehled běžné scénáře pro Azure Data Catalog, včetně registrace a zjišťování zdrojů dat vysoké hodnoty, povolení samoobslužné služby business intelligence a zaznamenávání stávajících znalostí o zdrojích dat a procesy."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 156710ad50349e8a3632e31c7752387d4449a65d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="azure-data-catalog-common-scenarios"></a>Běžné scénáře služby Azure Data Catalog
Tento článek představuje běžné scénáře, ve kterých Azure Data Catalog může pomoci organizaci získat vyšší přínos z již existujících zdrojů dat.

## <a name="scenario-1-registration-of-central-data-sources"></a>Scénář 1: Registrace centrálních zdrojů dat
Více zdrojů dat znamená často jejich vyšší hodnotu pro organizaci. Jednotlivé zdroje dat přitom mohou zahrnovat obchodní údaje, údaje o online zpracování informačními systémy (OLTP), údaje z datových skladů a databází, nástroje business intelligence nebo analytické nástroje a údaje o transakcích. Počet informačních systémů a jejich vzájemný překryv se obvykle v čase podnikání aktuálně neustále mění. Organizace a jí zpracovávané údaje rostou společně s tím, jak roste samotný podnik, ale také například také tím, že dochází k fúzím a akvizicím.

Pro představitele organizace přitom může být velice složité zjistit, kde vyhledat konkrétní data v rámci jednotlivých zdrojů dat. Velice často se setkáte s otázkami typu:

* V rámci společnosti se používají tři různé HR systémy a nevím, který použít k vytvoření tohoto typu sestavy?
* Kde najdu ověřená a důvěryhodná čísla o prodejích za poslední fiskální rok, který právě skončil?
* Koho bych měl požádat nebo jaký je proces pro žádost k získání oprávnění přístupu k datovému skladu?
* Nemám jistotu, zda jsou konkrétní čísla správná. Kdo je schopen náhledu na to, jak by se mohla konkrétní data využít v rámci organizace, než je bude možné sdílet se členy týmu?

Na tyto a další otázky můžete žádat odpovědo od Azure Data Catalogu. Zdroje dat představují pro organizaci vysokou hodnotu, pokud jsou centralizované a řádně spravované oddělením IT. V organizacích jsou proto IT oddělení často logicky výchozím bodem pro sestavování katalogu údajů. I když každý uživatel si může zaregistrovat další zdroj dat, před zahájením používání katalogu pro práci se zdroji dat je důležité definovat maximální počet uživatelů, kteří budou pomáhat se správou a používáním systému. 

Pokud jsou Začínáme s Azure Data Catalog, identifikujete a zaregistrujete klíčových zdrojích dat používané mnoha různými týmy spotřebitelé dat může být vaším prvním krokem na úspěšné provedení.

Tento scénář také představuje příležitost k přidání poznámek ke zdrojům dat vysoké hodnoty, aby byly snadněji pochopit a přístup. Je také informace o tom, jak mohou uživatelé žádat o přístup ke zdroji dat jeden klíčovým prvkem toto úsilí. S Azure Data Catalog můžete zadat e-mailovou adresu uživatele nebo týmu, který je zodpovědný za řízení přístupu zdroj dat, odkazů na existující nástroje nebo dokumentaci nebo libovolný text, který popisuje proces žádosti o přístup. Tyto informace pomáhají členy, kteří zjišťovat registrované datové zdroje, ale kteří ještě nemají oprávnění k přístupu k datům pomocí procesů, které jsou definovány a řídí vlastníky zdroje dat snadno požádat o přístup.

## <a name="scenario-2-self-service-business-intelligence"></a>Scénář 2: Samoobslužné služby business intelligence
Přestože tradiční podnikové řešení business intelligence nadále být neocenitelnou pomocí součástí krajiny data řada organizací, změna se stimulací podle obchodních udělal samoobslužné služby BI více důležité. Pomocí samoobslužné služby BI pracovníků s informacemi a analytici můžete vytvořit vlastní sestavy, sešity a řídicí panely bez spoléhání na centrální IT tým nebo je omezené na základě plánu a dostupnost IT tým.

V samoobslužné scénáře BI uživatelé běžně kombinovat data z více zdrojů, řada z nich nemusí dříve byl použit pro BI a analýzy. I když některé z těchto zdrojů dat už může být známé, může být náročné Pokud chcete zjistit, co dělat, vyhledejte a vyhodnotit potenciální zdroje dat pro danou úlohu.

Tradičně, tento proces zjišťování je ruční: analytici používat jejich připojení k síti peer k ostatním, kteří pracují s daty se žádá o identifikaci. Po zdroje dat nalezen a použít, proces se opakuje opakujte pro každý další samoobslužné služby BI úsilí, s více uživateli provádění redundantní ruční proces zjišťování.

S Azure Data Catalog může vaše organizace rozdělit tento cyklus úsilí. Po zjištění zdroji dat prostřednictvím tradiční znamená, můžete zaregistrovat analytik, aby jeho snadněji zjistitelné jinými uživateli v budoucnu. I když analytika můžete přidat další hodnoty zadávání poznámek k registrované datové prostředky, není nutné provést ve stejnou dobu jako registrace tato anotace. Uživatelé mohou přispívat v čase, jako jejich povolení plány postupně přidáním hodnoty do zdroje dat, které jsou zaregistrovány v katalogu.

Tato organickým růstem katalogu obsahu je přirozené doplněk k počáteční registrace centrálních zdrojů dat. Předem vyplní katalogu s daty, která bude nutné mnoho uživatelů může být motivator pro počáteční použití a zjišťování. Povolení uživatelům zaregistrujte a opatřete poznámkami další zdroje může být způsob, jak udržovat je i ostatním členům organizace pověření.

Je vhodné poznamenat, že i když tento scénář se zaměřuje konkrétně na BI samoobslužné služby, stejné vzory a problémy platí pro rozsáhlé podnikové BI projekty také. Pomocí katalogu Data Catalog vaší organizace může zvýšit žádné úsilí, který zahrnuje ruční proces zdroj dat zjišťování.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Scénář 3: Zaznamenání kmenové znalosti
Jak poznáte, jaká data je nutné provést úlohu a kde najít data?

Pokud jste si v úloze nějakou dobu, je pravděpodobně právě znát. Prošli postupné učení procesu a v čase se naučili o zdrojích dat, které jsou klíčem k každodenní práce.

V případě nového zaměstnance připojen váš tým, jak tato osoba vědět, jaká data je vyžadován pro úlohy a kde je najít?

Pravděpodobné se nové osobě obsahuje vám tyto otázky.

Tento probíhající přenos kmenové znalosti je součástí procesu zjišťování zdrojů dat v organizacích malé i velké. Mít senior a zkušenosti členové týmu průběhu let vytvořená znalostní báze a novější členové týmu se naučili a požádat ho, když mají dotazy. Nejvíce podstatné informace často existuje pouze v vedoucích několik klíčových osob a když tyto osoby na dovolenou nebo nechte týmem, vyskytne organizace.

Data odborníky normálně se snažit dokumentu své znalosti její sdílení e-mailem nebo do dokumentů aplikace Word na týmovém webu služby SharePoint. I když může být tento přístup, zavádí nové zjišťování problému: jak uživatelé zjistit, co dokumentace existuje a kde je najít?

S Azure Data Catalog má vaše organizace jednu centrální umístění pro ukládání a sdílení této kmenové znalosti a pro provedení snadno zjistitelné. V katalogu Data Catalog může vaše data odborníky opatřit poznámkami datové prostředky a zadejte odkazy na stávající dokumentaci. Pokud organizace členy pomocí katalogu můžete zjistit zdroj dat, budete najdou nejen zdroji sám sebe, ale také znalostní báze, který dříve existoval pouze s danou odborníky vaší organizace.
