---
title: "Integrace partnerských řešení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument popisuje, jak Azure Security Center umožňuje integrací s partnery zvýšit celkové zabezpečení vašich prostředků Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 85460ea37d251b53074de9d832610d253700f2c1
ms.contentlocale: cs-cz
ms.lasthandoff: 06/24/2017


---
<a id="partner-integration-in-azure-security-center" class="xliff"></a>

# Integrace partnerských řešení ve službě Azure Security Center
Tento dokument popisuje, jak Azure Security Center umožňuje integrací s partnery zvýšit celkové zabezpečení, poskytnout v Azure integrované prostředí a současně využít výhod služby Azure Marketplace pro certifikaci partnerů a účtování.

>[!NOTE] 
>Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>

<a id="why-deploy-partners-solutions-from-security-center" class="xliff"></a>

## Proč nasazovat partnerská řešení ze služby Security Center?

Existují čtyři hlavní důvody využití integrace partnerských řešení ve službě Security Center:

- **Snadné nasazení**: Nasazení partnerských řešení podle doporučení služby Security Center je mnohem jednodušší. Proces nasazení lze plně automatizovat pomocí výchozí konfigurace a síťové topologie, případně mohou zákazníci zvolit poloautomatický postup s větší flexibilitou a možnostmi přizpůsobení konfigurace.
- **Integrované detekce**: Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou sloučeny také s detekcemi z jiných zdrojů a poskytují pokročilé možnosti detekce hrozeb.
- **Sjednocená správa a monitorování stavu**: Integrované události týkající se stavu umožňují zákazníkům monitorování všech partnerských řešení na jeden pohled. Je dostupná základní správa se snadným přístupem k pokročilé konfiguraci s použitím partnerských řešení.
- **Export do SIEM:** Zákazníci teď můžou exportovat všechny výstrahy služby Security Center a partnerského řešení ve formátu CEF do místních systémů SIEM s použitím služby Microsoft Azure Log Integration (Preview).


<a id="what-partners-are-integrated-with-security-center" class="xliff"></a>

## Kteří partneři se integrují se službou Security Center?
Security Center se v současnosti integruje s řešeními:

- Ochrana koncových bodů ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Microsoft Antimalware for Azure](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Firewall webových aplikací ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), [WAF služby Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall nové generace ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) a [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Posouzení ohrožení zabezpečení ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Postupem času bude Security Center rozšiřovat počet partnerů v těchto kategoriích a přidávat nové kategorie. 

<a id="how-to-deploy-a-partner-solution" class="xliff"></a>

## Jak nasadit partnerské řešení?

V závislosti na konfiguraci prostředí Azure a nadefinovaných zásadách zabezpečení může Security Center doporučit nasazení partnerského řešení. Doporučení vás provede procesem výběru a instalace partnerského řešení. Celkové prostředí nasazení se v tomto okamžiku může lišit podle typu řešení a partnera. Další informace najdete prostřednictvím následujících odkazů:

- [Přidání brány firewall webových aplikací](security-center-add-web-application-firewall.md)
- [Přidání brány firewall příští generace](security-center-add-next-generation-firewall.md)
- [Instalace Endpoint Protection](security-center-install-endpoint-protection.md)
- [Není nainstalováno posouzení ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md)

<a id="how-to-manage-partner-solutions" class="xliff"></a>

## Jak spravovat partnerská řešení?

Jakmile je partnerské řešení nasazeno, můžete zobrazit informace o stavu řešení a provádět základní úlohy správy z dlaždice Partnerská řešení na hlavním řídicím panelu Security Center. Další informace o správě partnerských řešení v Security Center najdete v článku [Monitorování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md).

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> Podpora pro Symantec Endpoint Protection je omezena na zjišťování, ale nejsou k dispozici žádné výstrahy týkající se stavu.
>

<a id="see-also" class="xliff"></a>

## Viz také
V tomto dokumentu jste se naučili integrovat partnerská řešení do služby Azure Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Azure Security Center](security-center-alerts-type.md)
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

