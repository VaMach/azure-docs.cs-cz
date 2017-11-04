---
title: "Přehled Service Fabric v Azure | Microsoft Docs"
description: "Přehled Service Fabric, kde se aplikace skládá z mnoha mikroslužeb zajistit škálování a odolnost. Service Fabric je distribuovaných systémů platformy sloužící k vytvoření spolehlivé, škálovatelné a snadno spravovat aplikace pro cloud."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: msfussell
ms.custom: mvc
ms.openlocfilehash: 8ff0d38a679b673b148dd808050eda82060cfe80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-service-fabric"></a>Přehled Azure Service Fabric
Azure Service Fabric je platforma distribuovaných systémů usnadňující balení, nasazování a spravování škálovatelných a spolehlivých mikroslužeb a kontejnerů. Service Fabric také řeší významné problémy ve vývoji a správě cloudu nativních aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné. Service Fabric představuje generace platforma pro vytváření a správě těchto podnikových, úrovně 1, cloudové škálování aplikace běžící v kontejnerech.

Následující krátké video představuje Service Fabric a mikroslužeb:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Aplikace se skládá z mikroslužeb 
Service Fabric umožňuje vytvářet a spravovat škálovatelného a spolehlivého aplikace skládá z mikroslužeb, které jsou spuštěné v s vysokou hustotou na sdílenému fondu počítače, který se označuje jako cluster. Poskytuje sofistikované, lightweight runtime vytvářet distribuované, škálovatelné a bezstavové a stavové mikroslužeb spuštěna v kontejnerech. Také poskytuje funkce pro správu aplikace komplexní zřizování, nasazení, monitorování, upgrade nebo opravu a odstranění nasazené aplikace, včetně kontejnerizované služeb.

Service Fabric pohání řady služeb Microsoft services dnes, včetně databáze SQL Azure, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype pro firmy a mnoho základní služby Azure.

Service Fabric je přizpůsobit pro vytvoření cloudových nativní služby, které můžete spustit malé, podle potřeby a dosáhnout masivním měřítku se stovkami nebo tisíci počítačů.

Dnešní internetových služeb jsou vytvářeny ze mikroslužeb. Příklady mikroslužeb zahrnovat brány protokolu, uživatelských profilů, nákupních košíků, inventáře, zpracování, fronty a ukládá do mezipaměti. Service Fabric je mikroslužeb platforma, která poskytuje jedinečný název, který může být bezstavové nebo stavová každých mikroslužbu (nebo kontejneru).

Service Fabric nabízí komplexní runtime a životního cyklu správy funkce pro aplikace, které se skládají z těchto mikroslužeb. Počítač hostuje mikroslužeb uvnitř kontejnery, které jsou v clusteru Service Fabric aktivována a nasazena. Přesun z virtuálních počítačů do kontejnerů umožňuje pořadí velikosti zvýšení hustoty. Podobně bude možno jiné jednotkách o řád v hustotu, když přesouváte z kontejnerů mikroslužeb v těchto kontejnerech. Například jeden cluster pro Azure SQL Database zahrnuje stovky počítačů s desítkami tisíc kontejnery tohoto hostitele celkem stovky tisíc databáze. Každá databáze je stavová mikroslužbu Service Fabric. 

Další informace o mikroslužeb přístup pro čtení [proto mikroslužeb přístup k sestavení aplikací?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Nasazení kontejneru a Orchestrace
Service Fabric je společnosti Microsoft [kontejneru orchestrator](service-fabric-cluster-resource-manager-introduction.md) nasazení mikroslužeb mezi clustery počítačů. Mikroslužeb mohou být vytvořeny v mnoha směrech pomocí [Service Fabric programovací modely](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)do nasazení [žádný kód podle svého výběru](service-fabric-deploy-existing-app.md). Je důležité je možné kombinovat jak služby v procesy a služby v kontejnerech ve stejné aplikaci. Pokud chcete [nasazení a Správa kontejnerů](service-fabric-containers-overview.md), Service Fabric je ideální volbou jako kontejner orchestrator.

## <a name="any-os-any-cloud"></a>Všechny operační systém, všechny cloudu
Service Fabric se spustí everywhere. Můžete vytvořit clustery pro Service Fabric do mnoha prostředí, včetně Azure nebo místní, Windows Server nebo Linux. Clustery můžete vytvořit i na jiné veřejné cloudy. Kromě toho je vývojového prostředí v sadě SDK **identické** do produkčního prostředí s žádné emulátorů související se situací. Jinými slovy co běží na místní vývojový cluster nasadí do clusterů v jiných prostředích.

![Platforma Service Fabric][Image1]

Pro vývoj pro Windows je sada Service Fabric .NET SDK integrovaná do sady Visual Studio a prostředí Powershell. V tématu [Příprava vývojového prostředí v systému Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md). Pro Linux vývoj Service Fabric Java SDK je integrovaná s Eclipse a Yeoman slouží ke generování šablon pro Java, .NET Core a aplikace typu kontejner. V tématu [Příprava vývojového prostředí v systému Linux](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md)

Další informace o vytváření clusterů, najdete v tématu [vytvoření clusteru v systému Windows Server nebo Linux](service-fabric-deploy-anywhere.md) nebo pro vytvoření clusteru s podporou Azure [prostřednictvím portálu Azure](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Bezstavové a stavové mikroslužeb pro Service Fabric
Service Fabric umožňuje vytvářet aplikace, které se skládají z mikroslužeb nebo kontejnerů. Bezstavové mikroslužeb (například protokol brány a webové proxy servery) neudržují měnitelný stav mimo požadavek a odpověď ze služby. Role pracovního procesu Azure Cloud Services jsou příkladem bezstavové služby. Stavová mikroslužeb (například uživatelské účty, databáze, zařízení, nákupní košíků a fronty) udržovat měnitelný, autoritativní stavu kromě požadavku a odpovědi. Dnešní internetových aplikací se skládají z kombinace bezstavové a stavové mikroslužeb. 

Klíče rozdílů mezi s Service Fabric je silné zaměřuje na budování stavové služby, buď pomocí [předdefinované programovací modely ](service-fabric-choose-framework.md) nebo s kontejnerizované stavové služby. [Scénáře aplikací](service-fabric-application-scenarios.md) se popisují scénáře, kdy se používá stavové služby.


## <a name="application-lifecycle-management"></a>Správa životního cyklu aplikací
Service Fabric poskytuje podporu pro celou aplikaci životního cyklu a CI/CD cloudových aplikací, včetně kontejnery. Zahrnuje tohoto životního cyklu vývoje po nasazení, každodenní správu a údržbu na případné vyřazení z provozu.

Možnosti správy životního cyklu aplikace Service Fabric povolit správci aplikací a operátory IT pomocí pracovních postupů jednoduchý, nízká touch zřídit, nasazení, opravy a monitorování aplikací. Tyto pracovní postupy integrované výrazně snížit zatížení IT operátory zachovat aplikace, které jsou nepřetržitě k dispozici.

Většina aplikací se skládá z kombinace bezstavové a stavové mikroslužeb, kontejnery a další spustitelné soubory, které jsou nasazeny společně. Service Fabric tak, že silné typy na aplikace, umožňuje nasazení více instancí aplikace. Každá instance je spravovat a upgradovat nezávisle. Service Fabric je nejdůležitější, můžete nasadit kontejnery nebo všech spustitelných souborů a jejich spolehlivost. Například Service Fabric můžete nasadit .NET, ASP.NET Core, node.js, Windows kontejnery, Linux kontejnery, Java virtuálních počítačů, skripty, úhlová nebo oznámena všechno, co tvoří vaší aplikace.

Service Fabric je integrována CI/CD Nástroje, jako [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [volaných](https://jenkins.io/index.html), a [Chobotnice nasazení](https://octopus.com/) a lze použít s jakýkoli jiný nástroj oblíbených položek konfigurace nebo CD.

Další informace o správě životního cyklu aplikací, přečtěte si [životního cyklu aplikace](service-fabric-application-lifecycle.md). Další informace o tom, jak nasadit kód, najdete v tématu [nasazení spustitelný soubor hosta](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Klíčové funkce
Pomocí Service Fabric, můžete:

* Nasaďte do Azure nebo do místních datových center, která spustit Windows nebo Linux s nulové změnami kódu. Jeden zápis a pak nasadit kamkoli do žádného clusteru Service Fabric.
* Škálovatelné aplikace, které se skládají z mikroslužeb vyvíjet pomocí Service Fabric programovací modely, kontejnery nebo žádný kód.
* Vytvořte vysoce spolehlivé bezstavové a stavové mikroslužeb. Návrh aplikace Zjednodušte pomocí stavová mikroslužeb. 
* Pomocí nové programovacího modelu Reliable Actors vytvářet objekty cloudu se samoobslužné obsažené kód a stavu.
* Nasazení a orchestraci kontejnerů, které zahrnují kontejnery Windows a Linux kontejnery. Service Fabric je vědět, stavová, kontejner orchestrator data.
* Nasazení aplikací v sekundách, se stovkami nebo tisíci aplikací nebo kontejnery na počítač s vysokou hustotou.
* Nasadit různé verze stejné aplikace vedle sebe a každou aplikaci upgradovat nezávisle.
* Správa životního cyklu aplikací bez odstávky, včetně dopadem na dřívější kód a pevných upgrady.
* Horizontální navýšení kapacity nebo škálování počtu uzlů v clusteru. Při změně měřítka uzly, vaše aplikace automaticky škálovat.
* Monitorování a Diagnostika stavu aplikace a nastavení zásad pro provedení automatické opravy.
* Kukátko resource balancer orchestraci opětovnou distribuci aplikací napříč clusterem. Service Fabric obnoví selhání a optimalizuje distribuci zatížení na základě dostupných prostředků.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
* Další informace:
  * [Proč mikroslužeb přístupu k sestavení aplikací?](service-fabric-overview-microservices.md)
  * [Přehled terminologie](service-fabric-technical-overview.md)
* Nastavení vaší [vývojového prostředí systému Windows](service-fabric-get-started.md)  
* Nastavení vaší [Linux vývojového prostředí](service-fabric-get-started-linux.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
