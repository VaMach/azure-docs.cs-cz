---
title: "Další kroky pro vytvoření projektu Service Fabric | Microsoft Docs"
description: "Tento článek obsahuje odkazy na sadu úloh vývoj jádra pro Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2017
ms.author: rwike77
ms.openlocfilehash: 4523c63493bc9cb3f96713c4abbc1dd1da9130d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Vaše aplikace Service Fabric a další kroky
Vaše aplikace Azure Service Fabric byla vytvořena. Tento článek popisuje způsob vytvoření projektu a některé potenciální další kroky.

## <a name="your-application"></a>Vaše aplikace
Každé nové aplikace zahrnuje projekt aplikace. Může mít jeden nebo dva další projekty, v závislosti na typu služby vybrali.

### <a name="the-application-project"></a>Projekt aplikace
Projekt aplikace se skládá z:

* Sada odkazů na služby, které tvoří vaši aplikaci.
* Tři publikační profily (1uzlu místní, 5uzlu místní a cloudové), které můžete použít ke správě předvoleb pro práci s různých prostředích – například předvolby týkající se koncový bod clusteru a zda k provedení upgradu nasazení ve výchozím nastavení.
* Soubory tři parametr aplikace (stejné jako výše), můžete použít ke správě konfigurace specifické pro prostředí aplikace, například počet oddílů pro vytvoření pro službu.
* Skript nasazení, který můžete použít k nasazení aplikace z příkazového řádku nebo jako součást automatizované průběžnou integraci a nasazení kanálu.
* Manifest aplikace, která popisuje aplikace. Ve složce ApplicationPackageRoot můžete najít v manifestu.

### <a name="stateless-service"></a>Bezstavové služby
Při přidání nové bezstavové služby Visual Studio přidá do vašeho řešení, které zahrnuje typ následníky projektu služby `StatelessService`. Služba zvýší místní proměnné v čítače.

### <a name="stateful-service"></a>Stavové služby
Když přidáte novou stavové služby, Visual Studio přidá do vašeho řešení, které zahrnuje typ následníky projektu služby `StatefulService`. Služba zvýší čítače v jeho `RunAsync` metoda a ukládá výsledky v `ReliableDictionary`.

### <a name="actor-service"></a>Služba objektu actor
Při přidání nového objektu actor spolehlivé, Visual Studio přidá do vašeho řešení dva projekty: objektu actor projekt a projekt rozhraní.

Poskytuje metody pro nastavení projektu objektu actor a získávání hodnotu počítadla, která je spolehlivě uloženy v rámci objektu actor stavu. Rozhraní projektu poskytuje rozhraní, které má být vyvolán objektu actor můžete použít jiné služby.

### <a name="stateless-web-api"></a>Bezstavové webového rozhraní API
Bezstavové projekt webového rozhraní API poskytuje základní webová služba, která slouží k otevření aplikace externím klientům. Další informace o projektu strukturovaná, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>Jádro ASP.NET
Sada Service Fabric SDK obsahuje stejnou sadu ASP.NET Core šablony, které jsou k dispozici pro samostatné projektů ASP.NET Core: prázdná, [webového rozhraní API][aspnet-webapi], a [webové aplikace][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Spustitelné soubory a hostů kontejnery hosta

Service Fabric, guest, je služba, která není vytvořené s nástroji programovací modely platformu. Binární soubory pro Host můžete balíček buď [přímo v balíčku aplikace](service-fabric-deploy-existing-app.md) nebo [prostřednictvím bitovou kopii kontejneru](service-fabric-deploy-container.md). V obou případech Visual Studio vytvoří artefakty potřebné v **ApplicationPackageRoot** složku projekt aplikace. Visual Studio nebude vytvořit nový projekt služby, protože kód již existuje jinde. Pokud chcete ke správě vašich projektů hosta spolu s projekt aplikace Service Fabric, můžete je přidat do stejného řešení, Visual Studio.

## <a name="next-steps"></a>Další kroky
### <a name="create-an-azure-cluster"></a>Vytvoření clusteru služby Azure
Sada Service Fabric SDK poskytuje místní cluster pro vývoj a testování. Pokud chcete vytvořit cluster v Azure, najdete v části [nastavení cluster Service Fabric na portálu Azure][create-cluster-in-portal].

### <a name="publish-your-application-to-azure"></a>Publikování aplikace do Azure
Můžete publikovat svoji aplikaci přímo ze sady Visual Studio do clusteru služby Azure. Další informace, jak zjistit, [publikování aplikace do Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Vizualizujte cluster pomocí Service Fabric Exploreru
Service Fabric Explorer nabízí snadný způsob, jak vizualizace clusteru, včetně nasazené aplikace a fyzické rozložení. Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verze a upgradujte vašim službám
Service Fabric umožňuje nezávislé Správa verzí a upgrade nezávislých služeb v aplikaci. Další informace najdete v tématu [Správa verzí a upgrade vašich služeb][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurace průběžnou integraci s Visual Studio Team Services
Informace o tom, jak můžete nastavit průběžnou integraci proces pro vaši aplikaci Service Fabric, najdete v části [nakonfigurovat průběžnou integraci s Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
