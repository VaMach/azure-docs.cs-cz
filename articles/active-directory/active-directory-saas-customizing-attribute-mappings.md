---
title: "Přizpůsobení mapování atributů Azure AD | Microsoft Docs"
description: "Zjistěte, co mapování atributů pro aplikace SaaS ve službě Azure Active Directory se, jak je vyřešit obchodních potřeb můžete upravit."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d4618f7cd702a1cfefafe80eff35a0753e04a50c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Přizpůsobení mapování atributů zřizování pro aplikace SaaS ve službě Azure Active Directory uživatelů
Microsoft Azure AD poskytuje podporu pro zřizování uživatelů pro aplikace SaaS jiných výrobců jako Salesforce, Google Apps a dalších. Pokud máte uživatele zřizování pro aplikace SaaS třetích stran povoleno, portálu pro správu Azure prvky jeho hodnot atributů v podobě konfigurace s názvem "mapování atributů."

Není předkonfigurované sadu atributů mapování mezi objekty uživatele Azure AD a každá aplikace SaaS uživatelské objekty. Některé aplikace spravovat jiné typy objektů, jako jsou skupiny nebo kontakty. <br> 
 Mapování atributů výchozí můžete přizpůsobit podle obchodních potřeb. To znamená, můžete změnit nebo odstranit existující mapování atributů nebo vytvořte nové mapování atributů.

Na portálu Azure AD, dostanete tuto funkci kliknutím **mapování** konfigurace v **zřizování** v **spravovat** části  **Podniková aplikace**.


![Salesforce][5] 

Kliknutím **mapování** otevření související konfigurace, **mapování atributů** okno.  
Existují mapování atributů vyžadované aplikací SaaS fungovat správně. Pro povinné atributy **odstranit** funkce není dostupná.


![Salesforce][6]  

V předchozím příkladu můžete uvidíte, že **uživatelské jméno** se zobrazí v atributu spravovaného objektu v Salesforce **userPrincipalName** hodnota propojené objekt služby Azure Active Directory.

Můžete upravit existující **mapování atributů** kliknutím mapování. Tím se otevře **Upravit atribut** okno.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Principy typů mapování atributů
S mapování atributů řídit, jak jsou naplněny atributy v aplikaci SaaS jiných výrobců. Existují čtyři typy různých mapování podporované:

* **Přímé** – atribut target naplněný hodnotu atributu propojeného objektu ve službě Azure AD.
* **Konstantní** – cílový atribut je naplněna konkrétní řetězec, který jste zadali.
* **Výraz** -cílový atribut je vyplněný, na základě výsledku skriptu jako výraz. 
  Další informace najdete v tématu [zápis výrazy pro mapování atributů v Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Žádný** -cílový atribut je ponechán beze změny. Ale pokud cílový atribut je někdy prázdná, je naplněna s výchozí hodnotou, který určíte.

Kromě těchto čtyř typů mapování základní atribut mapování vlastních atributů podporují koncept volitelný **výchozí** přiřazení hodnoty. Přiřazení hodnoty výchozí zajistí, že atribut target je vyplněný s hodnotou, když není ani jedna hodnota ve službě Azure AD ani v cílovém objektu. Nejběžnější konfigurace je nechte pole prázdné.


## <a name="understanding-attribute-mapping-properties"></a>Principy vlastnosti mapování atributů

V předchozí části je již byly zavedeny na vlastnost typu mapování atributů.
Kromě tato vlastnost mapování atributů také podporují následující atributy:

- **Zdrojový atribut** -atribut uživatele ze zdrojového systému (například: Azure Active Directory).
- **Atribut target** – atribut uživatele v cílovém systému (například: ServiceNow).
- **Objekty používající tento atribut odpovídat** – jestli toto mapování se má použít k jednoznačné identifikaci uživatele mezi zdrojovým a cílovým systémem. To se obvykle nastavuje na atribut userPrincipalName nebo e-mailu ve službě Azure AD, která se obvykle mapuje na pole uživatelské jméno v cílové aplikace.
- **Odpovídající prioritu** – více odpovídající atributy lze nastavit. Pokud existuje více vyhodnocují se v pořadí podle tohoto pole. Jakmile je nalezena shoda, žádné další odpovídající atributy vyhodnocují.
- **Použít toto mapování**
    - **Vždy** – použít toto mapování na obou vytvoření uživatele a aktualizovat akce
    - **Pouze během vytváření** -použít toto mapování pouze na akcí vytvoření uživatele


## <a name="what-you-should-know"></a>Důležité informace

Microsoft Azure AD poskytuje efektivní implementaci procesu synchronizace. V prostředí inicializovaného jsou zpracovány pouze objekty, které vyžadují aktualizace při synchronizační cyklus. Aktualizace mapování atributů má vliv na výkon synchronizační cyklus. Aktualizace mapování atributů vyžaduje všechny spravované objekty do znovu vyhodnocena. Je součástí osvědčeného postupu udrželi počet po sobě jdoucích změn na vaše mapování atributů minimálně.

## <a name="next-steps"></a>Další kroky

* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizovat uživatele zřízení nebo zrušení zřízení k aplikacím SaaS](active-directory-saas-app-provisioning.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

