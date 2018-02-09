---
title: "Monitorování publikovaných rozhraní API pomocí služby Azure API Management | Microsoft Docs"
description: "Pomocí kroků v tomto kurzu se naučíte monitorovat rozhraní API pomocí služby Azure API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: db1ed08c4d4c9e9abd525ec13f5511da82ee1fe4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="monitor-published-apis"></a>Monitorování publikovaných rozhraní API

Azure Monitor je služba Azure, která poskytuje jeden zdroj pro monitorování prostředků Azure. Prostřednictvím služby Azure Monitor můžete vizualizovat metriky a protokoly pocházející z prostředků v Azure, jako je API Management, zadávat na ně dotazy, směrovat je, archivovat je a provádět s nimi příslušné akce. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zobrazení protokolů aktivit
> * Zobrazit diagnostické protokoly
> * Zobrazit metriky rozhraní API 
> * Nastavit pravidlo upozornění při neoprávněných voláních vašeho rozhraní API

Následující video ukazuje, jak pomocí služby Azure Monitor monitorovat službu API Management. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Požadavky

+ Projděte si následující rychlý start: [Vytvoření instance Azure API Managementu](get-started-create-service-instance.md).
+ Projděte si také následující kurz: Navíc kurzu: [Import a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Zobrazení protokolů aktivit

Protokoly aktivit poskytují přehled o operacích provedených vašimi službami API Management. Pomocí protokolů aktivit můžete zjistit obsah, autora a čas veškerých operací zápisu (PUT, POST, DELETE) provedených ve vašich službách API Management. 

> [!NOTE]
> Protokoly aktivit nezahrnují operace čtení (GET) ani operace prováděné na klasickém portálu pro vydavatele nebo pomocí původních rozhraní API pro správu.

Protokoly aktivit můžete zobrazit ve své službě API Management nebo k nim můžete získat přístup ze všech svých prostředků Azure prostřednictvím služby Azure Monitor. 

Zobrazení protokolů aktivit:

1. Vyberte instanci služby APIM.
2. Klikněte na **Protokol aktivit**.

## <a name="view-diagnostic-logs"></a>Zobrazení diagnostických protokolů

Diagnostické protokoly poskytují spoustu informací o operacích a chybách, které jsou důležité pro audit i pro účely odstraňování potíží. Diagnostické protokoly se liší od protokolů aktivit. Protokoly aktivit poskytují přehled o operacích provedených ve vašich prostředcích Azure. Diagnostické protokoly poskytují přehled o operacích, které provedly samotné vaše prostředky.

Přístup k diagnostickým protokolům:

1. Vyberte instanci služby APIM.
2. Klikněte na **Diagnostický protokol**.

## <a name="view-metrics-of-your-apis"></a>Zobrazení metrik vašich rozhraní API

API Management každou minutu vysílá metriky, takže vám skoro v reálném čase poskytuje přehled o stavu vašich rozhraní API. Toto je souhrn některých dostupných metrik:

* Kapacita (verze Preview): pomáhá při rozhodování o přechodu na vyšší nebo nižší verzi služeb APIM. Metrika se generuje každou minutu a odráží kapacitu brány v čase vytvoření sestavy. Její hodnoty se pohybují v rozsahu od 0 do 100 a počítají se na základě aspektů brány, jako je CPU nebo využití paměti.
* Celkový počet požadavků brány: počet požadavků rozhraní API za dané období. 
* Úspěšné požadavky brány: počet požadavků rozhraní API, které obdržely kódy úspěšné odpovědi HTTP včetně kódů 304, 307 a veškerých kódů menších než 301 (například 200). 
* Neúspěšné požadavky brány: počet požadavků rozhraní API, které obdržely kódy chybné odpovědi HTTP včetně kódů 400 a veškerých kódů větších než 500.
* Neoprávněné požadavky brány: počet požadavků rozhraní API, které obdržely kódy odpovědi HTTP jako 401, 403 a 429. 
* Ostatní požadavky brány: počet požadavků rozhraní API, které obdržely kódy odpovědi HTTP nepatřící do žádné z výše uvedených skupin (například 418).

Přístup k metrikám:

1. V nabídce ve spodní části stránky vyberte **Metriky**.
2. V rozevíracím seznamu vyberte metriky, které vás zajímají (můžete jich přidat několik). 
    
    V seznamu dostupných metrik vyberte třeba **Celkový počet požadavků brány** a **Neúspěšné požadavky brány**.
3. Graf zobrazí celkový počet volání rozhraní API. Ukazuje také počet volání rozhraní API, která se nezdařila. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Nastavení pravidla upozornění při neoprávněných požadavcích

Můžete nakonfigurovat odesílání upozornění na základě metrik a protokolů aktivit. Azure Monitor umožňuje nakonfigurovat upozornění, které při aktivaci provede některé z následujících kroků:

* Odeslání e-mailového oznámení
* Volání webhooku
* Vyvolání aplikace logiky Azure

Konfigurace upozornění:

1. V řádku nabídek ve spodní části stránky vyberte **Pravidla výstrah**.
2. Vyberte **Přidat upozornění metriky**.
3. Zadejte **Název** tohoto upozornění.
4. Jako metriku k monitorování vyberte **Neoprávněné požadavky brány**.
5. Vyberte možnost **Vlastníci, přispěvatelé a čtenáři e-mailů**.
6. Stiskněte **OK**.
7. Zkuste volat naše rozhraní API Conference bez klíče rozhraní API. Jako vlastník této služby API Management dostanete e-mailem upozornění. 

    > [!TIP]
    > Pravidlo upozornění může také při aktivaci volat webhook nebo aplikaci logiky Azure.

    ![Nastavení upozornění](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Zobrazení protokolů aktivit
> * Zobrazit diagnostické protokoly
> * Zobrazit metriky rozhraní API 
> * Nastavit pravidlo upozornění při neoprávněných voláních vašeho rozhraní API

Přejděte k dalšímu kurzu:

> [!div class="nextstepaction"]
> [Trasování volání](api-management-howto-api-inspector.md)
