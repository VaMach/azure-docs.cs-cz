---
title: "Úvod do služby Azure Container Registry | Dokumentace Microsoftu"
description: "Úvod do služby Azure Container Registry poskytující cloudové, spravované, privátní registry Dockeru."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: 806ffa8ce0c434d2a6cc1c7ec3a6e89be9145679
ms.openlocfilehash: 6661766d71d56e49b8840aeb250c105e132b9b04

---
# <a name="what-is-azure-container-registry"></a>Co je Azure Container Registry?
> [!NOTE]
> Container Registry je aktuálně ve verzi Preview.


Azure Container Registry je spravovaná služba [registrů Dockeru](https://docs.docker.com/registry/) založená na open source nástroji Docker Registry v2. Vytvořte a udržujte registry kontejnerů Azure za účelem ukládání a správy privátních imagí [kontejneru Dockeru](https://www.docker.com/what-docker). Použijte registry kontejnerů v Azure se svými stávajícími kanály vývoje a nasazení kontejnerů a čerpejte z odborných znalostí komunity Dockeru.

Související informace o Dockeru a kontejnerech najdete v tématech:

* [Uživatelská příručka Dockeru](https://docs.docker.com/engine/userguide/)
* [Oznámení verze Preview služby Azure Container Registry](https://azure.microsoft.com/blog/azure-container-registry-preview/) 

## <a name="key-concepts"></a>Klíčové koncepty
* **Registr** – Vytvořte jeden nebo více registrů kontejnerů ve svém předplatném Azure. Každý registr je zajišťován standardním [účtem úložiště](../storage/storage-introduction.md) Azure ve stejném umístění. Vytvořte registr ve stejném umístění Azure jako vaše nasazení a využijte místní úložiště imagí kontejnerů v síťové blízkosti. 

  Registry jsou pojmenované v kořenové doméně podle [tenanta Azure Active Directory](../active-directory/active-directory-howto-tenant.md) daného předplatného. Pokud například máte účet organizace v doméně Contoso, plně kvalifikovaný název registru bude mít podobu `myregistry-contoso.azurecr.io`. 
  
  [Přístup k registru kontejnerů řídíte](container-registry-authentication.md) pomocí [instančního objektu](../active-directory/active-directory-application-objects.md) zajištěného službou Azure Active Directory nebo poskytnutého účtu správce. Spusťte standardní příkaz `docker login` k ověření pomocí registru. 

* **Úložiště** – Registr obsahuje jedno nebo několik úložišť, což jsou skupiny imagí kontejnerů. Azure Container Registry podporuje víceúrovňové obory názvů úložiště. Tato funkce umožňuje seskupovat kolekce imagí souvisejících s konkrétní aplikací nebo kolekcí aplikací pro konkrétní vývojové nebo provozní týmy. Například:
  
  * `myregistry-contoso.azurecr.io/aspnetcore:1.0.1` představuje image pro celý podnik.
  * `myregistry-contoso.azurecr.io/warrantydept/dotnet-build` představuje image sloužící k vytváření aplikací .NET, která je sdílená v rámci záručního oddělení.
  * `myregistry-contoso.azrecr.io/warrantydept/customersubmissions/web` představuje webovou image seskupenou v zákaznické aplikaci pro odesílání a vlastněnou záručním oddělením.

* **Image** – Každá image uložená v úložišti je snímkem kontejneru Dockeru jen pro čtení. Registry kontejnerů Azure mohou zahrnovat image systémů Windows i Linux. Názvy imagí pro všechna nasazení kontejnerů určujete vy. Pomocí standardních [příkazů Dockeru](https://docs.docker.com/engine/reference/commandline/) můžete nahrávat image do úložiště nebo si z úložiště image stáhnout. 

* **Kontejner** – Kontejner definuje softwarovou aplikaci a její závislosti zabalené do kompletního systému souborů, včetně kódu, modulu runtime, systémových nástrojů a knihoven. Spouštějte kontejnery Dockeru na základě imagí systémů Windows nebo Linux, které si stáhnete z registru kontejnerů. Kontejnery běžící na jednom počítači sdílí jádro operačního systému. Kontejnery Dockeru jsou plně přenositelné na všechny hlavní distribuce Linuxu, Mac i Windows.

## <a name="use-cases"></a>Případy použití
Stažení imagí z registru kontejnerů Azure na různé cíle nasazení:

* **Škálovatelné systémy orchestrace**, které spravují kontejnerizované aplikace napříč clustery hostitelů, včetně [DC/OS](https://docs.mesosphere.com/), [Dockeru Swarm](https://docs.docker.com/swarm/) a [Kubernetes](http://kubernetes.io/docs/).
* **Služby Azure**, které podporují vytváření a spouštění škálovaných aplikací, včetně [Container Service](../container-service/index.md), [App Service](/app-service/index.md), [Batch](../batch/index.md) a [Service Fabric](../service-fabric/index.md). 

Vývojáři mohou nahrávat do registru kontejnerů také jako součást pracovního postupu vývoje kontejneru. Mohou například určit registr kontejnerů jako cíl v nástroji pro nasazení a nástroji průběžné integrace, jako je například [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) nebo [Jenkins](https://jenkins.io/).





## <a name="next-steps"></a>Další kroky
* [Vytvoření registru kontejnerů pomocí webu Azure Portal](container-registry-get-started-portal.md)
* [Vytvoření registru kontejnerů pomocí Azure CLI](container-registry-get-started-azure-cli.md)
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)
* Informace o sestavení pracovního postupu průběžné integrace a nasazení pomocí Visual Studio Team Services, Azure Container Service a Azure Container Registry, najdete v [tomto kurzu](../container-service/container-service-setup-ci-cd.md).
* Pokud chcete v Azure nastavit vlastní privátní registr Dockeru (bez veřejného koncového bodu), přečtěte si téma [Nasazení vlastního privátního registru Dockeru v Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).



<!--HONumber=Nov16_HO4-->


