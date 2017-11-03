---
title: "Přehled Azure Functions | Dokumentace Microsoftu"
description: "Během několika minut se naučte používat Azure Functions k optimalizaci asynchronních úloh."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 41f26a4b03a6431aaad21bda6336b8840d2d923f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="an-introduction-to-azure-functions"></a>Seznámení s Azure Functions  
Azure Functions je řešení umožňující snadno spouštět malé kódy (tzv. „funkce“) v cloudu. Můžete napsat přesně takový kód, jaký potřebujete pro aktuální problém, a nestarat se o infrastrukturu k jeho spuštění nebo aplikaci jako celek. Funkce provádět vývoj i zvýšit produktivitu a můžete použít svůj vývoj jazyk podle vlastní volby, například C#, F #, Node.js, Java nebo PHP. Plaťte pouze za čas, kdy kód běží, a potřebné škálování svěřte Azure. Azure Functions umožňuje vyvíjet [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) aplikací v Microsoft Azure.

Toto téma obsahuje obecný přehled Azure Functions. Pokud chcete rovnou začít a začít pracovat s funkcemi, začínat [vytvoření první funkce Azure](functions-create-first-azure-function.md). Pokud hledáte další odborné informace o Functions, přejděte k části [referenční informace pro vývojáře](functions-reference.md).

## <a name="features"></a>Funkce
Zde jsou některé klíčové funkce funkcí:

* **Volba jazyka** – pište funkce pomocí jazyka C#, F #, Node.js, Java, PHP, batch, bash nebo jakékoli spustitelný soubor.
* **Cenový model platba za použití** – Platíte jen za čas, kdy byl kód spuštěný. Viz možnost plánu hostování Consumption v [části týkající se cen](#pricing).  
* **Přineste si vlastní závislosti** – Functions podporuje NuGet a NPM, takže můžete používat své oblíbené knihovny.  
* **Integrované zabezpečení** – Chraňte funkce aktivované protokolem HTTP pomocí poskytovatelů OAuth, jako jsou Azure Active Directory, Facebook, Google, Twitter a účet Microsoft.  
* **Zjednodušená integrace** – Snadné využívání služeb Azure a nabídek softwaru jako služby (SaaS). Příklady najdete v [části týkající se integrace](#integrations).  
* **Flexibilní vývoj** – kódujte funkce přímo na portálu nebo nastavte průběžnou integraci a nasaďte kód prostřednictvím [Githubu](../app-service/scripts/app-service-cli-continuous-deployment-github.md), [Visual Studio Team Services](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md)a další [podporovaných vývojových nástrojů](../app-service/app-service-deploy-local-git.md).  
* **Open Source** – Modul runtime Functions je typu Open Source a [je dostupný na GitHubu](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co můžu dělat s Functions?
Functions je vynikající řešení pro zpracování dat, integraci systémů, práci s Internetu věcí (IoT) a vytváření jednoduchých rozhraní API a mikroslužeb. Zvažte Functions v případě úloh, jako je zpracování obrazu nebo objednávek a údržba souborů, a libovolných úloh, které chcete spustit podle plánu. 

Functions poskytuje šablony, které vám pomůžou začít s klíčovými scénáři, včetně těchto:

* **HTTPTrigger** – Aktivace provádění kódu pomocí žádosti protokolu HTTP. Příklad, naleznete v části [vytvoření první funkce](functions-create-first-azure-function.md).
* **TimerTrigger** – Provádění úkolů čištění nebo jiných dávkových úkolů podle předdefinovaného plánu. Příklad, naleznete v části [vytvořit funkci aktivovány časovač](functions-create-scheduled-function.md).
* **Webhook GitHubu** – Reakce na události, které nastaly v úložištích GitHubu. Příklad, naleznete v části [vytvořit funkci aktivovány webhook Githubu](functions-create-github-webhook-triggered-function.md).
* **Obecný webhook** – Zpracování žádostí webhooku protokolu HTTP z jakékoli služby, která podporuje webhooky. Příklad, naleznete v části [vytvořit funkci aktivovány obecný webhook](functions-create-generic-webhook-triggered-function.md).
* **CosmosDBTrigger** -proces Azure Cosmos DB dokumenty při jejich přidávání nebo aktualizaci v kolekcích v databáze NoSQL. Příklad, naleznete v části [vytvořit funkci aktivovány Azure Cosmos DB](functions-create-cosmos-db-triggered-function.md).
* **BlobTrigger** – Zpracování objektů blob Azure Storage po jejich přidání do kontejnerů. Tuto funkci můžete použít k změně velikosti imagí. Další informace najdete v tématu [objektu Blob úložiště vazby](functions-bindings-storage-blob.md).
* **QueueTrigger** – Reakce na zprávy přicházející do fronty Azure Storage. Příklad, naleznete v části [vytvořit funkci aktivovány Azure Queue storage](functions-create-storage-queue-triggered-function.md).
* **EventHubTrigger** – Reakce na události doručené do centra událostí Azure. Toto je obzvlášť užitečné pro scénáře instrumentace aplikací, zpracování činnosti nebo pracovního postupu uživatele a internetu věcí (IoT). Další informace najdete v tématu [vazby služby Event Hubs](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** – Připojení kódu k jiným službám Azure nebo místním službám prostřednictvím naslouchání frontě zpráv. Další informace najdete v tématu [Service Bus vazby](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger** – Připojení kódu k jiným službám Azure nebo místním službám prostřednictvím registrace k odběru témat. Další informace najdete v tématu [Service Bus vazby](functions-bindings-service-bus.md).

Azure Functions podporuje *triggery*, které představují jeden ze způsobů spuštění provádění kódu, a *vazby*, které zjednodušují kódování u vstupních a výstupních dat. Podrobný popis triggerů a vazeb podporovaných Azure Functions najdete v tématu [Referenční informace pro vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md).

## <a name="integrations"></a>Integrace
Azure Functions se integruje s celou řadou služeb Azure a služeb třetích stran. Tyto služby mohou aktivovat funkci a spustit provádění, nebo mohou sloužit jako vstup a výstup kódu. Azure Functions podporuje následující integrace služeb:

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Event Grid
* Azure Mobile Apps (tabulky)
* Azure Notification Hubs
* Azure Service Bus (fronty a témata)
* Azure Storage (objekt blob, fronty a tabulky) 
* GitHub (webhooky)
* Místní (pomocí služby Service Bus)
* Twilio (SMS zprávy)

## <a name="pricing"></a>Kolik stojí Functions?
Azure Functions nabízí dva druhy cenových plánů. Vyberte ten, který nejlépe vyhovuje vašim potřebám: 

* **Plán Consumption** – Když je funkce spuštěná, Azure poskytuje všechny nezbytné výpočetní prostředky. Nemusíte se starat o správu prostředků a platíte jenom čas, kdy byl kód spuštěný. 
* **Plán služby App Service** – Spouštějte funkce stejně jako webové a mobilní aplikace nebo aplikace API. Pokud již službu App Service používáte pro jiné aplikace, můžete spouštět funkce v tomtéž plánu bez dalších poplatků. 

Další informace o hostování plány najdete v tématu [Azure Functions hostování plán porovnání](functions-scale.md). Úplné podrobnosti o cenách jsou dostupné na [stránce Ceny Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Další kroky
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)  
  Umožňuje rovnou začít a vytvořit první funkci pomocí rychlého startu Azure Functions. 
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Poskytuje další odborné informace o modulu runtime Azure Functions a referenční informace pro kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné se službou Azure Functions (včetně plánu hostování Consumption), a výběr správného plánu. 
* [Další informace o Azure App Service](../app-service/app-service-web-overview.md)  
  Funkce využívá Azure Azure App Service pro základní funkce, jako třeba nasazení, proměnné prostředí a diagnostiky. 

