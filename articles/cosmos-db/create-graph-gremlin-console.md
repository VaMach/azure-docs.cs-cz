---
title: "Kurz ke službě Azure Cosmos DB: Vytváření, zadávání dotazů a procházení v konzole Gremlin | Dokumentace Microsoftu"
description: "Rychlý start ke službě Azure Cosmos DB vám pomůže s vytvářením vrcholů, okrajů a dotazů pomocí rozhraní Graph API služby Azure Cosmos DB."
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fb27ba1a70959ba92fbd021e9e42438081000e45
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-the-gremlin-console"></a>Služba Azure Cosmos DB: Vytváření, zadávání dotazů a procházení grafu v konzole Gremlin

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB. 

Tento rychlý start popisuje způsob vytvoření účtu služby Azure Cosmos DB a grafu (kontejneru) pomocí portálu Azure Portal a následné použití v [konzole Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) pro práci s daty (náhledem) rozhraní Graph API. V tomto kurzu se naučíte vytvářet vrcholy a okraje a zadávat k nim dotazy, a to aktualizací vlastnosti vrcholu, a dále zadávat dotazy pro vrcholy, procházet graf a vyřadit konkrétní vrchol.

![Služba Azure Cosmos DB z konzoly Apache Gremlin](./media/create-graph-gremlin-console/gremlin-console.png)

Konzola Gremlin je založena na technologii Groovy nebo Java a běží v systémech Linux, Mac a Windows. Můžete si ji stáhnout z [webu Apache TinkerPop](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.4/apache-tinkerpop-gremlin-console-3.2.4-bin.zip).

## <a name="prerequisites"></a>Požadavky

Abyste si mohli vytvořit účet služby Azure Cosmos DB pro tento rychlý start, musíte mít předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Musíte si také nainstalovat [konzolu Gremlin](http://tinkerpop.apache.org/). Použijte verzi 3.2.4 nebo vyšší.

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a id="ConnectAppService"></a>Připojení ke službě aplikace
1. Než začnete používat konzolu Gremlin, vytvořte nebo upravte v adresáři *apache-tinkerpop-gremlin-console-3.2.4/conf* konfigurační soubor *remote-secure.yaml*.
2. Vyplňte parametry *Hostitel*, *Port*, *Uživatelské jméno*, *Heslo*, *Fond připojení* a *Serializátor*:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Hostitelé|***.graphs.azure.com|Identifikátor URI služby grafu, který můžete načíst z portálu Azure Portal
    Port|443|Nastavte na hodnotu 443
    Uživatelské jméno|*Vaše uživatelské jméno*|Prostředek formuláře `/dbs/<db>/colls/<coll>`.
    Heslo|*Primární hlavní klíč*|Váš primární hlavní klíč ke službě Azure Cosmos DB
    Fond připojení|{enableSsl: true}|Nastavení fondu připojení pro protokol SSL
    Serializátor|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Nastavte na tuto hodnotu

3. V terminálu spusťte [konzolu Gremlin](http://tinkerpop.apache.org/docs/3.2.4/tutorials/getting-started/) příkazem *bin/gremlin.bat* nebo *bin/gremlin.sh*.
4. V terminálu se připojte ke službě aplikace spuštěním příkazu *:remote connect tinkerpop.server conf/remote-secure.yaml*.

Výborně! Nastavení se nám podařilo dokončit a teď můžete spouštět některé příkazy konzoly.

## <a name="create-vertices-and-edges"></a>Vytváření vrcholů a okrajů

Začněme přidáním čtyř osob pro nastavení vrcholů: *Tomáš*, *Marie*, *Robin* a *Petr*.

Vstup (Tomáš):

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Výstup:

```
==>[id:1eb91f79-94d7-4fd4-b026-18f707952f21,label:person,type:vertex,properties:[firstName:[[id:ec5fcfbe-040e-48c3-b961-31233c8b1801,value:Thomas]],lastName:[[id:86e5b580-0bca-4bc2-bc53-a46f92c1a182,value:Andersen]],age:[[id:2caeab3c-c66d-4098-b673-40a8101bb72a,value:44]]]]
```
Vstup (Marie):

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39)
```

Výstup:

```
==>[id:899a9d37-6701-48fc-b0a1-90950be7e0f4,label:person,type:vertex,properties:[firstName:[[id:c79c5599-8646-47d1-9a49-3456200518ce,value:Mary Kay]],lastName:[[id:c1362095-9dcc-479d-ab21-86c1b6d4ffc1,value:Andersen]],age:[[id:0b530408-bfae-4e8f-98ad-c160cd6e6a8f,value:39]]]]
```

Vstup (Robin):

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield')
```

Výstup:

```
==>[id:953aefd9-5a54-4033-9b3a-d4dc3049f720,label:person,type:vertex,properties:[firstName:[[id:bbda02e0-8a96-4ca1-943e-621acbb26824,value:Robin]],lastName:[[id:f0291ad3-05a3-40ec-aabb-6538a7c331e3,value:Wakefield]]]]
```

Vstup (Petr):

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller')
```

Výstup:

```
==>[id:81c891d9-beca-4c87-9009-13a826c9ed9a,label:person,type:vertex,properties:[firstName:[[id:3a3b53d3-888c-46da-bb54-1c42194b1e18,value:Ben]],lastName:[[id:48c6dd50-79c4-4585-ab71-3bf998061958,value:Miller]]]]
```

V dalším kroku přidejme pro vztahy mezi osobami okraje.

Vstup (Tomáš -> Marie):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

Výstup:

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Vstup (Tomáš -> Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

Výstup:

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Vstup (Robin -> Petr):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

Výstup:

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a>Aktualizace vrcholu

Aktualizujme vrchol *Tomáš* s novým věkovým údajem *45* let.

Vstup:
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
Výstup:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a>Dotazování grafu

Teď spustíme pro graf celou řadu dotazů.

Vyzkoušejme nejdříve dotaz s filtrem pro vrácení pouze osob nad 40 let.

Vstup (dotaz s filtrem):

```
:> g.V().hasLabel('person').has('age', gt(40))
```

Výstup:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

Teď navrhněme název pro skupinu osob nad 40 let.

Vstup (filtru + dotaz projekce):

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

Výstup:

```
==>Thomas
```

## <a name="traverse-your-graph"></a>Procházení grafu

Umožňuje procházet graf tak, aby vrátil všechny přátele uživatele Tomáš.

Vstup (přátelé uživatele Tomáš):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

Výstup: 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

V následujícím kroku načteme další vrstvu vrcholů. Umožňuje procházet graf tak, aby vrátil všechny přátele uživatele Tomáš.

Vstup (přátelé přátel uživatele Tomáš):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
Výstup:

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a>Vyřazení vrcholu

Teď odstraníme vrchol z databáze grafu.

Vstup (vyřazení vrcholu uživatele Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').drop()
```

## <a name="clear-your-graph"></a>Resetování grafu

Nakonec odstraníme z databáze všechny vrcholy a okraje.

Vstup:

```
:> g.V().drop()
```

Blahopřejeme! Dokončili jste tento kurz rozhraní Graph API služby Azure Cosmos DB!

## <a name="review-slas-in-the-azure-portal"></a>Ověření podmínek SLA na portálu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:  

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili se způsobem vytvoření účtu služby Azure Cosmos DB, vytvoření grafu pomocí Průzkumníku dat, vytváření vrcholů a okrajů a procházení grafu pomocí konzoly Gremlin. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)

