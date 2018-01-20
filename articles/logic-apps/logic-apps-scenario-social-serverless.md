---
title: "Scénář – vytvoření zákazníka přehledný řídicí panel s bez serveru Azure | Microsoft Docs"
description: "Příklad, jak můžete vytvořit řídicí panel pro správu názory zákazníků, sociální data a další s funkcemi Azure a Azure Logic Apps."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: d3e07b8d7194d83e3ba3986177170edff21e1d7a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Vytvořte v reálném čase zákazníka přehledný řídicí panel s funkcemi Azure a Azure Logic Apps

Nástroje Azure bez serveru zadejte výkonné možnosti k rychlému vytvoření a hostování aplikací v cloudu, aniž by museli vezměte v úvahu infrastruktury.  V tomto scénáři vytvoříme řídicí panel aktivovat názorů zákazníků, analýzu zpětnou vazbu pomocí machine learning a publikování Statistika zdroj jako Power BI nebo Azure Data Lake.

## <a name="overview-of-the-scenario-and-tools-used"></a>Přehled scénáře a použít nástroje

Chcete-li implementaci tohoto řešení, jsme využije dvě klíčové komponenty aplikace bez serveru v Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) a [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Služba Logic Apps je modul bez serveru pracovního postupu v cloudu.  Poskytuje orchestration mezi komponentami bez serveru a také se připojuje k více než 100 služeb a rozhraní API.  V tomto scénáři vytvoříme aplikace logiky k aktivaci na zpětnou vazbu od zákazníků.  Mezi konektory, které může být užitečné při reaktivní na názory zákazníků patří Outlook.com, Office 365, opic průzkum, Twitter a požadavek HTTP [z webového formuláře](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  V pracovním postupu níže jsme monitorovat hashtag na Twitteru.

Funkce zajistit bez serveru výpočetní v cloudu.  V tomto scénáři budeme používat Azure Functions na příznak tweetů od zákazníků založené na řadu předem definovaná klíčová slova.

Může být celé řešení [sestavení v sadě Visual Studio](logic-apps-deploy-from-vs.md) a [nasazen jako součást šablony prostředků](logic-apps-create-deploy-template.md).  Je také video s návodem scénáře [na webu Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a>Vytvoření aplikace logiky k aktivaci na data zákazníků

Po [vytvoření aplikace logiky](quickstart-create-first-logic-app-workflow.md) v sadě Visual Studio nebo portálu Azure:

1. Přidat aktivační událost pro **na nové Tweetů** ze služby Twitter.
2. Nakonfigurujte aktivační událost pro naslouchání na tweetů na – klíčové slovo nebo hashtag.

   > [!NOTE]
   > Vlastnost opakování na aktivační událost určí, jak často se aplikace logiky kontrolovat přítomnost nových položek na základě cyklického dotazování aktivační události

   ![Příklad aktivační události služby Twitter.][1]

Tato aplikace bude nyní platit na všechny nové tweetů.  Pak můžeme provést tweet dat a lépe postojích vyjádřené porozumět.  Pro tento používáme [kognitivní služby Azure](https://azure.microsoft.com/services/cognitive-services/) ke zjištění postojích textu.

1. Klikněte na tlačítko **nový krok**
1. Vyberte nebo vyhledejte **Analýza textu** konektoru
1. Vyberte **zjistit postojích** operace
1. Pokud se zobrazí výzva, zadejte platný klíč kognitivní služby pro službu Analýza textu
1. Přidat **Tweet Text** jako text k analýze.

Teď, když máme tweet dat a statistika na tweet, může být několik dalších konektorů relevantní:
* Power BI - přidávání řádků do datové sady streamování: zobrazení tweetů reálném čase na řídicí panel Power BI.
* Azure Data Lake - připojit soubor: přidat data zákazníků k datové sadě služby Azure Data Lake pro zahrnutí do úlohy analytics.
* SQL - přidávání řádků: ukládání dat v databázi pro pozdější načtení.
* Slack – poslat zprávu: výstrahy slack kanál na záporné zpětnou vazbu, která vyžaduje akce.

Funkce Azure lze také provést další vlastní výpočet nad data.

## <a name="enriching-the-data-with-an-azure-function"></a>Rozšíření dat pomocí funkce Azure

Můžeme vytvořit funkci, potřebujeme mít aplikaci funkce v naše předplatné Azure.  Informace o vytvoření funkce Azure na portálu můžete [se nachází tady](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Pro funkce k volání přímo z aplikace logiky, je nutné mít HTTP aktivovat vazby.  Doporučujeme používat **HttpTrigger** šablony.

V tomto scénáři by textu žádosti funkce Azure tweet text.  V kódu funkce jednoduše definujte logiku Pokud tweet text obsahuje klíčové slovo nebo frázi.  Samotná funkce může být udržovány jako jednoduché nebo komplexní, podle potřeby pro scénář.

Na konci funkce jednoduše vrátí odpověď do aplikace logiky určitými daty.  To může být jednoduchý logickou hodnotu (například `containsKeyword`), nebo komplexní objekt.

![Nakonfigurované krok funkce Azure][2]

> [!TIP]
> Při přístupu k komplexní odpověď z funkce v aplikaci logiky, použijte akci analyzovat JSON.

Po uložení funkce mohou být přidány do aplikace logiky vytvořili výše.  V aplikaci logiky:

1. Klikněte na tlačítko Přidat **nový krok**
1. Vyberte **Azure Functions** konektoru
1. Vybrat a vyberte existující funkce a přejděte do funkce vytvořena
1. Poslat **Tweet Text** pro **text žádosti**

## <a name="running-and-monitoring-the-solution"></a>Spuštění a sledování řešení

Jednou z výhod vytváření bez serveru orchestrations v Logic Apps je bohaté ladění a možnosti monitorování.  Všechny spustit (aktuální nebo historické) lze zobrazit z v sadě Visual Studio, portálu Azure nebo prostřednictvím rozhraní API REST a sady SDK.

Jedním z nejjednodušších způsobů, jak aplikaci logiky si otestujte používá **spustit** tlačítko v návrháři.  Kliknutím na tlačítko **spustit** bude pokračovat pro dotazování aktivační událost každých 5 sekund, dokud je zjištěna událost a pojmenujte za provozu zobrazení v průběhu běhu.

Předchozí spuštění historií lze zobrazit v okně Přehled v portálu Azure nebo pomocí Průzkumníka cloudové služby Visual Studio.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Vytvoření šablony nasazení pro automatické nasazení

Jakmile byla vyvinuta řešení, můžete zachytit a nasazují přes šablonu nasazení Azure všechny oblasti Azure na světě.  To je užitečné pro oba změny parametry pro různé verze tento pracovní postup, ale také pro integraci toto řešení v kanálu sestavení a verze.  Informace o vytváření šablony nasazení lze nalézt [v tomto článku](logic-apps-create-deploy-template.md).

Azure Functions také být použity během nasazení šablony - tak celé řešení s všechny závislosti můžete spravovat jako jednu šablonu.  Příklad funkce šablony nasazení lze nalézt v [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Další postup

* [Zobrazit další příklady a scénáře pro Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Podívejte se na video s návodem k vytvoření tohoto řešení pro kompletní](http://aka.ms/logicappsdemo)
* [Zjistěte, jak zpracovat a zachycení výjimek v rámci aplikace logiky](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png