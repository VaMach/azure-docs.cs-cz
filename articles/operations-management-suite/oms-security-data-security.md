---
title: "Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite | Dokumentace Microsoftu"
description: "Tento dokument popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit pro Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d81d3ff9c91d0056c69f5f190d3dfaa507c1e340


---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Zabezpečení dat v řešení Zabezpečení a audit pro Operations Management Suite
[Řešení Zabezpečení a audit pro Operations Management Suite (OMS)](operations-management-suite-overview.md) pomáhá zákazníkům předcházet hrozbám, rozpoznat je a reagovat na ně tím, že shromažďuje a zpracovává data o prostředcích, k nimž patří:

* Protokol událostí zabezpečení
* Události Trasování událostí pro Windows
* Události auditování AppLocker
* Protokol brány Windows Firewall
* Události Rozšířené analýzy hrozeb
* Výsledky základního vyhodnocování
* Výsledky antimalwarového vyhodnocování
* Výsledky vyhodnocování aktualizací a oprav
* Datové proudy syslog explicitně povolené v agentu

Zavázali jsme se, že soukromí a bezpečnost těchto dat budeme chránit. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb.
Tento článek popisuje způsob správy a ochrany dat v řešení Zabezpečení a audit v OMS.

## <a name="data-sources"></a>Zdroje dat
Řešení Zabezpečení a audit v OMS analyzuje data z virtuálních a fyzických počítačů s nainstalovaným agentem OMS. Řešení Zabezpečení a audit v OMS může shromažďovat informace o konfiguraci událostí zabezpečení, jako jsou například protokoly událostí a auditu systému Windows, protokoly služby IIS a zprávy syslog. Mezi příklady těchto dat patří: typ a verze operačního systému, spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID tenanta.  

## <a name="data-protection"></a>Ochrana dat
**Oddělení dat**: Data se v rámci služby ukládají logicky oddělená pro jednotlivé komponenty. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu celého životního cyklu dat a je vyžadováno na každé úrovni služby. 

**Přístup k datům**: Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají pracovníci společnosti Microsoft přístup k informacím shromažďovaným nebo analyzovaným službami. Dodržujeme [Podmínky online služeb společnosti Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), ve kterých je uvedeno, že společnost Microsoft nebude informace o zákaznících používat ani z nich odvozovat další informace pro reklamní nebo podobné obchodní účely. Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají pracovníci společnosti Microsoft přístup k informacím shromažďovaným nebo analyzovaným službami. Informace o zákaznících podle potřeby používáme pouze k poskytování služeb Azure a k účelům slučitelným s poskytováním těchto služeb. Všechna práva na vaše data zůstávají ve vašem vlastnictví.

**Použití dat**: Společnost Microsoft vylepšuje své schopnosti prevence a detekce pomocí schémat a analýzy hrozeb napříč několika klienty. Činíme tak v souladu se závazky k ochraně osobních údajů popsanými v našem [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Umístění dat je konfigurováno na úrovni pracovního prostoru OMS během jeho vytváření, což je součástí procesu prvotní konfigurace řešení Zabezpečení a audit pro OMS.
> 
> 

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli, jakým způsobem probíhá správa a ochrana dat v OMS. Další informace o řešení Zabezpečení a audit v OMS najdete v tématech:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Nov16_HO2-->


