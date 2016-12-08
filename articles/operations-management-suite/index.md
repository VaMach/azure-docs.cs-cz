---
title: "Přehled Operations Management Suite (OMS) | Dokumentace Microsoftu"
description: "Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.  Tento článek identifikuje různé služby zahrnuté v OMS a poskytuje odkazy na podrobné informace."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 4f14a461b842649de977d2c4c80edfc197e15657
ms.openlocfilehash: 14274c7d94213681b3779b97a44296e6eaf90be9


---
# <a name="what-is-operations-management-suite-oms"></a>Co je Operations Management Suite (OMS)?
Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.  Vzhledem k tomu, že je OMS implementována jako cloudová služba, je možné ji zprovoznit velmi rychle a s minimální investicí do služeb infrastruktury.  Nové funkce jsou doručovány pravidelně, což šetří průběžné náklady na údržbu a aktualizace.

Kromě samotného poskytování přínosných služeb se OMS může integrovat s komponentami služby System Center, například s nástrojem System Center Operations Manager, a rozšířit tak vaše stávající investice do správy do cloudu.  System Center a OMS mohou díky vzájemné spolupráci poskytnout úplné hybridní prostředí pro správu.

Následující oddíly poskytují obecný popis oblastí OMS s různou hodnotou a služeb, které je implementují.  V architektuře OMS najdete přehled různých komponent OMS před tím, než se pustíte do procházení podrobné dokumentace k jednotlivým komponentám.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Statistiky a analýza](media/operations-management-suite-overview/icon-insight-analytics.png) Statistiky a analýza
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) pomáhá shromažďovat, korelovat, vyhledávat a jednat podle dat protokolu a dat o výkonu vygenerovaných operačními systémy a aplikacemi. Poskytuje statistiky provozu v reálném čase pomocí integrovaného vyhledávání a vlastních řídicích panelům, které snadno analyzují milióny záznamů napříč všemi vašimi úlohami a servery bez ohledu na jejich fyzické umístění.

Do Log Analytics můžete jednoduše přidávat řešení, která definují, jaká data se mají shromažďovat a logiku pro jejich analýzu.  Řešení mohou obsahovat další funkce, které se automaticky doručí do agentů s potřebou minimální nebo dokonce žádné konfigurace.  Kromě používání analytických nástrojů, které poskytují jednotlivá řešení, můžete provádět vlastní vyhledávání napříč celou datovou sadou pro korelaci dat mezi systémy a aplikacemi.  

## <a name="automation-controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation-control"></a>![Automatizace a řízení](media/operations-management-suite-overview/icon-automation-control.png) Automatizace a řízení
Azure Automation umožňuje automatizovat procesy správy pomocí [runbooků](../automation/automation-runbook-types.md) založených na prostředí PowerShell, které běží v cloudu Azure.  Runbooky mohou přistupovat ke všem produktům a službám, které lze spravovat pomocí prostředí PowerShell, včetně prostředků v dalších cloudech, jako je například Amazon Web Services (AWS).  Runbooky se mohou provádět také na serveru ve vašem místním datovém centru a mohou spravovat místní prostředky.

Azure Automation poskytuje správu konfigurace pomocí [PowerShell DSC](../automation/automation-dsc-overview.md).  Můžete vytvářet a spravovat prostředky DSC hostované v Azure a použít je na cloudové a místní systémy, aby definovaly a automaticky vynutily jejich konfiguraci.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Ochrana a zotavení](media/operations-management-suite-overview/icon-protection-recovery.png) Ochrana a zotavení po havárii
Služba [Azure Backup](http://azure.microsoft.com/documentation/services/backup) chrání data vaší aplikace a dlouhá léta je uchovává bez nutnosti velkých investic a s minimálními provozními náklady.  Kromě úloh aplikací, jako jsou například SQL Server a SharePoint, umožňuje zálohovat i data z fyzických a virtuálních serverů Windows.  Službu může využít i System Center Data Protection Manager (DPM) k replikaci chráněných dat do Azure pro zajištění redundance a dlouhodobého uložení.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) přispívá ke strategii zachování plynulého chodu podniku a zotavení po havárii (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení místních virtuálních počítačů Hyper-V a VMware a fyzických serverů s Windows nebo Linuxem. Počítače můžete replikovat do sekundárního datového centra nebo rozšířit své datové centrum jejich replikací do Azure. Site Recovery také poskytuje možnosti jednoduchého převzetí služeb při selhání úloh a jejich obnovení. Integruje se s mechanismy zotavení po havárii, jako je SQL Server AlwaysOn, a poskytuje plány obnovení pro snadné převzetí služeb při selhání úloh vrstvených napříč více počítači.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![Zabezpečení a dodržování předpisů v OMS](media/operations-management-suite-overview/icon-security-compliance.png) Zabezpečení a dodržování předpisů
Zabezpečení a dodržování předpisů pomáhá identifikovat, vyhodnotit a zmírnit bezpečnostní rizika pro vaši infrastrukturu.  Tyto funkce OMS jsou implementovány prostřednictvím více řešení v Log Analytics, která analyzují data protokolů a konfigurace ze systémů agentů, a tak vám pomáhají zajistit průběžné zabezpečení prostředí.

* [Řešení Zabezpečení a audit](oms-security-getting-started.md) shromažďuje a analyzuje události zabezpečení na spravovaných systémech a identifikuje podezřelou aktivitu.
* [Antimalwarové řešení](../log-analytics/log-analytics-malware.md) podává zprávy o stavu antimalwarové ochrany na spravovaných systémech.  
* Řešení Aktualizace systému provádí analýzu bezpečnostních a dalších aktualizací na spravovaných systémech, abyste mohli jednoduše identifikovat systémy, které vyžadují opravy chyb.

## <a name="next-steps"></a>Další kroky
* Další informace o [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Další informace o [Azure Automation](../automation/automation-intro.md).
* Další informace o [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Další informace o [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).




<!--HONumber=Nov16_HO4-->


