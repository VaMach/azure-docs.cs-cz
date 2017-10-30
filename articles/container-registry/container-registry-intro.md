---
title: "Soukromé registry kontejnerů Dockeru v Azure"
description: "Úvod do služby Azure Container Registry poskytující cloudové, spravované, privátní registry Dockeru."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: mmacy
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: stevelas
ms.openlocfilehash: d54caa45078221cdbe091649cb3fe3b65eaa47e8
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2017
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Seznámení se soukromými registry kontejnerů Dockeru v Azure

Azure Container Registry je spravovaná služba [registrů Dockeru](https://docs.docker.com/registry/) založená na opensourcovém nástroji Docker Registry 2.0. Vytvořte a udržujte registry kontejnerů Azure za účelem ukládání a správy privátních imagí [kontejneru Dockeru](https://www.docker.com/what-docker). Použijte registry kontejnerů v Azure se svými stávajícími kanály vývoje a nasazení kontejnerů a čerpejte z odborných znalostí komunity Dockeru.

Související informace o Dockeru a kontejnerech najdete v [uživatelská příručce Dockeru](https://docs.docker.com/engine/userguide/).

## <a name="use-cases"></a>Případy použití
Stažení imagí z registru kontejnerů Azure na různé cíle nasazení:

* **Škálovatelné systémy orchestrace**, které spravují kontejnerizované aplikace napříč clustery hostitelů, včetně [DC/OS](https://docs.mesosphere.com/), [Dockeru Swarm](https://docs.docker.com/swarm/) a [Kubernetes](http://kubernetes.io/docs/).
* **Služby Azure**, které podporují vytváření a spouštění škálovaných aplikací, včetně [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) a dalších.

Vývojáři mohou nahrávat do registru kontejnerů také jako součást pracovního postupu vývoje kontejneru. Mohou například určit registr kontejnerů jako cíl v nástroji pro nasazení a nástroji průběžné integrace, jako je například [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) nebo [Jenkins](https://jenkins.io/).

## <a name="key-concepts"></a>Klíčové koncepty
* **Registr** – Vytvořte jeden nebo více registrů kontejnerů ve svém předplatném Azure. Registry jsou dostupné ve třech skladových položkách: [Basic, Standard a Premium](container-registry-skus.md). Každá z nich podporuje integraci webhooků, ověřování úložiště pomocí Azure Active Directory a funkci odstraňování. Využijte místní úložiště imagí kontejnerů v síťové blízkosti vytvořením registru ve stejném umístění Azure, jako jsou vaše nasazení. Funkci [geografické replikace](container-registry-geo-replication.md) v registrech úrovně Premium můžete využít ve scénářích pokročilé replikace a distribuce imagí kontejnerů. Plně kvalifikovaný název registru má formát `myregistry.azurecr.io`.

  [Přístup k registru kontejnerů řídíte](container-registry-authentication.md) pomocí [instančního objektu](../active-directory/active-directory-application-objects.md) zajištěného službou Azure Active Directory nebo poskytnutého účtu správce. Spusťte standardní příkaz `docker login` k ověření pomocí registru.

* **Úložiště** – Registr obsahuje jedno nebo několik úložišť, což jsou skupiny imagí kontejnerů. Azure Container Registry podporuje víceúrovňové obory názvů úložiště. S víceúrovňovými obory názvů můžete seskupovat kolekce imagí souvisejících s konkrétní aplikací nebo kolekcí aplikací pro konkrétní vývojové nebo provozní týmy. Například:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` představuje image pro celý podnik.
  * `myregistry.azurecr.io/warrantydept/dotnet-build` představuje image sloužící k vytváření aplikací .NET, která je sdílená v rámci záručního oddělení.
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` představuje webovou image seskupenou v zákaznické aplikaci pro odesílání a vlastněnou záručním oddělením.

* **Image** – Každá image uložená v úložišti je snímkem kontejneru Dockeru jen pro čtení. Registry kontejnerů Azure mohou zahrnovat image systémů Windows i Linux. Názvy imagí pro všechna nasazení kontejnerů určujete vy. Pomocí standardních [příkazů Dockeru](https://docs.docker.com/engine/reference/commandline/) můžete nahrávat image do úložiště nebo si z úložiště image stáhnout.

* **Kontejner** – Kontejner definuje softwarovou aplikaci a její závislosti zabalené do kompletního systému souborů, včetně kódu, modulu runtime, systémových nástrojů a knihoven. Spouštějte kontejnery Dockeru na základě imagí systémů Windows nebo Linux, které si stáhnete z registru kontejnerů. Kontejnery běžící na jednom počítači sdílí jádro operačního systému. Kontejnery Dockeru jsou plně přenositelné na všechny hlavní distribuce Linuxu, macOS i Windows.

## <a name="next-steps"></a>Další kroky
* [Vytvoření registru kontejnerů pomocí webu Azure Portal](container-registry-get-started-portal.md)
* [Vytvoření registru kontejnerů pomocí Azure CLI](container-registry-get-started-azure-cli.md)
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)
