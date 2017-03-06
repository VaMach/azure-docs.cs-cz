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
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 74460492c5eb6edfcc67015f8f6894267cb9edc8
ms.openlocfilehash: 5a219219cd5e34713cd6a1c54a98d6bd15310e05
ms.lasthandoff: 02/22/2017


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

Na webu Azure Portal jsou k dispozici tři hlavní kategorie aktivit souvisejících s auditováním:

- Uživatelé a skupiny   

- Aplikace

- Adresář   

Úplný seznam aktivit sestavy auditování najdete v [seznamu událostí sestavy auditu](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Vaším vstupním bodem k veškerým datům auditování je možnost **Protokoly auditu** v oddílu **Aktivita** v **Azure Active Directory**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/61.png "Protokoly auditu")

Protokol auditu nabízí zobrazení seznamu s účastníky (*kdo*), aktivitami (*co*) a cíli.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/345.png "Protokoly auditu")

Kliknutím na položku v zobrazení seznamu zobrazíte další podrobnosti.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/873.png "Protokoly auditu")


## <a name="users-and-groups-audit-logs"></a>Protokoly auditu uživatelů a skupin

S použitím sestav auditu orientovaných na uživatele a skupiny můžete najít odpovědi na otázky tohoto typu:

- Jaké typy aktualizací uživatelé použili?

- Kolik uživatelů bylo změněno?

- Kolik hesel bylo změněno?

- Co provedl správce v adresáři?

- Které skupiny byly přidány?

- Došlo u některých skupin ke změnám členství?

- Došlo ke změnám vlastníků skupiny?

- Jaké licence byly přiřazeny skupině nebo uživateli?

Pokud chcete jen zkontrolovat data auditování týkající se uživatelů a skupin, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Uživatelé a skupiny**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/93.png "Protokoly auditu")

## <a name="application-audit-logs"></a>Protokoly auditu aplikací
S použitím sestav auditu orientovaných na aplikace můžete najít odpovědi na otázky tohoto typu:

* Které aplikace byly přidány nebo aktualizovány?
* Které aplikace byly odebrány?
* Změnil se instanční objekt pro aplikaci?
* Změnily se názvy aplikací?
* Kdo udělil souhlas pro aplikaci?

Pokud chcete jen zkontrolovat data auditování týkající se aplikací, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Podnikové aplikace**.

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/134.png "Protokoly auditu")

## <a name="filtering-audit-logs"></a>Filtrování protokolů auditu
Přihlášení můžete filtrovat pomocí následujících polí a omezit tak množství zobrazených dat:

- Datum a čas

- Název objektu zabezpečení Actor

- Kategorie

- Typ prostředku aktivity

- Aktivita

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/625.png "Protokoly auditu")


Filtr **Kategorie** umožňuje zúžit rozsah sestavy auditu na základě těchto kategorií:

- Základní adresář

- Samoobslužná správa hesel

- Samoobslužná správa skupin

- Automatická změna hesel 

![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/626.png "Protokoly auditu")



Obsah seznamu **Typ prostředku aktivity** závisí na tom, kudy do tohoto okna vstoupíte.  
Pokud je vstupním bodem služba Azure Active Directory, bude tento seznam obsahovat všechny možné typy aktivit:

- Adresář

- Uživatel

- Skupina 

- Aplikace 

- Zásada

- Zařízení


![Auditování](./media/active-directory-reporting-activity-audit-logs/627.png "Auditování")

Obor aktivit uvedených v seznamu je nastaven podle typu aktivity.
Pokud jste například jako **Typ aktivity** vybrali **Uživatel**, bude seznam **Aktivita** obsahovat pouze aktivit související se skupinami.   

![Auditování](./media/active-directory-reporting-activity-audit-logs/628.png "Auditování")

Pokud jako **Typ aktivity** vyberete **Skupina**, získáte další možnost filtru, která vám umožňuje filtrovat také na základě následujících **zdrojů aktivit**:

- Azure AD

- O365


![Auditování](./media/active-directory-reporting-activity-audit-logs/629.png "Auditování")



Jinou metodou filtrování záznamů protokolu auditu je vyhledání konkrétních položek.

![Auditování](./media/active-directory-reporting-activity-audit-logs/237.png "Auditování")




## <a name="next-steps"></a>Další kroky
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).


