---
title: "Pomocí rozhraní Mongoose Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak se připojit k databázi Azure Cosmos aplikace Node.js Mongoose"
services: cosmos-db
documentationcenter: 
author: romitgirdhar
manager: jhubbard
editor: 
ms.assetid: de5eea58-ee7c-4609-b1c9-4af3e61a5883
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: rogirdh
ms.openlocfilehash: 9878936b5dd76730633dec16b1c3a3eaac78e95a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Pomocí Azure Cosmos DB rozhraní Mongoose

Tento kurz ukazuje, jak používat [Mongoose Framework](http://mongoosejs.com/) při ukládání dat v Azure Cosmos DB. V tomto návodu používáme MongoDB rozhraní API pro Azure Cosmos DB. Pro těch, které jste obeznámeni Mongoose představuje rozhraní objektu modelování pro MongoDB v Node.js a poskytuje jednoduché a na základě schématu řešení pro modelování data aplikací.

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) verze 0.10.29 nebo vyšší

## <a name="create-an-azure-cosmos-db-account"></a>Vytvoření účtu služby Azure Cosmos DB

Vytvořme účet služby Azure Cosmos DB. Pokud již máte účet, který chcete použít, můžete přeskočit na [nastavení aplikace Node.js](#SetupNode). Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení aplikace Node.js](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Nastavení aplikace Node.js

>[!Note]
> Pokud jste chtěli jenom návod ukázkový kód místo instalace vlastní aplikace, klonovat [ukázka](https://github.com/Azure-Samples/Mongoose_CosmosDB) používá pro účely tohoto kurzu a sestavit aplikaci Node.js Mongoose v Azure Cosmos DB.

1. Pokud chcete vytvořit aplikace Node.js ve složce podle vaší volby, spusťte následující příkaz v příkazovém řádku uzlu.

    ```npm init```

    Odpovězte na otázky a projektu budou připravené na vynucování.

1. Přidat nový soubor do složky a pojmenujte ji ```index.js```.
1. Nainstaluje potřebné balíčky pomocí jedné z ```npm install``` možnosti:
    * Mongoose:```npm install mongoose --save```
    * Dotenv (Pokud chcete načíst vaše tajné klíče ze souboru .env):```npm install dotenv --save```
    
    >[!Note]
    > ```--save``` Příznak přidá do souboru package.json závislost.

1. Importujte závislosti v souboru index.js.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Přidat Cosmos DB připojovací řetězec a název databáze Cosmos k ```.env``` souboru.

    ```JavaScript
    COSMOSDB_CONNSTR={Your MongoDB Connection String Here}
    COSMOSDB_DBNAME={Your DB Name Here}
    ```

1. Připojte k databázi Cosmos Azure pomocí rozhraní Mongoose přidáním následující kód do konce index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb"); //Creates a new DB, if it doesn't already exist

    var db = mongoose.connection;
    db.on('error', console.error.bind(console, 'connection error:'));
    db.once('open', function() {
    console.log("Connected to DB");
    });
    ```
    >[!Note]
    > Proměnné prostředí jsou zde načtené pomocí možnosti process.env. {variableName} pomocí balíčku npm 'dotenv'.

    Po připojení k databázi Cosmos Azure nyní můžete spustit nastavení objektové modely v Mongoose.
    
## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Upozornění pro Azure Cosmos DB pomocí Mongoose

Pro každý model, který vytvoříte Mongoose vytvoří novou kolekci MongoDB pod pozadí. Však zadána model fakturace pro kolekci Azure Cosmos databáze, nemusí být nejvíce finančně efektivním způsobem přejít, pokud máte k dispozici více objektové modely, které jsou řídce naplněny.

Tento návod popisuje obou modelů. Nejprve zaměříme návod na ukládání jeden typ dat na kolekci. Toto je chování defacto Mongoose.

Mongoose má také koncept názvem [Discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators jsou mechanismus dědičnosti schématu. Umožňují vám tak, aby měl více modelů s překrývající se schémat na stejnou základní kolekci MongoDB.

Můžete ukládat různé datové modely ve stejné kolekci a pak stahují jenom data potřebná pomocí klauzuli filtru v době dotazu.

### <a name="one-collection-per-object-model"></a>Jedna kolekce za objektový model

Výchozí chování Mongoose je vytvoření kolekce MongoDB pokaždé, když vytvoříte model objektů. V této části popisuje jak dosáhnout s MongoDB pro Azure Cosmos DB. Tato metoda se doporučuje s Azure Cosmos DB, když máte objektové modely s velkými objemy dat. Toto je výchozí operační modelu pro Mongoose, proto je seznámit s tím, pokud jste obeznámeni s Mongoose.

1. Otevřete váš ```index.js``` znovu.

1. Vytvořte definici schématu pro 'Family'.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Vytvoření objektu pro 'Family'.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Nakonec umožňuje ukládat objekt Azure Cosmos. Tím se vytvoří kolekce pod pozadí.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Nyní vytvoříme jiné schéma a objekt. Tento čas, můžeme vytvořit pro dovolenou cílů, které může být zajímá rodiny.
    1. Stejně jako poslední můžeme vytvořit schéma
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Vytvoření ukázkové objektu (můžete přidat více objektů pro toto schéma) a uložte ho.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Nyní přejděte do portálu Azure, jste si všimli dvě kolekce vytvořené v Azure Cosmos DB.

    ![Kurz k Node.js – snímek obrazovky portálu Azure znázorňující účet Azure Cosmos databáze s názvem kolekce zvýrazněná – databáze Node][alldata]

1. Umožňuje číst nakonec data z databáze Azure Cosmos. Vzhledem k tomu, že používáme výchozí provozní model Mongoose, čtení jsou stejné jako další čtení s Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Pomocí Mongoose discriminators k ukládání dat do jedné kolekce

Tato metoda používáme [Mongoose Discriminators](http://mongoosejs.com/docs/discriminators.html) za účelem optimalizace pro nákladů na každou kolekci Azure Cosmos DB. Discriminators umožňují definovat rozdílné 'klíče', které umožňuje ukládání, rozlišit a filtrovat modely jiný objekt.

Tady vytváříme základní objekt modelu, definujte rozdílné klíč a přidání 'Family' a 'VacationDestinations' jako rozšíření základní modelu.

1. Umožňuje nastavit základní konfigurace a definujte diskriminátoru klíč.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. V dalším kroku nastavíme běžné objektový model

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Nyní jsme definovali 'Family' modelu. Si zde všimnout, že používáme ```commonModel.discriminator``` místo ```mongoose.model```. Kromě toho také přidáváme základní konfigurace mongoose schématu. Zde discriminatorKey Ano, je ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Podobně přidejme jiné schéma, tentokrát pro 'VacationDestinations'. Tady je DiscriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Navíc umožňuje vytvářet objekty pro model a uložte ho.
    1. Umožňuje přidat objekty do modelu 'Family'.
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Dále umožňuje přidat objekty do modelu 'VacationDestinations' a uložte ho.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Nyní, pokud přejdete zpět na portál Azure, zjistíte, že máte jenom jednu kolekci s názvem ```alldata``` daty 'Family' a 'VacationDestinations'.

    ![Kurz k Node.js – snímek obrazovky portálu Azure znázorňující účet Azure Cosmos databáze s názvem kolekce zvýrazněná – databáze Node][mutiple-coll]

1. Všimněte si také, že každý objekt má jiný atribut říká ```__type```, které pomáhá rozlišit mezi dva modely jiný objekt.

1. Nakonec umožňuje číst data, která je uložená v Azure Cosmos DB. Mongoose postará filtrování dat založených na modelu. Ano budete muset udělat nic různých při čtení dat. Zadat pouze váš model (v tomto případě ```Family_common```) a zpracovává Mongoose filtrování 'DiscriminatorKey'.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Jak můžete vidět, je snadné pro práci s Mongoose discriminators. Ano Pokud máte aplikaci, která používá rozhraní Mongoose, v tomto kurzu je způsob, jak můžete ke zprovoznění aplikace a systémem MongoDB API v Azure Cosmos DB bez nutnosti příliš mnoho změn.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další postup

Další informace o MongoDB operace, operátory, fáze, příkazy a možnosti podporované Cosmos DB MongoDB rozhraní API služby Azure v [MongoDB API podpora pro funkce MongoDB a syntaxe](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png