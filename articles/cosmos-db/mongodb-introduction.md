---
title: "Úvod do Azure Cosmos DB: rozhraní API pro MongoDB | Microsoft Docs"
description: "Zjistěte, jak můžete používat Azure Cosmos databázi k ukládání a dotaz ohromné objemy dokumentů JSON s nízkou latencí pomocí Oblíbené MongoDB rozhraní API operačních systémů."
keywords: Co je MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 4dbf91a3c1d6a287d7337647f9e059566c7ddbe5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Úvod do Azure Cosmos DB: rozhraní API pro MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Azure Cosmos DB poskytuje [globální distribuci na klíč](distribute-data-globally.md), [elastické škálování propustnosti a úložiště](partition-data.md) po celém světě, latence v řádu milisekund na 99. percentilu, [pět jasně definovaných úrovní konzistence](consistency-levels.md) a zaručenou vysokou dostupnost. To vše je podloženo [nejlepšími smlouvami SLA v oboru](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), aniž by vyžadovala zapojení správy schémat a indexů. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Azure Cosmos DB: MongoDB rozhraní API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Cosmos DB databází lze použít jako úložiště dat pro aplikace napsané pro [MongoDB](https://docs.mongodb.com/manual/introduction/). To znamená, že pomocí stávající [ovladače](https://docs.mongodb.org/ecosystem/drivers/), vaše aplikace napsané pro MongoDB teď můžete komunikovat s Cosmos DB a používat Cosmos DB databáze místo databáze MongoDB. V mnoha případech můžete přepínat pomocí MongoDB do databáze Cosmos jednoduše změnou připojovací řetězec. Pomocí této funkce lze snadno vytvářet a spouštět aplikace databázi MongoDB ve službě Azure cloud s globální distribuční databázi Cosmos Azure a [komplexní špičkové SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), nadále používat známé dovednosti a nástroje pro MongoDB.


## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Co je výhodou používání Azure Cosmos DB pro MongoDB aplikace?

**Elasticky škálovatelná propustnost a úložiště:** snadno škálovat nahoru nebo dolů databázi MongoDB splňovala potřeby vaší aplikace. Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci. Cosmos DB podporuje MongoDB kolekce, které je možné škálovat na prakticky neomezené velikosti úložiště a zřízené propustnosti. Je možné Elasticky škálovat Cosmos DB s předvídatelným výkonem bezproblémově růstem vaší aplikace. 

**Replikace více oblast:** Cosmos DB transparentně replikuje data do všech oblastí, které jste spojené s vaším účtem MongoDB umožňuje vyvíjet aplikace, které vyžadují globální přístup k datům při současném poskytování kompromisy mezi konzistence, dostupnosti a výkonu, všechny s odpovídající záruky. Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání s více funkci rozhraní API a možnost Elasticky škálovat propustnost a úložiště po celém světě. Další informace v [distribuci dat globálně](distribute-data-globally.md).

**Kompatibilita MongoDB**: můžete použít existující MongoDB znalosti, kód aplikace a nástrojů. Můžete vyvíjet aplikace, které používají MongoDB a nasadit je do produkčního prostředí pomocí plně spravovaná globálně distribuované služby Cosmos DB.

**Žádný server správy**: Nemáte ke správě a škálovat vaše databáze MongoDB. Cosmos DB je plně spravovaná služba, což znamená, že nemusíte spravovat všechny infrastruktury nebo virtuální počítače sami. Je k dispozici v 30 + cosmos DB [oblasti Azure](https://azure.microsoft.com/regions/services/).

**Přizpůsobitelné úrovně konzistence:** Pro dosažení optimálního poměru mezi konzistencí a výkonem si můžete vybrat z pěti jasně definovaných úrovní konzistence. Pro dotazy a operace čtení Cosmos DB nabízí pět úrovně konzistence: silnou, s ohraničenou odolností, založenou relace, konzistentní Předpona a případnou. Tyto podrobné, dobře definované úrovně konzistence umožňují zvolit vhodný poměr mezi konzistencí, dostupností a latencí. Další informace najdete v tématu popisujícím [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu](consistency-levels.md).

**Automatické indexování**: ve výchozím nastavení, Cosmos DB automaticky indexuje všechny vlastnosti v rámci dokumenty ve vaší MongoDB databáze a nemá očekávat nebo nevyžaduje žádné schéma nebo vytváření sekundárních indexů.

**Podnikové úrovni** -Azure Cosmos DB podporuje více místní repliky k poskytování 99,99 % dostupnost a ochranu dat při krátkodobém místní a regionální selhání. Azure Cosmos DB má podnikové úrovni [dodržování předpisů certifikace](https://www.microsoft.com/trustcenter) a funkce zabezpečení. 

Další informace v této Azure videa s Scott Hanselman a Azure Cosmos DB hlavní inženýrství manažer Kirill Gavrylyuk pátek.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a>Jak začít

Postupujte podle quickstarts MongoDB vytvořit účet Cosmos DB a migrovat stávající aplikace Mongo DB pomocí Cosmos DB nebo vytvořit nový:

* [Migrovat stávající webovou aplikaci Node.js MongoDB](create-mongodb-nodejs.md).
* [Vytvoření webové aplikace MongoDB API pomocí rozhraní .NET a portálu Azure](create-mongodb-dotnet.md)
* [Sestavení rozhraní API MongoDB konzolovou aplikaci Java a portálu Azure](create-mongodb-java.md)

## <a name="next-steps"></a>Další kroky

Informace o rozhraní API MongoDB Azure Cosmos DB je integrována do dokumentace se celkově Azure Cosmos DB, ale tady jsou na několik věcí, které vám pomůžou začít:

* Postupujte podle [připojit k účtu MongoDB](connect-mongodb-account.md) kurzu se dozvíte, jak získat informace o účtu připojovací řetězec.
* Postupujte podle [MongoChef použití s Azure Cosmos DB](mongodb-mongochef.md) kurzu se dozvíte, jak vytvořit připojení mezi Azure Cosmos DB databáze a MongoDB aplikace v MongoChef.
* Postupujte podle [migrace dat do databáze Cosmos Azure s protokolem podporu pro MongoDB](mongodb-migrate.md) kurzu pro import dat do rozhraní API pro databázi MongoDB.
* Připojit k rozhraní API pro použití účtu MongoDB [Robomongo](mongodb-robomongo.md).
* Zjistěte, kolik RUs jsou vaše operace pomocí [GetLastRequestStatistics příkaz a metriku portálu Azure](request-units.md#GetLastRequestStatistics).
* Zjistěte, jak [konfigurace pro čtení předvolby pro globálně distribuované aplikace](../cosmos-db/tutorial-global-distribution-mongodb.md).
