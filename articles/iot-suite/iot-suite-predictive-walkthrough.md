---
title: "Návod pro prediktivní údržbu | Dokumentace Microsoftu"
description: "Návod pro předkonfigurované řešení prediktivní údržby Azure IoT."
services: 
suite: iot-suite
documentationcenter: 
author: aguilaaj
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2016
ms.author: araguila
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 6d80f266730701dc4e678f4365a2166be72c9143


---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Návod pro předkonfigurované řešení prediktivní údržby
## <a name="introduction"></a>Úvod
Předkonfigurované řešení prediktivní údržby je uceleným řešením pro podnikový scénář, které se pokouší předvídat bod, ve kterém pravděpodobně nastane chyba. Toto předkonfigurované řešení můžete aktivně využívat pro různé činnosti, jako je třeba optimalizace údržby. Řešení kombinuje klíčové služby sady Azure IoT Suite, včetně pracovního prostoru [Azure Machine Learning][lnk_machine_learning]. Tento pracovní prostor obsahuje experimenty založené na veřejné ukázkové datové sadě, která předpovídají zbývající dobu životnosti (RUL) leteckého motoru. Řešení nabízí úplnou implementaci daného obchodního scénáře IoT jako výchozího bodu pro plánování a implementaci řešení, které vyhovuje vašim konkrétním obchodním požadavkům.

## <a name="logical-architecture"></a>Logická architektura
Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:

![][img-architecture]

Modré položky jsou služby Azure, které jsou zřízené v umístění vybraném při zřizování daného předkonfigurovaného řešení. Předkonfigurované řešení můžete zřídit v oblasti východu USA, severní Evropy nebo jihovýchodní Asie.

Některé prostředky nejsou v oblastech, kde zřídíte předkonfigurované řešení, k dispozici. Oranžové položky v diagramu představují služby Azure zřízené v nejbližší dostupné oblasti (jižní střední USA, západní Evropa nebo jihovýchodní Asie) podle vybrané oblasti.

Zelená položka je simulované zařízení, které představuje letecký motor. Další informace o těchto simulovaných zařízeních najdete v následující části.

Šedé položky představují součásti, které implementují funkce *správy zařízení*. Aktuální verze předkonfigurovaného řešení prediktivní údržby tyto prostředky neposkytuje. Další informace o správě zařízení najdete v tématu [Předkonfigurované řešení pro vzdálené monitorování][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulovaná zařízení
V předkonfigurovaných řešeních simulované zařízení představuje letecký motor. Řešení obsahuje dva motory, které jsou součástí jednoho letadla. Každý motor vysílá čtyři typy telemetrických dat: ze snímače Sensor 9, Sensor 11, Sensor 14 a Sensor 15, které poskytují data potřebná k tomu, aby mohl model Machine Learning vypočítat zbývající dobu životnosti (RUL) pro tento motor. Každé simulované zařízení posílá do služby IoT Hub následující telemetrické zprávy:

*Počet cyklů*. Cyklus představuje dokončený let proměnlivé délky v rozmezí 2 až 10 hodin, během kterého se každou půlhodinu zaznamenávají telemetrická data.

*Telemetrická data*. V řešení jsou čtyři snímače, které představují vlastnosti motoru. Snímače jsou obecně označeny jako Sensor 9, Sensor 11, Sensor 14 a Sensor 15. Tyto 4 snímače představují dostatek telemetrických dat k tomu, aby bylo možné pomocí modelu Machine Learning dosáhnout pro RUL užitečných výsledků. Tento model je vytvořený z veřejné datové sady, obsahující data snímačů reálného motoru. Další informace o způsobu vytvoření modelu z původní sady dat najdete v tématu [Šablona prediktivní údržby v galerii Cortana Intelligence][lnk-cortana-analytics].

Simulovaná zařízení mohou zpracovávat následující příkazy, odeslané ze služby IoT Hub:

| Příkaz | Popis |
| --- | --- |
| StartTelemetry |Řídí stav simulace.<br/>Spustí odesílání telemetrických dat ze zařízení |
| StopTelemetry |Řídí stav simulace.<br/>Zastaví odesílání telemetrických dat ze zařízení |

Služba IoT Hub zajišťuje potvrzení příkazu zařízení.

## <a name="azure-stream-analytics-job"></a>Úlohy služby Azure Stream Analytics
**Úloha: Telemetrie** funguje v příchozím datovém proudu telemetrických dat ze zařízení pomocí dvou příkazů. První vybere všechny telemetrická ze zařízení a odešle tato data do úložiště objektu blob, kde jsou vizualizována ve webové aplikaci. Druhý příkaz vypočítá průměrné hodnoty čidel v rámci dvouminutového posuvného okna a odešle je prostřednictvím centra událostí do **procesoru událostí**.

## <a name="event-processor"></a>Procesor událostí
**Procesor událostí** přebírá průměrné hodnoty snímačů za dokončený cyklus. Potom tyto hodnoty předá do rozhraní API, které nabídne trénovaný model Machine Learning pro výpočet zbývající doby životnosti motoru.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Další informace o způsobu vytvoření modelu z původní sady dat najdete v tématu [Šablona prediktivní údržby v galerii Cortana Intelligence][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Začínáme
Tato část vás provede součástmi řešení, popíše zamýšlený případ použití a poskytne vám příklady.

### <a name="predictive-maintenance-dashboard"></a>Řídicí panel prediktivní údržby
Tato stránka ve webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz [Úložiště vizuálních prvků PowerBI][lnk-powerbi]) k vizualizaci:

* výstupních dat úlohy služby Stream Analytics v úložišti objektů blob
* zbývající doby životnosti (RUL) a počtu cyklů pro každý motor letadla

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Sledování chování cloudového řešení
Na webu Azure Portal přejděte do skupiny prostředků s názvem řešení, které jste si vybrali k zobrazení zřízených prostředků.

![][img-resource-group]

Při zřizování předkonfigurovaného řešení obdržíte e-mail s odkazem na pracovní prostor Machine Learning. Do pracovního prostoru Machine Learning se můžete dostat také ze stránky zřízeného řešení na webu [azureiotsuite.com][lnk-azureiotsuite] v případě, že je řešení ve stavu **Připraveno**.

![][img-machine-learning]

Na portálu řešení uvidíte, že ve vzorovém řešení jsou čtyři simulovaná zařízení, která představují dvě letadla se dvěma motory na každé letadlo, z nichž každý má čtyři snímače. Při první návštěvě portálu řešení dojde k zastavení simulace.

![][img-simulation-stopped]

Kliknutím na **Spustit simulaci** zahájíte simulaci, ve které uvidíte na řídicím panelu historii hodnot snímačů, zbývající dobu životnosti (RUL), počet cyklů a historii hodnot RUL.

![][img-simulation-running]

Když je zbývající doba životnosti (RUL) nižší než 160 (libovolná hodnota, zvolená pro demonstrační účely), portál řešení zobrazí vedle hodnoty RUL symbol upozornění a zvýrazní daný motor letadla žlutou barvou. Můžete si všimnout, že hodnoty zbývající doby životnosti (RUL) mají obecné klesající trend, ale kolísají nahoru a dolů. Toto chování vyplývá z různých délek cyklu a přesnosti modelu.

![][img-simulation-warning]

Při úplné simulaci trvá dokončení 148 cyklů asi 35 minut. Prahová hodnota 160 pro zbývající dobu životnosti (RUL) je poprvé dosažena přibližně po 5 minutách simulace a oba motory se na tuto hodnotu dostanou přibližně po 8 minutách.

Simulace zpracuje úplnou datovou sadu s údaji o 148 cyklech a vytvoří konečnou hodnotu RUL a cyklů.

Simulaci lze zastavit v libovolný okamžik, ale kliknutím na tlačítko **Start simulace** spustíte simulaci znovu od začátku datové sady.

## <a name="next-steps"></a>Další kroky
Když jste si vyzkoušeli předkonfigurované řešení prediktivní údržby, možná byste je chtěli upravit – pro tyto účely najdete informace v tématu [Pokyny k přizpůsobení předkonfigurovaných řešení][lnk-customize].

V příspěvku [IoT Suite - Pod kapotou - Prediktivní údržba](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) na blogu TechNet naleznete další podrobnosti o předkonfigurovaném řešení prediktivní údržby.

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT od počátku][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Dec16_HO1-->


