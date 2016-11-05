---
title: Správa partnerských řešení v Azure Security Center | Microsoft Docs
description: V tomto dokumentu se dozvíte, jak vám Azure Security Center umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/22/2016
ms.author: terrylan

---
# Sledování partnerských řešení pomocí Azure Security Center
V tomto dokumentu se dozvíte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.

> [!NOTE]
> Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center. Tento dokument vám tuto službu představí formou ukázkového nasazení. Není to podrobný průvodce.
> 
> 

## Co je Security Center?
 Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## Sledování partnerských řešení
Dlaždice **Partner solutions** (Partnerská řešení) v okně **Security Center** umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure.
![Dlaždice Partner solutions (Partnerská řešení)][1]

Na dlaždici **Partner solutions** (Partnerská řešení) se zobrazuje řada partnerských řešení a souhrn jejich stavů.

**STAV** partnerských řešení může být následující:

* Chráněné (zelená) – Stav je zcela v pořádku.
* Není v pořádku (červená) – Existuje problém stavu, které si žádá okamžitou pozornost.
* Nehlásí se (oranžová) – Řešení přestalo hlásit svůj stav.
* Neznámý stav ochrany (oranžová) – Stav řešení teď není známý, protože selhal proces přidávání nového prostředku do stávajícího řešení.
* Neuveden (šedá) – Řešení ještě nic nehlásilo. Stav řešení nemusí být uvedený, pokud se řešení teprve nedávno připojilo a ještě probíhá jeho nasazení.

Pokud nejsou ve vašem předplatném integrovaná žádná řešení, dlaždice bude obsahovat informaci, že nejsou dostupná žádná řešení. Po výběru dlaždice **Partner solutions** (Partnerská řešení) budete moct otevřít okno **Recommendations** (Doporučení), které umožňuje nasadit partnerská řešení zabezpečení.
![Žádná partnerská řešení][2]

Stav partnerských řešení zobrazíte takto:

1. Vyberte dlaždici **Partner solutions** (Partnerská řešení). Otevře se okno se seznamem partnerských řešení připojených k Security Center.
   ![Partnerská řešení][3]
2. Vyberte jedno partnerské řešení. V tomto příkladu vybereme řešení **F5-WAF2**.  Otevře se okno, které obsahuje stav tohoto partnerského řešení a jeho přidružené prostředky. Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.
   ![Podrobné zobrazení partnerského řešení][4]
3. Vraťte se do okna **F5-WAF2** a vyberte **Link app** (Připojit aplikaci). Otevře se okno **Link Applications** (Připojit aplikace). Tady můžete ke svému partnerskému řešení připojit prostředky.
   ![Připojení prostředků k partnerskému řešení][5]

## Další kroky
V tomto dokumentu jste se seznámili s dlaždicí **Partner Solutions** (Partnerská řešení) ve službě Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png



<!--HONumber=Jun16_HO2-->


