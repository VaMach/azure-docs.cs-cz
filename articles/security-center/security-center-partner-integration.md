---
title: "Integrace partnerských řešení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Zjistěte, jak Azure Security Center umožňuje integrací s partnerskými řešeními zvýšit celkové zabezpečení vašich prostředků Azure."
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
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 4d0909e926de14a0cbe9799b969ac7a1946d69d1
ms.contentlocale: cs-cz
ms.lasthandoff: 07/10/2017


---
# <a name="partner-integration-in-azure-security-center"></a>Integrace partnerských řešení ve službě Azure Security Center

V tomto článku probereme, jak Azure Security Center pomáhá integrací s partnerskými řešeními zvýšit celkové zabezpečení. Security Center nabízí integrované prostředí v Azure a využívá Azure Marketplace pro certifikaci partnerů a účtování.

> [!NOTE] 
> Od června 2017 používá Security Center ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v tématu [Migrace platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Proč nasazovat partnerská řešení ze služby Security Center

Existují čtyři hlavní důvody k využití integrace partnerských řešení ve službě Security Center:

- **Snadné nasazení**. Nasazení partnerských řešení podle doporučení služby Security Center je mnohem jednodušší. Proces nasazení je možné plně automatizovat využitím výchozího nastavení a topologie sítě. Alternativně můžou zákazníci zvolit možnost poloautomatického nasazení s větší flexibilitou a možnostmi přizpůsobení.
- **Integrované detekce**. Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou také sloučeny s detekcemi z jiných zdrojů a poskytují pokročilé možnosti detekce hrozeb.
- **Jednotné monitorování stavu a správa**. Zákazníci můžou pomocí integrovaných událostí stavu monitorovat všechna partnerská řešení na první pohled. Je dostupná základní správa se snadným přístupem k pokročilému nastavení s použitím partnerského řešení.
- **Export do SIEM**. Zákazníci můžou pomocí integrace protokolů Azure exportovat všechny výstrahy služby Security Center a partnerských řešení ve formátu CEF (Common Event Format) do místních systémů správy událostí a informací o zabezpečení (SIEM).


## <a name="partners-that-integrate-with-security-center"></a>Partnerská řešení s možností integrace se službou Security Center

Security Center se aktuálně integruje s těmito řešeními:

- Ochrana koncových bodů ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec a [Microsoft Antimalware pro Azure Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Firewall webových aplikací ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) a [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall nové generace ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) a [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Posouzení ohrožení zabezpečení ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Postupem času bude Security Center rozšiřovat počet partnerů v těchto kategoriích a přidávat nové kategorie. 

## <a name="deploy-a-partner-solution"></a>Nasazení partnerského řešení

V závislosti na nastavení prostředí Azure a nadefinovaných zásadách zabezpečení může Security Center doporučit nasazení partnerského řešení. Doporučení služby Security Center vás provede procesem výběru a instalace partnerského řešení. Celkové prostředí nasazení se může lišit podle použitého typu řešení a partnera. Další informace najdete v následujících článcích:

- [Instalace Endpoint Protection](security-center-install-endpoint-protection.md)
- [Přidání brány firewall webových aplikací](security-center-add-web-application-firewall.md)
- [Přidání brány firewall nové generace](security-center-add-next-generation-firewall.md)
- [Není nainstalováno posouzení ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Správa partnerských řešení

Pokud po nasazení chcete zobrazit informace o stavu řešení a provádět základní úlohy správy, v okně **Security Center** vyberte dlaždici **Partnerská řešení**. Další informace o správě partnerských řešení v Security Center najdete v článku [Monitorování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md).

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> Podpora pro Symantec Endpoint Protection je omezena na zjišťování. Nejsou k dispozici žádné výstrahy týkající se stavu.
>

## <a name="see-also"></a>Viz také

V tomto článku jste se naučili integrovat partnerská řešení do služby Azure Security Center. Další informace o službě Security Center najdete v následujících článcích:

* [Průvodce plánováním a provozem služby Security Center](security-center-planning-and-operations-guide.md)
* [Správa a reakce na výstrahy zabezpečení ve službě Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Security Center](security-center-alerts-type.md)
* [Monitorování stavu zabezpečení ve službě Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Monitorování partnerských řešení pomocí služby Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Získejte odpovědi na nejčastější dotazy k používání této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

