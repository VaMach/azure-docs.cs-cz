---
title: "Azure IoT Suite – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy k sadě IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2017
ms.author: dobett
ms.openlocfilehash: 63b059cb91956231fd3bafbe9770a04a0b7e347a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Nejčastější dotazy k sadě IoT Suite

Viz také připojené konkrétní objekt pro vytváření [– nejčastější dotazy](iot-suite-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Kde najdu zdrojový kód pro předkonfigurované řešení?

Zdrojový kód je uložena v následující úložišť GitHub:

* [Vzdálené monitorování předkonfigurované řešení (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Vzdálené monitorování předkonfigurované řešení (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Předkonfigurované řešení prediktivní údržby](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Připojené objekt pro vytváření předkonfigurovaného řešení](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Kolik stojí zřídit nové řešení vzdáleného monitorování?

Nové předkonfigurované řešení nabízí dvě možnosti nasazení:

* A *základní* možnost určená pro vývojáře, kteří hledají nižší náklady na vývoj nebo zákazníci chtějí vytvořit ukázku nebo testování konceptu.
* A *standardní* možnost určený pro podniky chtějí nasadit infrastrukturu produkční prostředí.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Jak můžete zajistit, že I snížení Moje nákladů při I vyvíjet mém řešení?

Kromě dvě odlišné nasazení, nové řešení vzdáleného monitorování má nastavení, které povolí nebo zakáže všechna Simulovaná zařízení na vyžádání. Zakázání simulaci snižuje požity v řešení a proto celkové náklady na data.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>Nové architektury mikroslužeb je k dispozici pro všechny tři předkonfigurovaná řešení?

V současné době pouze řešení vzdáleného monitorování používá architekturu mikroslužeb jako pokrývá nejširší scénář.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>V nové aktualizace jaké výhody poskytuje nové open source na základě mikroslužeb architektury?

Za poslední dva roky se výrazně vyvinuly cloudové architektury. Malých služby jako skvělé vzor k dosažení škálování a flexibilitu, aniž by došlo ke ztrátě vývoj rychlostí této služby. Tento vzor architektury se používá v několika služeb Microsoft interně s skvělé spolehlivosti a škálovatelnosti výsledky. Jsme jsou uvedení těchto učení v praxi, aby naše zákazníky těžit z nich.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>Je k dispozici nové předkonfigurované řešení ve stejné zeměpisné oblasti jako existující řešení?

Ano, nové vzdálené monitorování je dostupný ve stejné zeměpisné oblasti.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Jaký je rozdíl mezi možnostmi úrovně basic a standard nasazení? Jak se rozhodnout mezi možnostmi dvě nasazení?

Jednotlivé možnosti nasazení odpoví na různé potřeby. Základní nasazení slouží k zahájení práce a vývoj, testování koncepce a rozsáhlá pilotní nasazení. Nabízí efektivní architektura s minimální potřebné prostředky a nižší náklady. Standardní nasazení slouží k vytvoření a přizpůsobení řešení pro produkční prostředí a poskytuje nasazení s nezbytné elementy pro Pamatujte si, že. Spolehlivost a škálování aplikace mikroslužeb jsou vytvořené jako kontejnery Docker a nasazují pomocí orchestrator (Kubernetes ve výchozím nastavení). Orchestrator je zodpovědná za nasazení, škálování a správu aplikace. Měli byste vybrat možnost podle aktuální potřeby. Můžete použít jednu, dalších nebo kombinace obojího, v závislosti na vaší fázi projektu.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Můžete nadále využívat mé existující investice do Azure IoT Suite?

Ano. Žádné řešení, které existuje ještě dnes nadále fungovat ve vašem předplatném Azure a zdrojový kód zůstává k dispozici na webu GitHub.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Jaký je rozdíl mezi odstranění skupiny prostředků na portálu Azure a kliknutím na Odstranit v předkonfigurovaném řešení na stránkách azureiotsuite.com?

* Pokud odstraníte předkonfigurované řešení na [azureiotsuite.com](https://www.azureiotsuite.com/), odstranit všechny prostředky, které byly zřízeny při vytváření předkonfigurovaného řešení. Pokud jste do skupiny prostředků přidali další prostředky, tyto prostředky budou také odstraněny.
* Pokud odstraníte skupinu prostředků [portál Azure](https://portal.azure.com), se odstraní pouze prostředky v příslušné skupině prostředků. Budete také muset odstranit aplikaci Azure Active Directory, které jsou spojené s předkonfigurovaným řešením.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Kolik instancí služby IoT Hub můžete zřídit v jednom předplatném?

Ve výchozím nastavení můžete zřídit [10 centra IoT na jedno předplatné](../azure-subscription-service-limits.md#iot-hub-limits). Můžete vytvořit [lístek podpory Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) chcete tento limit zvýšit. V důsledku toho vzhledem k tomu, že každé předkonfigurované řešení zřídí novou službu IoT Hub, můžete zřídit až 10 předkonfigurovaných řešení v daném předplatném.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Kolik instancí Azure Cosmos DB můžete zřídit v jednom předplatném?

Padesát. Můžete vytvořit [lístek podpory Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) chcete tento limit zvýšit, ale ve výchozím nastavení, můžete zřídit 50 instancí Cosmos DB za předplatné.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Kolik bezplatných rozhraní API Map Bing můžu zřídit v jednom předplatném?

Dvě. Můžete vytvořit pouze dvě vnitřní transakce úroveň 1 mapy Bing pro podnikových plánů v předplatné Azure. Ve výchozím nastavení s plánem vnitřní transakce úrovně 1 se zřídí řešení vzdáleného monitorování. V důsledku toho můžete v daném předplatném zřídit nanejvýš dvě předkonfigurovaná řešení vzdáleného monitorování bez možnosti úprav.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Můžu vytvořit předkonfigurované řešení, když mám Microsoft Azure pro DreamSpark?

V současné době nelze vytvořit předkonfigurované řešení s [Microsoft Azure pro DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99) účtu. Ale můžete vytvořit [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) si během několika minut, který vám umožní vytvořit předkonfigurované řešení.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Můžete vytvořit předkonfigurované řešení, pokud mám předplatné Cloud Solution Provider (CSP)?

V současné době nelze vytvořit předkonfigurované řešení s předplatným Cloud Solution Provider (CSP). Ale můžete vytvořit [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) si během několika minut, který vám umožní vytvořit předkonfigurované řešení.

### <a name="how-do-i-delete-an-aad-tenant"></a>Jak se odstraním klienta AAD?

Najdete v příspěvku blogu od Erica Golpeho [návod odstranění klient služby Azure AD](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Další kroky

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby](iot-suite-predictive-overview.md)
* [Přehled připojené objekt pro vytváření předkonfigurovaného řešení](iot-suite-connected-factory-overview.md)
* [Zabezpečení IoT od základů](securing-iot-ground-up.md)