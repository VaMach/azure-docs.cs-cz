---
title: "Registry kontejnerů pro Private Docker v Azure | Dokumentace Microsoftu"
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
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: f9dff5384838521c309a2d2a5ebb5376c90159fb
ms.contentlocale: cs-cz
ms.lasthandoff: 07/12/2017

---
# <a name="introduction-to-private-docker-container-registries"></a>Úvod k registrům kontejnerů pro Private Docker


Azure Container Registry je spravovaná služba [registrů Dockeru](https://docs.docker.com/registry/) založená na opensourcovém nástroji Docker Registry 2.0. Vytvořte a udržujte registry kontejnerů Azure za účelem ukládání a správy privátních imagí [kontejneru Dockeru](https://www.docker.com/what-docker). Použijte registry kontejnerů v Azure se svými stávajícími kanály vývoje a nasazení kontejnerů a čerpejte z odborných znalostí komunity Dockeru.

Související informace o Dockeru a kontejnerech najdete v tématech:

* [Uživatelská příručka Dockeru](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Případy použití
Stažení imagí z registru kontejnerů Azure na různé cíle nasazení:

* **Škálovatelné systémy orchestrace**, které spravují kontejnerizované aplikace napříč clustery hostitelů, včetně [DC/OS](https://docs.mesosphere.com/), [Dockeru Swarm](https://docs.docker.com/swarm/) a [Kubernetes](http://kubernetes.io/docs/).
* **Služby Azure**, které podporují vytváření a spouštění škálovaných aplikací, včetně [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](../service-fabric/index.md) a dalších.

Vývojáři mohou nahrávat do registru kontejnerů také jako součást pracovního postupu vývoje kontejneru. Mohou například určit registr kontejnerů jako cíl v nástroji pro nasazení a nástroji průběžné integrace, jako je například [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) nebo [Jenkins](https://jenkins.io/).





## <a name="key-concepts"></a>Klíčové koncepty
* **Registr** – Vytvořte jeden nebo více registrů kontejnerů ve svém předplatném Azure. Každý registr je zajišťován standardním [účtem úložiště](../storage/storage-introduction.md) Azure ve stejném umístění. Využijte místní úložiště imagí kontejnerů v síťové blízkosti vytvořením registru ve stejném umístění Azure, jako jsou vaše nasazení. Plně kvalifikovaný název registru má formát `myregistry.azurecr.io`.

  [Přístup k registru kontejnerů řídíte](container-registry-authentication.md) pomocí [instančního objektu](../active-directory/active-directory-application-objects.md) zajištěného službou Azure Active Directory nebo poskytnutého účtu správce. Spusťte standardní příkaz `docker login` k ověření pomocí registru.

* **Spravovaný registr** – Vrstva, která nabízí další možnosti pro registry ve třech skladových položkách (SKU) – Basic, Standard a Premium. Image těchto SKU se ukládají v účtech služby Storage spravovaných službou Azure Container Registry, což zvyšuje spolehlivost a umožňuje nové funkce. Nové možnosti zahrnují integraci webhooků, ověřování úložiště pomocí Azure Active Directory a podporu funkce odstraňování. Uživatelé mají možnost výběru mezi spravovanými registry a vytvořením registru, jehož vytvoření zajišťuje vlastní účet služby Storage.

* **Úložiště** – Registr obsahuje jedno nebo několik úložišť, což jsou skupiny imagí kontejnerů. Azure Container Registry podporuje víceúrovňové obory názvů úložiště. Tato funkce umožňuje seskupovat kolekce imagí souvisejících s konkrétní aplikací nebo kolekcí aplikací pro konkrétní vývojové nebo provozní týmy. Například:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` představuje image pro celý podnik.
  * `myregistry.azurecr.io/warrantydept/dotnet-build` představuje image sloužící k vytváření aplikací .NET, která je sdílená v rámci záručního oddělení.
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web` představuje webovou image seskupenou v zákaznické aplikaci pro odesílání a vlastněnou záručním oddělením.

* **Image** – Každá image uložená v úložišti je snímkem kontejneru Dockeru jen pro čtení. Registry kontejnerů Azure mohou zahrnovat image systémů Windows i Linux. Názvy imagí pro všechna nasazení kontejnerů určujete vy. Pomocí standardních [příkazů Dockeru](https://docs.docker.com/engine/reference/commandline/) můžete nahrávat image do úložiště nebo si z úložiště image stáhnout.

* **Kontejner** – Kontejner definuje softwarovou aplikaci a její závislosti zabalené do kompletního systému souborů, včetně kódu, modulu runtime, systémových nástrojů a knihoven. Spouštějte kontejnery Dockeru na základě imagí systémů Windows nebo Linux, které si stáhnete z registru kontejnerů. Kontejnery běžící na jednom počítači sdílí jádro operačního systému. Kontejnery Dockeru jsou plně přenositelné na všechny hlavní distribuce Linuxu, Mac i Windows.




## <a name="next-steps"></a>Další kroky
* [Vytvoření registru kontejnerů pomocí webu Azure Portal](container-registry-get-started-portal.md)
* [Vytvoření registru kontejnerů pomocí Azure CLI](container-registry-get-started-azure-cli.md)
* [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)
* Informace o sestavení pracovního postupu průběžné integrace a nasazení pomocí Visual Studio Team Services, Azure Container Service a Azure Container Registry, najdete v [tomto kurzu](../container-service/container-service-docker-swarm-setup-ci-cd.md).
* Pokud chcete v Azure nastavit vlastní privátní registr Dockeru (bez veřejného koncového bodu), přečtěte si téma [Nasazení vlastního privátního registru Dockeru v Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).

