---
title: "Integrace řešení zabezpečení v Azure Security Center | Dokumentace Microsoftu"
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
ms.date: 10/26/2017
ms.author: yurid
ms.openlocfilehash: 0c0029d2dea293e71c6e3daf74b85f0234bfdffd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrace řešení zabezpečení v Azure Security Center
Tento dokument vám pomůže se správou řešení zabezpečení již propojených s Azure Security Center a s přidáním nových řešení.

## <a name="integrated-azure-security-solutions"></a>Integrovaná řešení zabezpečení Azure
Security Center umožňuje snadné povolení integrovaných řešení zabezpečení v Azure. Mezi výhody patří:

- **Zjednodušené nasazení:** Security Center nabízí zjednodušené zřízení integrovaných partnerských řešení. Pro řešení, jako jsou antimalware a hodnocení ohrožení zabezpečení, může Security Center zřídit potřebného agenta na virtuálních počítačích, a pro zařízení brány firewall může Security Center zařídit většinu požadované konfigurace sítě.
- **Integrované detekce:** Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou také sloučeny s detekcemi z jiných zdrojů a poskytují pokročilé možnosti detekce hrozeb.
- **Sjednocená správa a monitorování stavu:** Zákazníci můžou využít integrované události týkající se stavu k monitorování všech partnerských řešení na první pohled. Je dostupná základní správa se snadným přístupem k pokročilému nastavení s použitím partnerského řešení.

Mezi integrovaná řešení zabezpečení v současné době patří:

- Ochrana koncových bodů ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, Windows Defender a System Center Endpoint Protection (SCEP))
- Firewall webových aplikací ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) a [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewall nové generace ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) a [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))
- Posouzení ohrožení zabezpečení ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Prostředí integrace ochrany koncových bodů se může lišit v závislosti na řešení. Následující tabulka obsahuje další podrobnosti o prostředí jednotlivých řešení:

| Endpoint Protection (Ochrana koncových bodů)               | Platformy                             | Instalace Security Center | Zjišťování Security Center |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (antimalware Microsoftu)                  | Windows Server 2016                   | Ne, součást operačního systému           | Ano                       |
| Ochrana koncových bodů System Center (antimalware Microsoftu) | Windows Server 2012 R2, 2012, 2008 R2 | Prostřednictvím rozšíření                | Ano                       |
| Trend Micro – všechny verze         | Řada Windows Serverů                 | Prostřednictvím rozšíření                | Ano                       |
| Symantec v12+                     | Řada Windows Serverů                 | Ne                           | Ano                        |
| MacAfee                           | Řada Windows Serverů                 | Ne                           | Ne                        |
| Kaspersky                         | Řada Windows Serverů                 | Ne                           | Ne                        |
| Sophos                            | Řada Windows Serverů                 | Ne                           | Ne                        |



## <a name="how-security-solutions-are-integrated"></a>Způsob integrace řešení zabezpečení
Řešení zabezpečení Azure nasazená ze služby Security Center se automaticky připojí. Můžete také připojit další zdroje dat zabezpečení, včetně:

- Azure AD Identity Protection
- Počítače spuštěné místně nebo v jiných cloudech
- Řešení zabezpečení s podporou formátu CEF (Common Event Format)
- Microsoft Advanced Threat Analytics

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Správa integrovaných řešení zabezpečení Azure a dalších zdrojů dat

Po nasazení můžete zobrazit informace o stavu integrovaných řešení zabezpečení Azure a provádět základní úlohy správy. Můžete také připojit další typy zdrojů dat zabezpečení, jako jsou upozornění služby Azure Active Directory Identity Protection a protokoly brány firewall ve formátu CEF (Common Event Format). Na řídicím panelu Security Center vyberte Řešení zabezpečení.

### <a name="connected-solutions"></a>Připojená řešení

Část **Připojená řešení** obsahuje řešení zabezpečení, která jsou aktuálně připojená ke službě Security Center, a informace o stavu jednotlivých řešení.  

![Připojená řešení](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

### <a name="discovered-solutions"></a>Zjištěná řešení

V části **Zjištěná řešení** se zobrazují všechna řešení přidaná prostřednictvím Azure. Zobrazují se také řešení, u kterých služba Security Center navrhuje, že by se k ní měla připojit.

![Zjištěná řešení](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center automaticky zjišťuje další řešení zabezpečení provozovaná v Azure. To zahrnuje řešení Azure, jako je [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), i partnerská řešení provozovaná v Azure. Pokud chcete integrovat tato řešení se službou Security Center, vyberte **PŘIPOJIT**.

### <a name="add-data-sources"></a>Přidat zdroje dat

Část **Přidat zdroje dat** obsahuje další dostupné zdroje dat, které je možné připojit. Pokyny k přidání dat z některého z těchto zdrojů získáte kliknutím na **PŘIDAT**.

![Zdroje dat](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Další kroky

V tomto článku jste se naučili integrovat partnerská řešení do služby Security Center. Další informace o službě Security Center najdete v následujících článcích:

* [Průvodce plánováním a provozem služby Security Center](security-center-planning-and-operations-guide.md)
* [Propojení Microsoft Advanced Threat Analytics a Azure Security Center](security-center-ata-integration.md)
* [Propojení Azure Active Directory Identity Protection a Azure Security Center](security-center-aadip-integration.md)
* [Monitorování stavu zabezpečení ve službě Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Monitorování partnerských řešení pomocí služby Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Získejte odpovědi na nejčastější dotazy k používání služby Security Center.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
