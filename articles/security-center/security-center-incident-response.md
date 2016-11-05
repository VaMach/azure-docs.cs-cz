---
title: Využití Azure Security Center při reakci na incidenty | Microsoft Docs
description: Tento dokument popisuje, jak využít Azure Security Center ve scénáři reakce na incidenty.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2016
ms.author: yurid

---
# Využití Azure Security Center při reakci na incidenty
Mnoho organizací se naučí reagovat na incidenty zabezpečení teprve když se stanou obětí útoku. Za účelem snížení nákladů a minimalizace škod je důležité mít hotový plán reakce na incidenty ještě před tím, než k útoku dojde. Azure Security Center můžete využít v různých fázích reakce na incidenty.

## Reakce na incidenty
Efektivní plán závisí na třech základních schopnostech: na schopnosti chránit se před hrozbami, zjišťovat je a reagovat na ně. Účelem ochrany je předcházet incidentům, detekce slouží k včasné identifikaci hrozeb a schopnost reagovat umožňuje vyhnání útočníka a obnovení systému pro zmírnění dopadů porušení zabezpečení. 

V tomto článku se budou používat fáze reakce na incidenty zabezpečení z článku [Reakce zabezpečení Microsoft Azure v cloudu](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678), jak je znázorněno na následujícím diagramu:

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Security Center můžete využít během fází Detekce, Vyhodnocení a Diagnostika. Další informace o každé z těchto fází. Zde je příklad toho, jak může být Security Center užitečná během těchto tří počátečních fází reakce na incidenty:

* **Zjištění**: První indikace vyšetřování události.
  * Příklad: Počáteční ověření, že v řídicím panelu Security Center došlo k vydání výstrahy zabezpečení s vysokou prioritou.
* **Posouzení**: Počáteční zhodnocení a získávání dalších informací o podezřelé aktivitě.
  * Příklad: Získání dalších informací o dané výstraze zabezpečení.
* **Diagnostika**: Provedení technického vyšetřování a určení strategií pro zadržení, zmírnění škod a alternativní řešení.
  * Příklad: Postupujte podle nápravných kroků, které Security Center popisuje v konkrétní výstraze zabezpečení.

Následující scénář vám ukáže, jak můžete využít Security Center během fází incidentu zabezpečení detekce, vyhodnocení a diagnostiky/reakce.  V Security Center představuje [incident zabezpečení](security-center-incident.md) souhrn všech výstrah pro určitý prostředek, které odpovídají schématům modelu [Kill Chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/). Incidenty se zobrazí na dlaždici a v okně [Výstrahy zabezpečení](security-center-managing-and-responding-alerts.md). Incident odhalí seznam souvisejících výstrah, který vám umožní získat další informace o každém výskytu. V Security Center jsou k dispozici také samostatné výstrahy zabezpečení, které lze použít k vysledování podezřelé aktivity.

## Scénář
Společnost Contoso nedávno provedla migraci některých místních prostředků do Azure, včetně některých databází SQL a úloh obchodního systému založených na virtuálních počítačích. Nyní má Hlavní tým reakce na incidenty zabezpečení počítačů (CSIRT) společnosti Contoso problém s vyšetřováním problémů zabezpečení kvůli nedostatku inteligence zabezpečení integrované do současných nástrojů reakce na incidenty. Tato nedostatečná integrace představuje problém během fází detekce (příliš mnoho falešných poplachů), vyhodnocení a diagnostiky. Jako součást této migrace se rozhodli vyjádřit výslovný souhlas s tím, aby jim Security Center pomohl vyřešit tento problém. 

První fáze migrace byla dokončena po připojení všech prostředků a vyřešení všech doporučení zabezpečení od Security Center. CSIRT společnosti Contoso je ústředním bodem řešení incidentů zabezpečení počítačů. Tým se skládá ze skupiny lidí, kteří mají odpovědnost za řešení všech incidentů zabezpečení. Členové týmu mají jasně určené povinnosti, aby se zajistilo, že jsou pokryté všechny oblasti reakce. 

Pro účely tohoto scénáře se budeme soustředit na role následujících osob, které jsou součástí týmu CSIRT společnosti Contoso:

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy je pracovnice oddělení zabezpečení a mezi její povinnosti patří:

* Nepřetržité monitorování a reagování na ohrožení zabezpečení
* Podle potřeby je předat vlastníkovi cloudových úloh nebo analytikovi zabezpečení

Sam je analytik zabezpečení a mezi jeho povinnosti patří:

* Vyšetřování útoků
* Napravení výstrah 
* Práce s vlastníky úloh na určení a aplikaci nápravných kroků

Jak vidíte, Judy a Sam mají rozdílné povinnosti, a musí spolupracovat a sdílet informace získané ze Security Center. 

## Doporučené řešení
Vzhledem k tomu, že Judy a Sam mají rozdílné role, budou k získávání relevantních informací pro své denní aktivity používat různé části Security Center. Judy bude používat jako součást denního monitorování Výstrahy zabezpečení. 

![Výstraha zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy bude používat Výstrahy zabezpečení během fází Detekce a Vyhodnocení. Jakmile Judy dokončí počáteční vyhodnocení, může v případě potřeby dalšího vyšetřování předat problém Samovi. V tomto okamžiku Sam použije informace, které mu poskytne Security Center, někdy i ve spojení s dalšími zdroji dat, a přejde k fázi Diagnostika.

## Postup implementace tohoto řešení
Pro ukázku toho, jak byste použili Azure Security Center ve scénáři reakce na incident, budeme sledovat kroky Judy ve fázích Detekce a Vyhodnocení. Následně se podíváme, co udělá Sam, aby problém diagnostikoval. 

### Fáze reakce na incidenty Detekce a Vyhodnocení
Judy se přihlásila na Azure Portal a nachází se v konzole Security Center. Jako součást denních aktivit monitorování začala kontrolovat výstrahy zabezpečení s vysokou prioritou provedením následujících kroků:

1. Kliknutím na dlaždici **Výstrahy zabezpečení** otevřete okno **Výstrahy zabezpečení**.
    ![Okno Výstrahy zabezpečení](./media/security-center-incident-response/security-center-incident-response-fig4.png)
   
   > [!NOTE]
   > Pro účely tohoto scénáře provede Judy vyhodnocení výstrahy Škodlivá aktivita SQL, jak je znázorněno na obrázku výše. 
   > 
   > 
2. Klikněte na výstrahu **Škodlivá aktivita SQL** a zkontrolujte napadené prostředky v okně **Škodlivá aktivita SQL**:  ![Podrobnosti o incidentu](./media/security-center-incident-response/security-center-incident-response-fig5.png)
   
    V tomto okně si Judy může dělat poznámky týkající se napadených prostředků, kolikrát k útoku došlo a kdy byl zjištěn.
3. Kliknutím na **napadený prostředek** získáte další informace o daném útoku. 

Po přečtení popisu je Judy přesvědčená, že se nejedná o falešný poplach, a že by měla předat tento případ Samovi.

### Fáze reakce na incidenty Diagnostika
Sam od Judy obdržel případ a začal s kontrolováním nápravných kroků, které navrhuje Security Center.

![Životní cyklus reakce na incidenty](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### Další zdroje
Členové týmu reakce na incidenty mohou také využít schopnost [Security Center Power BI](security-center-powerbi.md) a zobrazit různé typy sestav, které jim mohou během dalšího vyšetřování pomoci s vizualizací, analýzou a filtrováním doporučení a výstrah zabezpečení. Pro společnosti, které používají jejích řešení správy událostí a informací o zabezpečení (SIEM), mohou také [integrovat Security Center s vlastním řešením](security-center-integrating-alerts-with-log-integration.md). Protokoly auditu a události zabezpečení virtuálních počítačů Azure lze také integrovat pomocí [nástroje pro integraci protokolů Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). Tyto informace lze použít ve spojení s informacemi, které poskytuje Security Center, k vyšetřování útoku.

## Závěr
Sestavení týmu dříve, než dojde k incidentu, je pro vaši organizaci velmi důležité a pozitivně to ovlivní způsob zpracování incidentů. Dostupnost správných nástrojů pro monitorování prostředků může tomuto týmu pomoci s výběrem správného postupu k napravení incidentu zabezpečení. [Funkce detekce](security-center-detection-capabilities.md) v Security Center mohou IT oddělení pomoci s rychlou reakcí na incidenty zabezpečení a s nápravou problémů zabezpečení.

<!--HONumber=Sep16_HO3-->


