---
title: "Sestavy aktivit auditu na portálu Azure Active Directory ve verzi Preview | Dokumentace Microsoftu"
description: "Seznámení se sestavami aktivit auditu na portálu Azure Active Directory ve verzi Preview"
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
ms.date: 04/07/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: d88f8bed0cbd14ee49986d6749396731a810034b
ms.lasthandoff: 04/18/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Sestavy aktivit auditu na portálu Azure Active Directory ve verzi Preview

Díky možnosti vytváření sestav v Azure Active Directory ve verzi [Preview](active-directory-preview-explainer.md) získáte všechny informace, které potřebujete ke zjištění stavu vašeho prostředí.

Architektuře generování sestav v Azure Active Directory se skládá z těchto součástí:

- **Aktivita** 
    - **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
    - **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů
- **Zabezpečení** 
    - **Riziková přihlášení** –Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. Další podrobnosti najdete v tématu Riziková přihlášení.
    - **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.

V toto tématu najdete přehled aktivit auditu.
 


## <a name="audit-logs"></a>Protokoly auditu

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


## <a name="filtering-audit-logs"></a>Filtrování protokolů auditu

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
- Zřizování účtů
- Automatická změna hesel
- pozvaní uživatelé
- Služba MIM

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

| Kategorie aktivity| Typ prostředku aktivity| Aktivita |
| :-- | :-: | :-- |
| Základní adresář| Skupina| Odstranění nastavení skupin|
| Základní adresář| Adresář| Aktualizační doména|
| Základní adresář| Adresář| Odebrání partnera ze společnosti|
| Základní adresář| Uživatel| Aktualizace role|
| Základní adresář| Uživatel| Přidání role ze šablony|
| Základní adresář| Skupina| Přidání přiřazení role aplikace do skupiny|
| Základní adresář| Skupina| Zahájení použití skupinové licence pro uživatele|
| Základní adresář| Aplikace| Přidání instančního objektu|
| Základní adresář| Zásada| Aktualizace zásad|
| Základní adresář| Zásada| Přidání zásad k instančnímu objektu|
| Základní adresář| Zařízení| Přidání registrovaného vlastníka zařízení|
| Základní adresář| Zařízení| Přidání registrovaných uživatelů zařízení|
| Základní adresář| Zařízení| Aktualizace konfigurace zařízení|
| Samoobslužná správa hesel| Uživatel| Resetování hesla (samoobslužné)|
| Samoobslužná správa hesel| Uživatel| Odemknutí uživatelského účtu (samoobslužné)|
| Samoobslužná správa hesel| Uživatel| Resetování hesla (správcem)|
| Samoobslužná správa skupin| Skupina| Odstranění čekající žádosti o připojení ke skupině|
| Zřizování účtů| Aplikace| Podmíněné zpracování|
| Automatická změna hesel| Aplikace| Automatická změna hesel|
| Pozvaní uživatelé| Ostatní| Zpracované hromadné pozvánky|
| Základní adresář| Adresář| Odebrání ověřené domény|
| Základní adresář| Adresář| Přidání neověřené domény|
| Základní adresář| Adresář| Přidání ověřené domény|
| Základní adresář| Adresář| Nastavení funkce adresáře v tenantovi|
| Základní adresář| Adresář| Nastavení příznaku Dirsyncenabled|
| Základní adresář| Adresář| Vytvoření nastavení společnosti|
| Základní adresář| Adresář| Aktualizace nastavení společnosti|
| Základní adresář| Adresář| Odstranění nastavení společnosti|
| Základní adresář| Adresář| Nastavení povoleného umístění dat společnosti|
| Základní adresář| Adresář| Povolení vícejazykové funkce společnosti|
| Základní adresář| Uživatel| Aktualizace uživatele|
| Základní adresář| Uživatel| Odstranění uživatele|
| Základní adresář| Skupina| Odebrání člena ze skupiny|
| Základní adresář| Skupina| Nastavení skupinové licence|
| Základní adresář| Skupina| Vytvoření nastavení skupin|
| Základní adresář| Aplikace| Aktualizace instančního objektu|
| Základní adresář| Aplikace| Odstranění aplikace|
| Základní adresář| Aplikace| Aktualizace aplikace|
| Základní adresář| Aplikace| Odebrání instančního objektu|
| Základní adresář| Aplikace| Přidání přihlašovacích údajů instančního objektu|
| Základní adresář| Aplikace| Odebrání přiřazení role aplikace z instančního objektu|
| Základní adresář| Aplikace| Odebrání vlastníka z aplikace|
| Základní adresář| Zařízení| Odebrání registrovaného vlastníka ze zařízení|
| Samoobslužná správa hesel| Uživatel| Průběh aktivity toku samoobslužného resetování hesel|
| Zřizování účtů| Aplikace| Správa|
| Zřizování účtů| Aplikace| Operace adresáře|
| Služba MIM| Skupina| Odebrání člena|
| Základní adresář| Zásada| Odstranění zásad|
| Pozvaní uživatelé| Uživatel| Vytvoření virálního tenanta|
| Základní adresář| Adresář| Aktualizace externích tajných klíčů|
| Základní adresář| Adresář| Nastavení vlastností Rights Management|
| Základní adresář| Adresář| Aktualizace společnosti|
| Základní adresář| Uživatel| Přidání uživatele|
| Základní adresář| Uživatel| Převod federovaného uživatele na spravovaného|
| Základní adresář| Uživatel| Vytvoření hesla aplikace pro uživatele|
| Základní adresář| Skupina| Přidání člena do skupiny|
| Základní adresář| Skupina| Přidání skupiny|
| Základní adresář| Aplikace| Souhlas s aplikací|
| Základní adresář| Aplikace| Přidání aplikace|
| Základní adresář| Aplikace| Přidání vlastníka k instančnímu objektu|
| Základní adresář| Aplikace| Odebrání Oauth2Permissiongrant|
| Základní adresář| Zásada| Odebrání přihlašovacích údajů pro zásady|
| Základní adresář| Zařízení| Odstranění konfigurace zařízení|
| Samoobslužná správa skupin| Skupina| Nastavení vlastností dynamických skupin|
| Samoobslužná správa skupin| Skupina| Aktualizace zásad správy životního cyklu|
| Zřizování účtů| Aplikace| Akce synchronizace pravidel|
| Pozvaní uživatelé| Ostatní| Odeslané hromadné pozvánky|
| Služba MIM| Skupina| Přidání člena|
| Základní adresář| Uživatel| Nastavení vlastností licencí|
| Základní adresář| Uživatel| Obnovení uživatele|
| Základní adresář| Uživatel| Odebrání člena z role|
| Základní adresář| Uživatel| Odebrání uživateli přiřazení role aplikace|
| Základní adresář| Uživatel| Odebrání vymezeného člena z role|
| Základní adresář| Skupina| Aktualizace skupiny|
| Základní adresář| Skupina| Přidání vlastníka ke skupině|
| Základní adresář| Skupina| Ukončení použití skupinové licence pro uživatele|
| Základní adresář| Skupina| Odebrání přiřazení role aplikace ze skupiny|
| Základní adresář| Skupina| Nastavení správy skupiny uživatelem|
| Základní adresář| Aplikace| Přidání Oauth2Permissiongrant|
| Základní adresář| Aplikace| Přidání přiřazení role aplikace k instančnímu objektu|
| Základní adresář| Aplikace| Odebrání přihlašovacích údajů instančního objektu|
| Základní adresář| Zásada| Odebrání zásad z instančního objektu|
| Základní adresář| Zařízení| Aktualizace zařízení|
| Základní adresář| Zařízení| Přidání zařízení|
| Základní adresář| Zařízení| Přidání konfigurace zařízení|
| Samoobslužná správa hesel| Uživatel| Změna hesla (samoobslužná)|
| Samoobslužná správa hesel| Uživatel| Registrace uživatele pro samoobslužné resetování hesla|
| Samoobslužná správa skupin| Skupina| Schválení čekající žádosti o připojení ke skupině|
| Základní adresář| Adresář| Odebrání neověřené domény|
| Základní adresář| Adresář| Ověření domény|
| Základní adresář| Adresář| Nastavení doménového ověřování|
| Základní adresář| Adresář| Nastavení zásad pro hesla|
| Základní adresář| Adresář| Přidání partnera ke společnosti|
| Základní adresář| Adresář| Propagace společnosti u partnera|
| Základní adresář| Adresář| Nastavení partnerství|
| Základní adresář| Adresář| Nastavení prahové hodnoty náhodného odstranění|
| Základní adresář| Adresář| Snížení úrovně partnera|
| Pozvaní uživatelé| Uživatel| Pozvání externího uživatele|
| Zřizování účtů| Aplikace| Import|
| Základní adresář| Aplikace| Odebrání vlastníka z instančního objektu|
| Základní adresář| Zařízení| Odebrání registrovaných uživatelů ze zařízení|
| Základní adresář| Adresář| Nastavení informací o společnosti|
| Základní adresář| Adresář| Nastavení federování v doméně|
| Základní adresář| Adresář| Vytvoření společnosti|
| Základní adresář| Adresář| Vyprázdnění vlastností Rights Management|
| Základní adresář| Adresář| Nastavení funkce Dirsync|
| Základní adresář| Adresář| Ověření domény s e-mailovým ověřením|
| Základní adresář| Uživatel| Změna uživatelské licence|
| Základní adresář| Uživatel| Změna hesla uživatele|
| Základní adresář| Uživatel| Resetování hesla uživatele|
| Základní adresář| Uživatel| Přidání přiřazení role aplikace k uživateli|
| Základní adresář| Uživatel| Přidání člena do role|
| Základní adresář| Uživatel| Odstranění hesla aplikace pro uživatele|
| Základní adresář| Uživatel| Aktualizace přihlašovacích údajů uživatele|
| Základní adresář| Uživatel| Nastavení správce uživatelů|
| Základní adresář| Uživatel| Přidání vymezeného člena do role|
| Základní adresář| Skupina| Odstranění skupiny|
| Základní adresář| Skupina| Odebrání vlastníka ze skupiny|
| Základní adresář| Skupina| Aktualizace nastavení skupin|
| Základní adresář| Aplikace| Přidání vlastníka do aplikace|
| Základní adresář| Aplikace| Odvolání souhlasu|
| Základní adresář| Zásada| Přidání zásad|
| Základní adresář| Zařízení| Odstranění zařízení|
| Samoobslužná správa hesel| Uživatel| Blokování samoobslužného resetování hesla|
| Samoobslužná správa skupin| Skupina| Žádost o připojení ke skupině|
| Samoobslužná správa skupin| Skupina| Vytvoření zásad správy životního cyklu|
| Samoobslužná správa skupin| Skupina| Odmítnutí čekající žádosti o připojení ke skupině|
| Samoobslužná správa skupin| Skupina| Zrušení čekající žádosti o připojení ke skupině|
| Samoobslužná správa skupin| Skupina| Obnovení skupiny|
| Zřizování účtů| Aplikace| Export|
| Zřizování účtů| Aplikace| Ostatní|
| Pozvaní uživatelé| Uživatel| Uplatnění pozvání externího uživatele|
| Pozvaní uživatelé| Uživatel| Vytvoření virálního uživatele|
| Pozvaní uživatelé| Uživatel| Přiřazení externího uživatele k aplikaci|




## <a name="audit-logs-shortcuts"></a>Zástupci pro protokoly auditu

Kromě **Azure Active Directory** poskytuje web Azure Portal dva další vstupní body k datům auditu:

- Uživatelé a skupiny
- Podnikové aplikace

Úplný seznam aktivit sestavy auditování najdete v [seznamu událostí sestavy auditu](active-directory-reporting-audit-events.md#list-of-audit-report-events).


### <a name="users-and-groups-audit-logs"></a>Protokoly auditu uživatelů a skupin

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

### <a name="enterprise-applications-audit-logs"></a>Protokoly auditu podnikových aplikací

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


## <a name="next-steps"></a>Další kroky
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).


