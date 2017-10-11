---
title: "Použít MongoChef pro Azure Cosmos DB | Microsoft Docs"
description: "Další informace o použití MongoChef se Azure DB Cosmos: rozhraní API pro MongoDB účet"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Pomocí Azure DB Cosmos MongoChef: rozhraní API pro MongoDB účet

Pro připojení k databázi Azure Cosmos: rozhraní API pro MongoDB účet, musíte:

* Stáhněte a nainstalujte [MongoChef](http://3t.io/mongochef)
* Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace

## <a name="create-the-connection-in-mongochef"></a>Vytvoření připojení v MongoChef
Přidání vaší Azure DB Cosmos: rozhraní API pro MongoDB účet pro připojení správce MongoChef, proveďte následující kroky.

1. Načíst vaše Azure DB Cosmos: rozhraní API pro informace o připojení MongoDB pomocí pokynů [zde](connect-mongodb-account.md).

    ![Snímek obrazovky okna řetězec připojení](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klikněte na tlačítko **připojit** a otevřít Správce připojení, potom klikněte na **nové připojení**

    ![Snímek obrazovky Správce připojení MongoChef](./media/mongodb-mongochef/ConnectionManager.png)
3. V **nové připojení** okno na **Server** zadejte hostitele (FQDN) systému Cosmos databázi Azure: rozhraní API pro MongoDB účet a PORT.

    ![Snímek obrazovky na kartu server manager MongoChef připojení](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. V **nové připojení** okno na **ověřování** , zvolte režim ověřování **Standard (MONGODB CR nebo SCARM-SHA-1)** a zadejte uživatelské jméno a heslo.  Přijměte výchozí ověřování databáze (správce) nebo zadejte vlastní hodnotu.

    ![Snímek obrazovky s kartou ověřování MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. V **nové připojení** okno na **SSL** zkontrolujte **protokol SSL používá pro připojení** zaškrtávací políčko a **přijmout SSL certifikáty podepsané svým držitelem serveru**  přepínač.

    ![Snímek obrazovky s kartou SSL MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klikněte **Test připojení** a ověřte informace o připojení, klikněte na tlačítko **OK** vraťte do okna nové připojení, a pak klikněte na **Uložit**.

    ![Snímek obrazovky okna MongoChef test připojení](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Použít MongoChef k vytvoření databáze, kolekce a dokumentů
K vytvoření databáze, kolekce a dokumenty pomocí MongoChef, proveďte následující kroky.

1. V **Správce připojení**, zvýrazněte připojení a klikněte na **Connect**.

    ![Snímek obrazovky Správce připojení MongoChef](./media/mongodb-mongochef/ConnectToAccount.png)
2. Klikněte pravým tlačítkem na hostitele a zvolte **přidat databázi**.  Zadejte název databáze a klikněte na tlačítko **OK**.

    ![Snímek obrazovky možnost MongoChef přidat databáze](./media/mongodb-mongochef/AddDatabase1.png)
3. Klikněte pravým tlačítkem na databázi a zvolte **přidat kolekce**.  Zadejte název kolekce a klikněte na **vytvořit**.

    ![Snímek obrazovky možnost MongoChef přidat kolekce](./media/mongodb-mongochef/AddCollection.png)
4. Klikněte na tlačítko **kolekce** nabídky položky, klepněte na tlačítko **přidat dokument**.

    ![Snímek obrazovky MongoChef přidat dokument položky nabídky](./media/mongodb-mongochef/AddDocument1.png)
5. V dialogovém okně Přidat dokument vložte následující a potom klikněte na **přidat dokument**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Přidání jiného dokumentu, tentokrát s následujícím obsahem.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Spustit ukázkový dotaz. Například vyhledejte rodiny s příjmení 'rodinu a vrátí nadřazené položky a pole stavu.

    ![Snímek obrazovky Mongo Chef výsledky dotazu](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Další kroky
* Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).
