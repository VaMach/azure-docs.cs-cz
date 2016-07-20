<properties
  pageTitle="Kurz k NoSQL Node.js pro DocumentDB | Microsoft Azure"
  description="Kurz k NoSQL Node.js, v rámci kterého se vytváří databáze Node a konzolová aplikace pomocí sady DocumentDB Node.js SDK DocumentDB je databáze NoSQL pro JSON."
    keywords="node.js tutorial, node database"
  services="documentdb"
  documentationCenter="node.js"
  authors="AndrewHoh"
  manager="jhubbard"
  editor="monicar"/>

<tags
  ms.service="documentdb"
  ms.workload="data-services"
  ms.tgt_pltfrm="na"
  ms.devlang="node"
  ms.topic="hero-article"
  ms.date="04/26/2016"
  ms.author="anhoh"/>

# Kurz k NoSQL Node.js: Konzolová aplikace DocumentDB Node.js  

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

Vítejte v kurzu k Node.js pro sadu DocumentDB Node.js SDK! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky DocumentDB, včetně databáze Node, a dotazuje se na ně.

Budeme se zabývat těmito tématy:

- Vytvoření a připojení k účtu DocumentDB
- Nastavení aplikace
- Vytvoření databáze Node
- Vytvoření kolekce
- Vytvoření dokumentů JSON
- Dotazování na kolekci
- Nahrazení dokumentu
- Odstranění dokumentu
- Odstranění databáze Node

Nemáte čas? Nevadí! Úplné řešení je k dispozici na [GitHubu](https://github.com/Azure-Samples/documentdb-node-getting-started). Rychlé pokyny najdete v části [Získání úplného řešení](#GetSolution).

Až tento kurz Node.js dokončíte, řekněte nám prosím svůj názor pomocí hlasovacích tlačítek v horní a dolní části této stránky. Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.

Můžeme začít!

## Předpoklady pro kurz k Node.js 

Ujistěte se prosím, že máte následující:

- Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
- [Node.js](https://nodejs.org/) verze 0.10.29 nebo vyšší

## Krok 1: Vytvoření účtu DocumentDB

Vytvořme účet DocumentDB. Pokud již máte účet, který chcete použít, můžete přeskočit na [Nastavení aplikace Node.js](#SetupNode).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

##<a id="SetupNode"></a> Krok 2: Nastavení aplikace Node.js

1. Otevřete svůj oblíbený terminál.
2. Vyhledejte složku nebo adresář, do kterého chcete uložit aplikaci Node.js.
3. Vytvořte dva prázdné javascriptové soubory, které budou obsahovat následující příkazy:
  - Windows:
      * ```fsutil file createnew app.js 0```
        * ```fsutil file createnew config.js 0```
  - Linux nebo OS X:
      * ```touch app.js```
        * ```touch config.js```
4. Přes npm nainstalujte modul documentdb. Použijte následující příkaz:
    * ```npm install documentdb --save```

Výborně! Teď když jste dokončili nastavování, napišme nějaký kód.

##<a id="Config"></a> Krok 3: Nastavení konfigurací aplikace

Ve svém oblíbeném textovém editoru otevřete ```config.js```.

Pak zkopírujte a vložte fragment kódu níže a nastavte vlastnosti ```config.endpoint``` a ```config.primaryKey``` na hodnoty identifikátoru URI a primárního klíče vašeho koncového bodu DocumentDB. Obě tyto konfigurace je možné najít na [Portálu Azure](https://portal.azure.com).

![Kurz k Node.js – snímek obrazovky Portálu Azure znázorňující účet DocumentDB se zvýrazněným aktivním centrem, zvýrazněným tlačítkem Klíče v okně účtu DocumentDB a zvýrazněnými hodnotami URI, PRIMÁRNÍ KLÍČ a SEKUNDÁRNÍ KLÍČ v okně Klíče – databáze Node][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Zkopírujte ```database id```, ```collection id``` a ```JSON documents``` a vložte je do objektu ```config``` níže, kde nastavíte vlastnosti ```config.endpoint``` a ```config.authKey```. Pokud již máte data, která chcete uložit do databáze, můžete místo přidávání definic dokumentů použít [nástroj pro migraci dat](documentdb-import-data.md) DocumentDB.

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Databáze, kolekce a definice dokumentů se budou chovat jako parametry ```database id``` a ```collection id``` DocumentDB a data dokumentů.

Nakonec exportujte objekt ```config```, abyste na něj mohli odkazovat ze souboru ```app.js```.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

##<a id="Connect"></a> Krok 4: Připojení k účtu DocumentDB

V textovém editoru otevřete prázdný soubor ```app.js```. Zkopírováním a vložením kódu níže importujte modul ```documentdb``` a nově vytvořený modul ```config```.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Zkopírujte a vložte kód, který použije dříve uložené ```config.endpoint``` a ```config.primaryKey``` k vytvoření nové instance DocumentClient.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Nyní, když máte kód pro inicializaci klienta documentdb, se budeme věnovat práci s prostředky DocumentDB.

## Krok 5: Vytvoření databáze Node
Zkopírujte a vložte kód níže, který nastaví stav HTTP pro odpověď Not Found (Nenalezeno), URL databáze a URL kolekce. Pomocí těchto adres URL klient DocumentDB najde správnou databázi a kolekci.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

[Databázi](documentdb-resources.md#databases) je možné vytvořit pomocí funkce [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**. Databáze je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi.

Pro vytvoření nové databáze v souboru app.js s ```id``` zadaným v objektu ```config``` zkopírujte a vložte funkci **getDatabase**. Funkce zkontroluje, jestli již neexistuje databáze se stejným ID ```FamilyRegistry```. Pokud ano, místo vytvoření nové databáze se vrátí databáze s tímto ID.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Zkopírujte a vložte kód níže, kde za funkci **getDatabase** přidáte pomocnou funkci **exit**. Ta vypíše závěrečnou zprávu a zavolá funkci **getDatabase**.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste vytvořili databázi DocumentDB.

##<a id="CreateColl"></a>Krok 6: Vytvoření kolekce  

> [AZURE.WARNING] **CreateDocumentCollectionAsync** vytvoří novou kolekci, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).

[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí funkce [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.

Pro vytvoření nové kolekce s ```id``` zadaným v objektu ```config``` zkopírujte a vložte funkci **getCollection** pod funkci **getDatabase**. Opět zkontrolujeme, že kolekce se stejným ID ```FamilyCollection``` ještě neexistuje. Pokud existuje, místo vytvoření nové kolekce se vrátí kolekce s tímto ID.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Zkopírujte a vložte kód pod voláním funkce **getDatabase**, aby se spustila funkce **getCollection**.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste vytvořili kolekci DocumentDB.

##<a id="CreateDoc"></a>Krok 7: Vytvoření dokumentu
[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit dokument do DocumentDB.

Pod funkci **getCollection** zkopírujte a vložte funkci **getFamilyDocument**, která bude vytvářet dokumenty obsahující data JSON uložené v objektu ```config```. Opět zkontrolujeme, že dokument se stejným ID ještě neexistuje.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód pod voláním funkce **getCollection**, aby se spustila funkce **getFamilyDocument**.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste vytvořili dokument DocumentDB.

![Kurz k Node.js – diagram ilustrující hierarchický vztah mezi účtem, databází, kolekcí a dokumenty – databáze Node](./media/documentdb-nodejs-get-started/node-js-tutorial-account-database.png)

##<a id="Query"></a>Krok 8: Dotazování prostředků DocumentDB

DocumentDB podporuje [bohaté dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí. Následující ukázka kódu obsahuje dotaz, který je možné spustit proti dokumentům v kolekci.

Zkopírujte funkci **queryCollection** a vložte ji pod funkci **getFamilyDocument**. Jak je vidět níže, DocumentDB podporuje dotazy podobné jazyku SQL. Další informace o vytváření komplexních dotazů najdete v [Query Playground](https://www.documentdb.com/sql/demo) a [dokumentaci k dotazům](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Následující diagram ilustruje, jak se volá syntaxe dotazu DocumentDB SQL proti kolekci, kterou jste vytvořili.

![Kurz k Node.js – diagram znázorňující obor a význam dotazu – databáze Node](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Klíčové slovo [FROM](documentdb-sql-query.md#from-clause) je v dotazu volitelné, protože dotazy DocumentDB již mají obor nastaven na jedinou kolekci. Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte. DocumentDB standardně vyvodí, že Families, root nebo zvolený název proměnné odkazují na aktuální kolekci.

Zkopírujte a vložte kód pod voláním funkce **getFamilyDocument**, aby se spustila funkce **queryCollection**.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste se dotázali na dokument DocumentDB.

##<a id="ReplaceDocument"></a>Krok 9: Nahrazení dokumentu
DocumentDB podporuje nahrazování dokumentů JSON.

Zkopírujte funkci **replaceDocument** a vložte ji pod funkci **queryCollection**.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód pod voláním funkce **queryCollection**, aby se spustila funkce **replaceDocument**. Také znovu přidejte kód volání **queryCollection**, aby bylo možné ověřit, že se dokument úspěšně změnil.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste nahradili dokument DocumentDB.

##<a id="DeleteDocument"></a>Krok 10: Odstranění dokumentu
DocumentDB podporuje odstraňování dokumentů JSON.

Zkopírujte funkci **deleteDocument** a vložte ji pod funkci **replaceDocument**.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód pod druhým voláním funkce **queryCollection**, aby se spustila funkce **deleteDocument**.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Blahopřejeme! Úspěšně jste odstranili dokument DocumentDB.

##<a id="DeleteDatabase"></a>Krok 11: Odstranění databáze Node

Odstraněním vytvořené databáze dojde k odstranění databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

Zkopírujte a vložte následující fragment kódu (funkce **cleanup**), který odstraní databázi a všechny podřízené prostředky.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Zkopírujte a vložte kód pod voláním funkce **deleteDocument**, aby se spustila funkce **cleanup**.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

##<a id="Run"></a>Krok 12: Spuštění celé aplikace Node.js!

Jako celek by pořadí volání funkcí mělo vypadat takto:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```

Měl by se zobrazit výstup počáteční aplikace. Výstup by měl odpovídat ukázkovému textu níže.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Blahopřejeme! Dokončili jste kurz k Node.js a máte svou první konzolovou aplikaci DocumentDB!

##<a id="GetSolution"></a> Získání úplného řešení kurzu k Node.js
Abyste mohli sestavit řešení GetStarted, které obsahuje všechny ukázky tohoto článku, budete potřebovat následující:

-   [Účet DocumentDB][documentdb-create-account]
-   Řešení [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) dostupné na GitHubu

Přes npm nainstalujte modul **documentdb**. Použijte následující příkaz:
* ```npm install documentdb --save```

Dále v souboru ```config.js``` aktualizujte hodnoty config.endpoint a config.authKey, jak je popsáno v části [Krok 3: Nastavení konfigurací aplikace](#Config).

## Další kroky

-   Hledáte složitější ukázku Node.js? Přečtěte si [Vytvoření webové aplikace Node.js pomocí DocumentDB](documentdb-nodejs-application.md).
-  Naučte se [monitorovat účet DocumentDB](documentdb-monitor-accounts.md).
-  Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).
-  Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md

[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png



<!--HONumber=Jun16_HO2-->


