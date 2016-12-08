---
title: "Integrace partnerských řešení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument popisuje, jak Azure Security Center umožňuje integrací s partnery zvýšit celkové zabezpečení vašich prostředků Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 0c946ce6a96f2e3644b9890dad5d60a35ad4bcb7
ms.openlocfilehash: 10184c52d532eb56e66212fafdea3d059b0c43e3


---
# <a name="partner-integration-in-azure-security-center"></a>Integrace partnerských řešení ve službě Azure Security Center
Tento dokument popisuje, jak Azure Security Center umožňuje integrací s partnery zvýšit celkové zabezpečení, poskytnout v Azure integrované prostředí a současně využít výhod služby Azure Marketplace pro certifikaci partnerů a účtování.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Proč nasazovat partnerská řešení ze služby Security Center?

Existují čtyři hlavní důvody využití integrace partnerských řešení ve službě Security Center:

- **Snadné nasazení**: Nasazení partnerských řešení podle doporučení služby Security Center je mnohem jednodušší. Proces nasazení lze plně automatizovat pomocí výchozí konfigurace a síťové topologie, případně mohou zákazníci zvolit poloautomatický postup s větší flexibilitou a možnostmi přizpůsobení konfigurace.
- **Integrované detekce**: Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou sloučeny také s detekcemi z jiných zdrojů a poskytují pokročilé možnosti detekce hrozeb.
- **Sjednocená správa a monitorování stavu**: Integrované události týkající se stavu umožňují zákazníkům monitorování všech partnerských řešení na jeden pohled. Je dostupná základní správa se snadným přístupem k pokročilé konfiguraci s použitím partnerských řešení.
- **Export do SIEM**: Zákazníci teď můžou exportovat všechny výstrahy služby Security Center a partnerského řešení ve formátu CEF do místních systémů SIEM s použitím služby Microsoft Azure Log Integration (Preview).


## <a name="what-partners-are-integrated-with-security-center"></a>Kteří partneři se integrují se službou Security Center?
Security Center se v současnosti integruje s řešeními následujících partnerů:

- Ochrana koncových bodů (Trend Micro) 
- Firewall webových aplikací (Barracuda, F5, Imperva a brzy také Microsoft WAF a Fortinet), 
- Firewall nové generace (Check Point, Barracuda a brzy také Fortinet a Cisco) 
- Posouzení ohrožení zabezpečení (Qualys – Preview) 

Postupem času bude Security Center rozšiřovat počet partnerů v těchto kategoriích a přidávat nové kategorie. 

## <a name="how-to-deploy-a-partner-solution"></a>Jak nasadit partnerské řešení?

Partnera řešení, které už jsou v Centru zabezpečení nasazená, jsou snadno přístupná na dlaždici Partnerská řešení v hlavním řídicím panelu Security Center:

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig1.png)

Nasazení nového partnerského řešení na základě doporučení služby Security Center provedete takto:

> [!NOTE]
> Postup v následující příkladu předpokládá, že máte úlohy, které chcete chránit pomocí brány firewall webových aplikací.

1. Na řídicím panelu Security Center klikněte na dlaždici **Doporučení**.
2. V okně **Doporučení** klikněte na možnost **Přidat bránu firewall webových aplikací**.
3. Klikněte na název aplikace v okně **Přidání brány firewall webových aplikací**.
4. V okně **Přidání brány firewall webových aplikací** klikněte na **Vytvořit novou**.
5. Okno **Vytvoření nové brány firewall webových aplikací** okno nabízí seznam aktuálních partnerských řešení, implementujících bránu firewall webových aplikací.
6. Vyberte požadované partnerské řešení a postupujte podle zobrazených pokynů (které se mohou lišit v závislosti na partnerovi).

Také samotné prostředí nasazení se v tomto okamžiku může mezi různými partnery lišit. Další informace o správě partnerských řešení ve službě Security Center najdete v článku [Sledování partnerských řešení](security-center-partner-solutions.md) v Azure Security Center.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili integrovat partnerská řešení do služby Azure Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Azure Security Center](security-center-alerts-type.md)
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Nov16_HO5-->


