---
title: "Správa Azure Cosmos DB v Azure Storage Explorer"
description: "Naučte se spravovat Azure DB Cosmos v Azure Storage Explorer."
Keywords: Azure Cosmos DB, Azure Storage Explorer, DocumentDB, MongoDB, DocumentDB
services: cosmos-db
documentationcenter: 
author: Jiaj-Li
manager: omafnan
editor: 
tags: Azure Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: Jiaj-Li
ms.openlocfilehash: e695cdd7c51e18a386764ab8444d3336366ae265
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Správa Azure Cosmos DB v Azure Storage Explorer (Preview)

Použití Azure Cosmos DB v Azure Storage Explorer umožňuje uživatelům spravovat Azure Cosmos DB entity, pracovat s daty, aktualizace uložených procedur a aktivačních událostí společně s dalšími subjekty Azure jako úložiště objektů BLOB a fronty. Nyní můžete na stejný nástroj ke správě vašeho různých Azure entit na jednom místě. V současné době podporuje Azure Storage Explorer SQL (DocumentDB) a účty MongoDB.

V tomto článku můžete další informace o použití Průzkumníka úložiště ke správě Azure Cosmos DB.


## <a name="prerequisites"></a>Požadavky

Účet Azure Cosmos DB pro SQL (DocumentDB) nebo databázi MongoDB. Pokud nemáte účet, můžete vytvořit jeden na portálu Azure jak je popsáno v [Cosmos databázi Azure: vytvoření webové aplikace DocumentDB rozhraní API pomocí rozhraní .NET a portálu Azure](create-documentdb-dotnet.md).

## <a name="installation"></a>Instalace

Nainstalujte nejnovější Azure Storage Explorer bits zde: [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), teď podporujeme verze systému Windows, Linuxu a MACU.

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatnému Azure

1. Po instalaci **Azure Storage Explorer**, klikněte **modulu plug-in** ikonu na levé straně, jak je znázorněno na následujícím obrázku.
       
   ![Zařadit ikonu](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/plug-in-icon.png)
 
2. Vyberte **přidat účet Azure**a potom klikněte na **přihlášení**.

   ![Připojení k předplatnému Azure](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-azure-subscription.png)

2. V **Azure přihlášení** dialogové okno, vyberte **přihlášení**a pak zadejte přihlašovací údaje Azure.

    ![Přihlášení](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/sign-in.png)

3. Zvolte předplatné ze seznamu a pak klikněte na **použít**.

    ![Použít](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/apply-subscription.png)

    V podokně Průzkumník aktualizuje a zobrazí účty ve vybraném předplatném.

    ![Seznamu účtů](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/account-list.png)

    Úspěšně připojíte k vaší **účet Azure Cosmos DB** k předplatnému Azure.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Připojte se k Azure Cosmos databázi pomocí připojovacího řetězce

Alternativní způsob připojení k databázi Cosmos Azure je použití připojovací řetězec. Pomocí následujících kroků pro připojení pomocí připojovacího řetězce.

1. Najít **místní a připojené** ve stromu vlevo, klikněte pravým tlačítkem na **Azure Cosmos DB účty**, zvolte **připojit k databázi Cosmos Azure...**

    ![Připojte se k Azure Cosmos databázi pomocí připojovacího řetězce](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-db-by-connection-string.png)

2. Zvolte odpovídající **výchozí prostředí** pro typ vašeho účtu buď **DocumentDB** nebo **MongoDB**, vložte vaší **připojovací řetězec**a potom klikněte na **OK** pro připojení účet Azure Cosmos DB. Informace o načítání připojovací řetězec najdete v tématu [získat připojovací řetězec](https://docs.microsoft.com/en-us/azure/cosmos-db/manage-account#get-the--connection-string).

    ![připojovací řetězec](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Správa prostředků Azure Cosmos DB

Účet Azure Cosmos DB lze spravovat nástrojem následující operace:
* Otevřete účet na portálu Azure
* Daný prostředek přidejte do seznamu rychlý přístup
* Hledání a aktualizace prostředků
* Vytvářet a odstraňovat databáze
* Vytvářet a odstraňovat kolekce
* Vytvořit, upravit, odstranit a filtrovat dokumenty
* Spravovat uložené procedury, triggery a uživatelem definované funkce

### <a name="quick-access-tasks"></a>Úlohy rychlý přístup

Kliknutím pravým tlačítkem na předplatné v podokně Průzkumník, můžete provádět mnoho rychlé akce úlohy:

* Klikněte pravým tlačítkem na účet Azure Cosmos databáze nebo databáze, můžete zvolit **otevřít na portálu** a správě prostředků v prohlížeči na portálu Azure.

     ![Otevřít na portálu](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/open-in-portal.png)

* Můžete také přidat účet Azure Cosmos DB, databáze, kolekce **rychlý přístup**.
* **Hledání odsud** umožňuje vyhledávat – klíčové slovo v rámci vybrané cesty.

    ![vyhledávání z tohoto umístění](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Správa databáze a kolekce.
#### <a name="create-a-database"></a>Vytvoření databáze 
Klikněte pravým tlačítkem na účet Azure Cosmos DB, zvolte **Create Database**, vstupní název databáze a stiskněte klávesu **Enter** k dokončení.

![Vytvoření databáze](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Odstranění databáze
Klikněte pravým tlačítkem na databázi, klikněte na tlačítko **odstranit databázi**a klikněte na tlačítko **Ano** v místním okně. Uzel databáze se odstraní a účet Azure Cosmos DB aktualizuje automaticky.

![Odstranit database1](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database1.png)  

![Odstranit database2](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Vytvoření kolekce
Klikněte pravým tlačítkem na databázi, zvolte **vytvořit kolekci**a potom zadejte následující informace jako **ID kolekce**, **kapacity úložiště**atd. Kliknutím na **OK** vytváření dokončete. Informace o nastavení klíče oddílu najdete v tématu [návrhu pro dělení](partition-data.md#designing-for-partitioning).

Pokud klíč oddílu se používá při vytváření kolekce, po dokončení vytváření, nelze změnit hodnotu klíče oddílu v kolekci.

![Vytvoření collection1](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection.png)

![Vytvoření collection2](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Odstranit kolekci
Klikněte pravým tlačítkem na kolekci, klikněte na tlačítko **odstranit kolekci**a potom klikněte na **Ano** v místním okně. 

Uzel kolekce je Odstraněná a databáze se aktualizuje automaticky.  

![Odstranit kolekci](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Správa dokumentů

#### <a name="create-and-modify-documents"></a>Vytvářet a upravovat dokumenty
Chcete-li vytvořit nový dokument, otevřete **dokumenty** v levém okně klikněte na tlačítko **nový dokument**, upravit obsah v pravém podokně a pak klikněte na tlačítko **Uložit**. Můžete také aktualizovat stávající dokument a pak klikněte na tlačítko **Uložit**. Změny můžete zrušeny kliknutím **zahodit**.

![Dokument](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/document.png)

#### <a name="delete-a-document"></a>Odstranění dokumentu
Klikněte **odstranit** tlačítko Odstranit vybraný dokument.
#### <a name="query-for-documents"></a>Dotaz pro dokumenty
Upravit filtr dokumentu zadáním [dotazu SQL](documentdb-sql-query.md) a pak klikněte na **použít**.

![Filtr](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Spravovat uložené procedury, triggery a UDF
* Pokud chcete vytvořit uložené procedury, v levém stromu, klikněte pravým tlačítkem na **uloženou proceduru**, zvolte **vytvořit uloženou proceduru**, zadejte název v levé, zadejte skripty uložené procedury v pravém podokně okna a potom Klikněte na tlačítko **vytvořit**. 
* Můžete také upravit existující uložené procedury dvakrát kliknete na soubor, provedení aktualizace a pak kliknutím na **aktualizace** uložíte nebo kliknutím na **zahodit** zrušit změny.

![Uložené procedury](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/stored-procedure.png)

* Operace pro **aktivační události** a **UDF** jsou podobná těm pro **uložené procedury**.

## <a name="next-steps"></a>Další kroky

* V následujícím videu v tématu Jak používat Azure Cosmos DB v Azure Storage Explorer: [použít Azure Cosmos DB v Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Další informace o Storage Explorer a připojte další služby v [Začínáme se Storage Explorerem (Preview)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer).

