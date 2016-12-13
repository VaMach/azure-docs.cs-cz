---
title: "Doprovodné materiály k Azure | Dokumentace Microsoftu"
description: "Osvědčené postupy a doprovodné materiály k Azure"
services: 
documentationcenter: na
author: bennage
manager: marksou
editor: 
tags: 
ms.assetid: de94c74a-fea7-4815-8484-553e421a7490
ms.service: guidance
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: christb
translationtype: Human Translation
ms.sourcegitcommit: 5f3ced657cf3d6587a63789b3dd3ca41cd2856f0
ms.openlocfilehash: 0061e1ff2ae2d6b8ed7b7c3bb60405e76d4cc91b


---
# <a name="azure-guidance"></a>Doprovodné materiály k Azure
[!INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tým Microsoftu pro vzory a postupy je součástí zákaznického poradního týmu Azure. Naším cílem je pomáhat vývojářům, architektům a IT specialistům odborníky zajistit úspěšnost na platformě Microsoft Azure. Vyvíjíme doprovodné materiály, které ukazují osvědčené postupy pro vytváření cloudových řešení v Azure.

## <a name="checklists"></a>Kontrolní seznamy
Tyto seznamy pomáhají rychle zkontrolovat základní aspekty dostupnosti a škálovatelnosti. 

* [Kontrolní seznam k dostupnosti][AvailabilityChecklist] 
  
    Přehled doporučených postupů pro zajištění dostupnosti
* [Kontrolní seznam ke škálovatelnosti][ScalabilityChecklist]
  
    Přehled doporučených postupů pro návrh a implementaci škálovatelných služeb a zpracování správy dat

## <a name="best-practices-articles"></a>Články k osvědčeným postupům
Tyto články obsahují podrobné informace o důležitých konceptech běžně spojovaných s cloud computingem. 

* [Návrh rozhraní API][APIDesign] 
  
    Diskuze nad aspekty návrhu, které je potřeba vzít v úvahu při navrhování webového rozhraní API
* [Implementace rozhraní API][APIImplementation] 
  
    Sada doporučených postupů pro implementaci a publikování webového rozhraní API
* [Doprovodné materiály k zabezpečení rozhraní API](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 
  
    Diskuze nad aspekty ověřování a autorizace (například typy tokenů, ověřovací protokoly, autorizační toky a snižování rizik)
* [Pokyny k automatickému škálování][AutoscalingGuidance] 
  
    Souhrn předpokladů pro řešení škálování bez nutnosti ručního zásahu
* [Pokyny k úlohám na pozadí][BackgroundJobsGuidance] 
  
    Popis dostupných možností a doporučených postupů pro implementaci úloh, které by se měly provádět na pozadí, nezávisle na jakýchkoli interaktivních operacích a operacích v popředí.
* [Pokyny k síti pro doručování obsahu (CDN)][CDNGuidance] 
  
    Obecné pokyny a doporučené postupy pro využití CDN k zajištění minimálního zatížení aplikací a maximální dostupnosti a výkonu
* [Pokyny k ukládání do mezipaměti][CachingGuidance] 
  
    Přehled toho, jak ukládání do mezipaměti využít ke zlepšení výkonu a škálovatelnosti systému
* [Pokyny k dělení dat][DataPartitioningGuidance]
  
    Strategie, které je možné využít k dělení dat s cílem vylepšit škálovatelnost, omezit kolize a optimalizovat výkon
* [Pokyny k monitorování a diagnostice][MonitoringandDiagnosticsGuidance] 
  
    Obecné pokyny k tomu, jak sledovat využití systému jednotlivými uživateli a využití prostředků a obecně monitorovat stav a výkon vašeho systému
* [Doporučené zásady vytváření názvů][naming-conventions] 
  
    Doporučené zásady vytváření názvů pro prostředky Azure
* [Obecné pokyny k opakovaným pokusům][RetryGeneralGuidance] 
  
    Diskuze nad obecnými koncepcemi pro zpracování přechodných selhání
* [Pokyny k opakovaným pokusům pro konkrétní služby][RetryServiceSpecificGuidance]
  
    Přehled funkcí opakování pro celou řadu služeb Azure, včetně informací, které pomáhají při využití, úpravách a rozšíření mechanismu opakování pro konkrétní služby

## <a name="scenario-guides"></a>Pokyny pro scénáře
* [Spuštění Elasticsearch v Azure][elasticsearch] 
  
    Elasticsearch je vysoce škálovatelný opensourcový vyhledávací web a databáze. Je vhodný pro situace, které vyžadují rychlou analýzu a zjišťování údajů uložených v rozsáhlých datových sadách. Tyto pokyny se zaměřují na některé klíčové aspekty, které je potřeba vzít v úvahu při navrhování clusteru služby Elasticsearch.
* [Správa identit pro víceklientské aplikace][identity-multitenant] 
  
    Víceklientská architektura je taková, ve které několik vzájemně izolovaných tenantů sdílí jednu aplikaci. V těchto pokynech se dozvíte, jak spravovat identity uživatelů ve víceklientské aplikaci a využívat [Azure Active Directory][AzureAD] pro zpracování přihlašování a ověřování.
* [Vývoj řešení pro velké objemy dat](https://msdn.microsoft.com/library/dn749874.aspx)
  
    Tato příručka se zabývá využitím HDInsight v různých scénářích – při iterativním zkoumání, ve formě datového skladu, pro procesy ETL a pro integraci do stávajících systémů BI. Zahrnuje také vysvětlení konceptů velkých objemů dat a pokyny k plánování a návrhu řešení pro velké objemy dat a implementaci těchto řešení.

## <a name="patterns"></a>Vzory
* [Vzory návrhu v cloudu: Doporučené postupy pro cloudové aplikace](https://msdn.microsoft.com/library/dn568099.aspx)
  
    Článek Vzory návrhu v cloudu nabízí knihovnu vzorů návrhu a související témata s postupy. Zabývá se výhodami využití vzorů a ukazuje, jak jednotlivé vzory zapadají do architektur cloudových aplikací.
* [Optimalizace výkonu pro cloudové aplikace](https://github.com/mspnp/performance-optimization)
  
    Tyto materiály ukazují obvyklé chyby, které aplikacím při zatížení brání ve škálování. Zahrnuje ukázky demonstrující osm typických chyb, [základy analýzy výkonu](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) a pokyny pro [vyhodnocení výkonu na základě klíčových metrik](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Referenční architektury
Naše referenční architektury jsou uspořádané podle scénářů.
Každá jednotlivá architektura nabízí osvědčené postupy, předepsané kroky a spustitelnou komponentu, která tato doporučení shrnuje.

Aktuální knihovny referenčních architektur je dostupná na webu [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Doprovodné materiály k zajištění odolnosti
Tato témata popisují postup návrhu aplikací, které jsou odolné vůči selhání v distribuované cloudovém prostředí.   

* [Přehled odolnosti][ResiliencyOvervew]
  
     Postup vytváření aplikací na platformě Azure, které se mohou zotavit z chyb a nadále fungovat Popisuje strukturovaný přístup pro zajištění odolnosti proti chybám, od návrhu až po implementaci, nasazení a provoz.
* [Kontrolní seznam k odolnosti][resiliency-checklist]
  
    Kontrolní seznam doporučení, který vám pomůže plánovat pro různé režimy selhání, které by mohly nastat
* [Analýza režimu selhání][resiliency-fma] 
  
    Analýza režimu selhání (FMA) je proces pro zajištění odolnosti systému prostřednictvím identifikace možných bodů selhání. Jako výchozí bod pro vaše procesy FMA tento článek obsahuje katalog potenciálních režimů selhání a jejich možných zmírnění. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md




<!--HONumber=Dec16_HO1-->


