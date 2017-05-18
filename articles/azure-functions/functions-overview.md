---
title: "Přehled Azure Functions | Dokumentace Microsoftu"
description: "Během několika minut se naučte používat Azure Functions k optimalizaci asynchronních úloh."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: cfowler;mahender;glenga
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7fcf35b99cc410655f2a3b9b6961f5e3bef3aeb5
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="an-introduction-to-azure-functions"></a>Seznámení s Azure Functions  
Azure Functions je řešení umožňující snadno spouštět malé kódy (tzv. „funkce“) v cloudu. Můžete napsat přesně takový kód, jaký potřebujete pro aktuální problém, a nestarat se o infrastrukturu k jeho spuštění nebo aplikaci jako celek. Služba Functions může ještě zvýšit produktivitu vývoje, a navíc k vývoji můžete použít jazyk podle vlastní volby, například C#, F#, Node.js, Python nebo PHP. Plaťte pouze za čas, kdy kód běží, a potřebné škálování svěřte Azure. Služba Azure Functions umožňuje v Microsoft Azure vytvářet aplikace bez serveru.

Toto téma obsahuje obecný přehled Azure Functions. Pokud chcete rovnou začít používat Azure Functions, začněte tématem [Vytvoření první funkce Azure](functions-create-first-azure-function.md). Pokud hledáte další odborné informace o Functions, přejděte k části [referenční informace pro vývojáře](functions-reference.md).

## <a name="features"></a>Funkce
Toto jsou některé klíčové funkce Azure Functions:

* **Volba jazyka** – Pište funkce pomocí jazyka C#, F#, Node.js, Python, F#, PHP, Batch, Bash nebo libovolného spustitelného jazyka.
* **Cenový model platba za použití** – Platíte jen za čas, kdy byl kód spuštěný. Viz možnost plánu hostování Consumption v [části týkající se cen](#pricing).  
* **Přineste si vlastní závislosti** – Functions podporuje NuGet a NPM, takže můžete používat své oblíbené knihovny.  
* **Integrované zabezpečení** – Chraňte funkce aktivované protokolem HTTP pomocí poskytovatelů OAuth, jako jsou Azure Active Directory, Facebook, Google, Twitter a účet Microsoft.  
* **Zjednodušená integrace** – Snadné využívání služeb Azure a nabídek softwaru jako služby (SaaS). Příklady najdete v [části týkající se integrace](#integrations).  
* **Flexibilní vývoj** – Kódujte funkce přímo na portálu nebo nastavte průběžnou integraci a nasaďte kód prostřednictvím nástrojů GitHub, Visual Studio Team Services a dalších [podporovaných vývojových nástrojů](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open Source** – Modul runtime Functions je typu Open Source a [je dostupný na GitHubu](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co můžu dělat s Functions?
Azure Functions je vynikající řešení pro zpracování dat, integraci systémů, práci s internetem věcí (IoT) a vytváření jednoduchých rozhraní API a mikroslužeb. Zvažte Functions v případě úloh, jako je zpracování obrazu nebo objednávek a údržba souborů, a libovolných úloh, které chcete spustit podle plánu. 

Functions poskytuje šablony, které vám pomůžou začít s klíčovými scénáři, včetně těchto:

* **BlobTrigger** – Zpracování objektů blob Azure Storage po jejich přidání do kontejnerů. Tuto funkci můžete použít k změně velikosti imagí.
* **EventHubTrigger** – Reakce na události doručené do centra událostí Azure. Toto je obzvlášť užitečné pro scénáře instrumentace aplikací, zpracování činnosti nebo pracovního postupu uživatele a internetu věcí (IoT).
* **Obecný webhook** – Zpracování žádostí webhooku protokolu HTTP z jakékoli služby, která podporuje webhooky.
* **Webhook GitHubu** – Reakce na události, které nastaly v úložištích GitHubu. Příklady najdete v tématu [Vytvoření webhooku nebo funkce rozhraní API](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** – Aktivace provádění kódu pomocí žádosti protokolu HTTP.
* **QueueTrigger** – Reakce na zprávy přicházející do fronty Azure Storage. Příklad najdete v tématu [Vytvoření funkce Azure s vazbou na službu Azure](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** – Připojení kódu k jiným službám Azure nebo místním službám prostřednictvím naslouchání frontě zpráv. 
* **ServiceBusTopicTrigger** – Připojení kódu k jiným službám Azure nebo místním službám prostřednictvím registrace k odběru témat. 
* **TimerTrigger** – Provádění úkolů čištění nebo jiných dávkových úkolů podle předdefinovaného plánu. Příklad najdete v tématu [Vytvoření funkce zpracování událostí](functions-create-an-event-processing-function.md).

Azure Functions podporuje *triggery*, které představují jeden ze způsobů spuštění provádění kódu, a *vazby*, které zjednodušují kódování u vstupních a výstupních dat. Podrobný popis triggerů a vazeb podporovaných Azure Functions najdete v tématu [Referenční informace pro vývojáře triggerů a vazeb Azure Functions](functions-triggers-bindings.md).

## <a name="integrations"></a>Integrace
Azure Functions se integruje s celou řadou služeb Azure a služeb třetích stran. Tyto služby mohou aktivovat funkci a spustit provádění, nebo mohou sloužit jako vstup a výstup kódu. Azure Functions podporuje následující integrace služeb. 

* Databáze Azure Cosmos
* Azure Event Hubs 
* Azure Mobile Apps (tabulky)
* Azure Notification Hubs
* Azure Service Bus (fronty a témata)
* Azure Storage (objekt blob, fronty a tabulky) 
* GitHub (webhooky)
* Místní (pomocí služby Service Bus)
* Twilio (SMS zprávy)

## <a name="pricing"></a>Kolik stojí Functions?
Azure Functions nabízí dva druhy cenových plánů. Zvolte ten, který nejlíp vyhovuje vašim potřebám: 

* **Plán Consumption** – Když je funkce spuštěná, Azure poskytuje všechny nezbytné výpočetní prostředky. Nemusíte se starat o správu prostředků a platíte jenom čas, kdy byl kód spuštěný. 
* **Plán služby App Service** – Spouštějte funkce stejně jako webové a mobilní aplikace nebo aplikace API. Pokud již službu App Service používáte pro jiné aplikace, můžete spouštět funkce v tomtéž plánu bez dalších poplatků. 

Úplné podrobnosti o cenách jsou dostupné na [stránce Ceny Functions](https://azure.microsoft.com/pricing/details/functions/). Další informace o škálování funkcí najdete v tématu [Postup škálování Azure Functions](functions-scale.md).

## <a name="next-steps"></a>Další kroky
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)  
  Umožňuje rovnou začít a vytvořit první funkci pomocí rychlého startu Azure Functions. 
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Poskytuje další odborné informace o modulu runtime Azure Functions a referenční informace pro kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné se službou Azure Functions (včetně plánu hostování Consumption), a výběr správného plánu. 
* [Další informace o Azure App Service](../app-service/app-service-value-prop-what-is.md)  
  Azure Functions využívá platformu Azure App Service pro základní funkce, jako jsou nasazení, proměnné prostředí a diagnostika. 


