---
title: "Předkonfigurované řešení prediktivní údržby | Dokumentace Microsoftu"
description: "Popis předkonfigurovaného řešení prediktivní údržby pomocí platformy Azure IoT"
services: 
suite: iot-suite
documentationcenter: 
author: stevehob
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2016
ms.author: araguila
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 32da7abc3263e1d76f3f08ccae7ce159ead58bee


---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Přehled řešení předkonfigurované prediktivní údržby
Předkonfigurované řešení *prediktivní údržby* je jedním z [předkonfigurovaných řešení][lnk_preconfigured_solutions] vydaných jako součást sady [Microsoft Azure IoT Suite][lnk_iot_suite]. Toto řešení integruje sběr telemetrických dat ze zařízení v reálném čase s prediktivním modelem vytvořeným pomocí služby [Azure Machine Learning][lnk_machine_learning].

Pomocí sady Azure IoT Suite se mohou podniky snadno a rychle připojit k prostředkům, monitorovat je a analyzovat data v reálném čase. Předkonfigurované řešení prediktivní údržby přebírá data a pomocí podrobných řídicích panelů a vizualizací podnikům poskytuje nové informace, které jim můžou pomoct zvýšit efektivitu a výnosy.

## <a name="the-scenario"></a>Scénář
Fabrikam je regionální letecká společnost, která se zaměřuje na pohodlí zákazníků za konkurenční ceny. Jednou z příčin zpoždění letů jsou problémy s údržbou, protože údržba leteckých motorů je velmi náročná. Poruchám motorů během letu je nutné za každou cenu zabránit, a proto společnost Fabrikam své stroje pravidelně kontroluje a dodržuje program plánované údržby. Letecké motory se ale neopotřebovávají všechny stejně. Některou údržbu motorů není vždy nutné provádět. Naproti tomu se můžou vyskytnout problémy, kvůli kterým musí letadlo zůstat na zemi, dokud není opravené. To způsobuje nákladná zdržení, zejména v pokud je letadlo v místě, kde nejsou k dispozici vhodní technici nebo náhradní díly.

Motory letadel společnosti Fabrikam jsou vybaveny snímači, které monitorují stav motoru během letu. Společnost Fabrikam pomocí sady Azure IoT Suite sbírá data shromážděná během letu. Z údajů o provozu a selháních motorů nashromážděných za mnoho let analytici společnosti Fabrikam vytvořili model, který předpovídá zbývající dobu životnosti (RUL) leteckého motoru. Zjistili, že existuje korelace mezi údaji ze čtyř snímačů v motoru a opotřebením motoru, které vede k jeho případnému selhání. I když společnost Fabrikam stále provádí pravidelné kontroly k zajištění bezpečnosti, může pomocí modelu vypočítat zbývající životnost jednotlivých motorů po každém letu na základě telemetrických údajů shromážděných z motorů během letu. Společnost Fabrikam teď může předpovídat budoucí selhání a s předstihem plánovat údržbu a opravy.

> [!NOTE]
> Model řešení využívá data o opotřebení ze skutečných motorů.
> 
> 

Díky tomu, že společnost Fabrikam dokáže předpovědět čas potřebné údržby, může optimalizovat svůj provoz a snižovat náklady. Koordinátoři údržby spolu s plánovači plánují údržbu tak, aby se časově shodovala se zastávkami letadel v konkrétních místech, a aby servis letadel nezpůsoboval komplikace v časovém harmonogramu. Společnost Fabrikam může podle tohoto plánu rozvrhnout služby příslušných techniků a zajistit, že servis letadla bude probíhat efektivně a bez prodlev. Plán údržby dostávají také správci řízení zásob, kteří díky tomu mohou optimalizovat proces objednávání a skladové zásoby náhradních dílů. Všechny tyto informace umožňují společnosti Fabrikam minimalizovat prostoje letadel a snižovat provozní náklady a současně zajistit bezpečnost cestujících i posádek.

Vysvětlení, jak sada [Azure IoT Suite][lnk_iot_suite] zákazníkům umožňuje využít potenciál prediktivní údržby, najdete v této [infografice][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Jak je sestaveno řešení prediktivní údržby
Řešení využívá existující model Azure Machine Learning, který je k dispozici jako šablona a ukazuje možnosti práce s telemetrickými údaji ze zařízení shromážděnými prostřednictvím služeb IoT Suite. Společnost Microsoft vytvořila [regresní model][lnk_regression_model] leteckého motoru a zveřejnila úplnou šablonu, data <sup>\[1\]</sup> a podrobné pokyny k použití modelu.

Předkonfigurované řešení prediktivní údržby Azure IoT používá regresní model vytvořený z této šablony, je nasazené do vašeho předplatného Azure a vystavené prostřednictvím automaticky generovaného rozhraní API. Řešení obsahuje podmnožinu testovacích dat, která představují 4 motory (z celkem 100) a 4 datové proudy ze snímačů (z celkem 21), které pro cvičný model poskytují přesné výsledky.

*\[1\] A. Saxena and K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set“, datové úložiště NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="next-steps"></a>Další kroky
Další informace o tom, jak Azure IoT podporuje scénáře prediktivní údržby, najdete v tématu [Získání hodnoty z Internetu věcí][lnk_capture_value].

[Návod][lnk-predictive-walkthrough] pro předkonfigurované řešení prediktivní údržby.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT od počátku][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Dec16_HO1-->


