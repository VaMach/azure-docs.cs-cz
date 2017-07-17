---
title: "Sestavy aktivit auditu na portálu Azure Active Directory | Dokumentace Microsoftu"
description: "Seznámení se sestavami aktivit auditu na portálu Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d8c49272789e7d33c6f0684875765a1ecea5a2ff
ms.contentlocale: cs-cz
ms.lasthandoff: 05/26/2017

---
# Sestavy aktivit auditu na portálu Azure Active Directory
<a id="audit-activity-reports-in-the-azure-active-directory-portal" class="xliff"></a> 

Generování sestav v Azure Active Directory (Azure AD) umožňuje získat všechny informace potřebné ke zjištění stavu vašeho prostředí.

Architektura generování sestav v Azure AD se skládá z následujících komponent:

- **Aktivita** 
    - **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Riziková přihlášení** –Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

V toto tématu najdete přehled aktivit auditu.
 
## Kdo má přístup k datům?
<a id="who-can-access-the-data" class="xliff"></a>
* Uživatelé v roli Správce zabezpečení nebo Čtenář zabezpečení
* Globální správci
* Jednotliví uživatelé (bez oprávnění správce) mohou zobrazit své vlastní aktivity.


## Protokoly auditu
<a id="audit-logs" class="xliff"></a>

Protokoly auditu v Azure Active Directory obsahují záznamy aktivit systému pro zajištění dodržování předpisů.  
Prvním vstupním bodem k veškerým datům auditování je možnost **Protokoly auditu** v oddílu **Aktivita** služby **Azure Active Directory**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/61.png "Protokoly auditu")

Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- datum a čas výskytu
- iniciátor/aktér aktivity (*kdo*) 
- aktivita (*co*) 
- cíl

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/18.png "Protokoly auditu")

Zobrazení seznamu můžete upravit kliknutím na **Sloupce** na panelu nástrojů.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/19.png "Protokoly auditu")

To umožňuje zobrazit další pole, nebo odebrat pole, která jsou už zobrazená.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/21.png "Protokoly auditu")


Kliknutím na položku v zobrazení seznamu k ní zobrazíte všechny dostupné podrobnosti.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/22.png "Protokoly auditu")


## Filtrování protokolů auditu
<a id="filtering-audit-logs" class="xliff"></a>

Abyste omezili zobrazovaná data na úroveň, která vám vyhovuje, můžete filtrovat data přihlašování s využitím následujících polí:

- Rozsah dat
- Spustil(a) (činitel)
- Kategorie
- Typ prostředku aktivity
- Aktivita

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/23.png "Protokoly auditu")


Filtr pro **rozsah dat** umožňuje definovat časový rámec pro vracená data.  
Možné hodnoty:

- 1 měsíc
- 7 dní
- 24 hodin
- Vlastní

Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.

Filtr **Spustil(a)** umožňuje definovat jméno aktéra nebo hlavní název uživatele (UPN).

Filtr **Kategorie** umožňuje vybrat jeden z následujících filtrů:

- Všechny
- Základní kategorie
- Základní adresář
- Samoobslužná správa hesel
- Samoobslužná správa skupin
- Zřizování účtů – automatická změna hesel
- Pozvaní uživatelé
- Služba MIM
- Identity Protection
- B2C

Filtr **Typ prostředku aktivity** umožňuje vybrat jeden z následujících filtrů:

- Všechny 
- Skupina
- Adresář
- Uživatel
- Aplikace
- Zásada
- Zařízení
- Ostatní

Když jako **Typ prostředku aktivity** vyberete **Skupina**, zobrazí se další kategorie filtru, která umožňuje zadat také **Zdroj**:

- Azure AD
- O365


Filtr **Aktivita** je založený na vybrané kategorii a typu prostředku aktivity. Můžete vybrat konkrétní aktivitu, kterou chcete zobrazit, nebo zvolit všechny. 

Seznam všech aktivit auditu můžete získat pomocí Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, kde $tenantdomain = název domény. Také se můžete podívat na článek o [událostech sestavy auditování](active-directory-reporting-audit-events.md#list-of-audit-report-events).


## Zástupci pro protokoly auditu
<a id="audit-logs-shortcuts" class="xliff"></a>

Kromě **Azure Active Directory** poskytuje web Azure Portal dva další vstupní body k datům auditu:

- Uživatelé a skupiny
- Podnikové aplikace

### Protokoly auditu uživatelů a skupin
<a id="users-and-groups-audit-logs" class="xliff"></a>

S použitím sestav auditu orientovaných na uživatele a skupiny můžete najít odpovědi na otázky tohoto typu:

- Jaké typy aktualizací uživatelé použili?

- Kolik uživatelů bylo změněno?

- Kolik hesel bylo změněno?

- Co provedl správce v adresáři?

- Které skupiny byly přidány?

- Došlo u některých skupin ke změnám členství?

- Došlo ke změnám vlastníků skupiny?

- Jaké licence byly přiřazeny skupině nebo uživateli?

Pokud chcete jenom zkontrolovat data auditování týkající se uživatelů a skupin, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Uživatelé a skupiny**. Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Uživatelé a skupiny**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/93.png "Protokoly auditu")

### Protokoly auditu podnikových aplikací
<a id="enterprise-applications-audit-logs" class="xliff"></a>

S použitím sestav auditu orientovaných na aplikace můžete najít odpovědi na otázky tohoto typu:

* Které aplikace byly přidány nebo aktualizovány?
* Které aplikace byly odebrány?
* Změnil se instanční objekt pro aplikaci?
* Změnily se názvy aplikací?
* Kdo udělil souhlas pro aplikaci?

Pokud chcete jenom zkontrolovat data auditování týkající se aplikací, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Podnikové aplikace**. Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Podniková aplikace**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/134.png "Protokoly auditu")

Toto zobrazení je možné dál filtrovat až na samotné **skupiny** nebo **uživatele**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/25.png "Protokoly auditu")


## Další kroky
<a id="next-steps" class="xliff"></a>
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).


