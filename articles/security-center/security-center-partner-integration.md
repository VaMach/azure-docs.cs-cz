---
title: "Partnerská řešení a jejich integrace v Azure Security Center | Dokumentace Microsoftu"
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
ms.date: 09/12/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8cc44da0f61362018d2757da58ca4fb3a9a43764
ms.contentlocale: cs-cz
ms.lasthandoff: 09/14/2017

---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Partnerská řešení a jejich integrace v Azure Security Center

V tomto článku probereme, jak Azure Security Center pomáhá integrací s partnerskými řešeními zvýšit celkové zabezpečení. Security Center nabízí integrované prostředí v Azure a využívá Azure Marketplace pro certifikaci partnerů a účtování.

## <a name="why-deploy-partner-solutions-from-security-center"></a>Proč nasazovat partnerská řešení ze služby Security Center

Existují čtyři hlavní důvody k využití integrace partnerských řešení ve službě Security Center:

- **Snadné nasazení**. Nasazení partnerských řešení podle doporučení služby Security Center je mnohem jednodušší. Proces nasazení je možné plně automatizovat využitím výchozího nastavení a topologie sítě. Alternativně můžou zákazníci zvolit možnost poloautomatického nasazení s větší flexibilitou a možnostmi přizpůsobení.
- **Integrované detekce**. Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou také sloučeny s detekcemi z jiných zdrojů a poskytují pokročilé možnosti detekce hrozeb.
- **Jednotné monitorování stavu a správa**. Zákazníci můžou pomocí integrovaných událostí stavu monitorovat všechna partnerská řešení na první pohled. Je dostupná základní správa se snadným přístupem k pokročilému nastavení s použitím partnerského řešení.
- **Export do SIEM**. Zákazníci můžou pomocí integrace protokolů Azure exportovat všechny výstrahy služby Security Center a partnerských řešení ve formátu CEF (Common Event Format) do místních systémů správy událostí a informací o zabezpečení (SIEM).


## <a name="partners-that-integrate-with-security-center"></a>Partnerská řešení s možností integrace se službou Security Center

Aktuálně jsou na webu Azure Marketplace k dispozici následující partnerská řešení s možností nativní integrace se službou Security Center:

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

Pokud po nasazení chcete zobrazit informace o stavu řešení a provádět základní úlohy správy, na řídicím panelu **Security Center** vyberte možnost **Partnerská řešení**.

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Obsah, který se zobrazí po otevření řešení zabezpečení, se může lišit v závislosti na vaší infrastruktuře. Když jako příklad použijeme předchozí obrázek, stránka má tři části:

- **Připojená řešení:** Zobrazuje řešení připojená ke službě Security Center.
- **Zjištěná řešení:** Zobrazuje řešení, která nejsou připojená ke službě Security Center. Tato řešení můžete připojit a pak se zobrazí v části Připojená řešení.  Pokud služba Security Center nezjistí žádná nepřipojená řešení, tato část bude skrytá.
- **Přidat zdroje dat:** Zobrazuje zdroje dat v Azure a mimo Azure, které můžete přidat do služby Security Center.

### <a name="connected-solutions"></a>Připojená řešení

V části **Připojená řešení** se zobrazují všechna řešení zabezpečení, která jsou aktuálně propojená se službou Security Center. 

![Připojená řešení](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Informace zobrazené u jednotlivých řešení se můžou lišit v závislosti na řešení. Mezi informacemi dostupnými na každé dlaždici může být:

- Ikona společnosti partnera.  Pokud služba Security Center nemá ikonu společnosti, zobrazí se první znak(y) názvu partnera.
- Typ řešení.
- Může se zobrazit název počítače.
- Stav.  Pokud se indikátor stavu neodešle, služba Security Center zobrazí datum a čas poslední přijaté události, aby indikovala, jestli se zařízení hlásí, nebo ne. Pokud služba Security Center neobdrží indikátor stavu z konkrétního řešení, dlaždice takového řešení se v této části nezobrazí.

> [!NOTE]
> Všimněte si, že služba Security Center zobrazuje datum a čas poslední přijaté události, aby indikovala, jestli se zařízení hlásí, nebo ne. Řešení, která neodesílají indikátor stavu, se zobrazují jako připojená, pokud odeslala upozornění nebo událost během posledních 14 dní.
>  

Některá z těchto řešení můžou být plně integrovaná v Azure, jiná můžou být místní. Vzhledem k tomu, že služba Security Center podporuje [formát CEF (Common Event Format)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef), může se propojit s řešeními používajícími formát CEF, jako je brána firewall s podporou formátu CEF. Po přidání tohoto řešení do služby Security Center odesílá brána firewall protokoly ve formátu CEF do služby Security Center, která je předává ke zpracování do [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Brána firewall je prostředek mimo Azure a odesílá události, ale ne indikátor stavu.  Jediná informace, kterou má služba Security Center o stavu, je čas posledního odeslání události z tohoto zařízení.  Pro všechny prostředky mimo Azure zobrazí služba Security Center na dlaždici v oblasti stavu datum a čas přijetí poslední události, což indikuje, že se prostředek mimo Azure stále hlásí.

### <a name="discovered-solutions"></a>Zjištěná řešení

V části **Zjištěné prostředky** se zobrazují všechna řešení přidaná prostřednictvím Azure a služba Security Center navrhuje jejich připojení.

![Zjištěná řešení](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Služba Security Center se může integrovat s integrovanými řešeními Azure, jako je [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection). Pokud máte licenci řešení Azure AD Identity Protection, které ale není připojené ke službě Security Center, bude řešení Azure AD Identity Protection uvedeno v části **Zjištěná řešení**. Pokud chcete toto řešení integrovat se službou Security Center, klikněte na **PŘIPOJIT** na dlaždici **Azure AD Identity Protection** a zobrazí se následující stránka:

![Azure AD Identity Protection](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

K dokončení připojení Azure AD Identity Protection je potřeba vybrat pracovní prostor, ve kterém jsou data uložena. Veškerá data z Azure AD Identity Protection budou přicházet z oblasti pracovního prostoru vybraného v tomto kroku.  Pomocí selektoru pracovního prostoru vyberete pracovní prostor a začnou do něj přicházet data.

Aby bylo možné připojení ke službě Security Center, musíte být globálním správcem nebo správcem zabezpečení.  Pokud nemáte příslušná oprávnění, tlačítko **Připojit** bude neaktivní a zobrazí se zpráva s vysvětlením, proč je tlačítko neaktivní.

Upozornění Azure AD Identity Protection procházejí kanálem detekce služby Security Center. To umožňuje získávat upozornění ze služby Security Center i z Azure Active Directory Identity Protection. Služba Security Center sloučí všechna pravděpodobně související upozornění a vytvoří [incident zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-incident). V popisu incidentu zabezpečení najdete další informace o podezřelé aktivitě.

### <a name="add-data-sources"></a>Přidat zdroje dat

Pro integraci se službou Security Center můžete přidat počítače Azure i počítače mimo Azure.  Přidání počítačů mimo Azure znamená, že můžete přidat místní počítač nebo zařízení, které podporuje formát CEF. 

![Zdroje dat](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Viz také

V tomto článku jste se naučili integrovat partnerská řešení do služby Azure Security Center. Další informace o službě Security Center najdete v následujících článcích:

* [Průvodce plánováním a provozem služby Security Center](security-center-planning-and-operations-guide.md)
* [Správa a reakce na výstrahy zabezpečení ve službě Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Security Center](security-center-alerts-type.md)
* [Monitorování stavu zabezpečení ve službě Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Monitorování partnerských řešení pomocí služby Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Získejte odpovědi na nejčastější dotazy k používání této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

