---
title: "Správa účtu Azure Cosmos DB prostřednictvím portálu Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat váš účet Azure Cosmos DB prostřednictvím portálu Azure. Najít průvodce na pomocí portálu Azure k zobrazení, kopírování, odstranění a přístup k účtům."
keywords: "Portál Azure, azure documentdb, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Správa účtu Azure Cosmos DB
Zjistěte, jak nastavit globální konzistence, práce s klíči a odstranit účet Azure Cosmos DB na portálu Azure.

## <a id="consistency"></a>Spravovat nastavení konzistence Azure Cosmos DB
Výběr správné konzistence úroveň závisí na sémantiku vaší aplikace. Byste si měli přečíst s úrovní konzistence k dispozici v Azure Cosmos DB načtením [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu v Azure Cosmos DB][consistency]. Azure Cosmos DB poskytuje konzistence, dostupnosti a výkonu záruky, na všech úrovních konzistence k dispozici pro váš účet databáze. Konfigurace účtu databáze s úrovní konzistence silným vyžaduje, aby vaše data uzavřeného k jedné oblasti Azure a globálně dostupnou. Na druhé straně, úrovně volný konzistence - typu s ohraničenou prošlostí, session nebo případné povolení k přidružení libovolný počet oblastí Azure s vaším účtem databáze. Jednoduché následující kroky ukazují, jak vybrat výchozí úroveň konzistence pro váš účet databáze. 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>Chcete-li určit výchozí konzistence účtu Azure Cosmos DB
1. V [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.
2. V okně účtu klikněte na **výchozí konzistence**.
3. V **výchozí konzistence** okně, vyberte novou úroveň konzistence a klikněte na **Uložit**.
    ![Výchozí konzistence relace][5]

## <a id="keys"></a>Zobrazení, kopírování a opětovné vytváření přístupových klíčů
Když vytvoříte účet Azure Cosmos DB, generuje tato služba dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB. Poskytnutím dvou přístupových klíčů Azure Cosmos DB umožňuje znovu vygenerovat klíče bez přerušení ke svému účtu Azure Cosmos DB. 

V [portál Azure](https://portal.azure.com/), přístup **klíče** okno na v nabídce prostředků **účet Azure Cosmos DB** okna zobrazení, kopírování a opětovné vygenerování přístupových klíčů, které se používají pro přístup k účtu Azure Cosmos DB.

![Azure Portal snímku obrazovky okna klíče](./media/manage-account/keys.png)

> [!NOTE]
> **Klíče** okno také obsahuje primární a sekundární připojovací řetězce, které lze použít pro připojení k účtu z [nástroj pro migraci dat](import-data.md).
> 
> 

Klíče jen pro čtení jsou také k dispozici v tomto okně. Čtení a dotazy jsou jen pro čtení operace, než vytvoří, odstranění, a nahradí nejsou.

### <a name="copy-an-access-key-in-the-azure-portal"></a>Zkopírovat přístupový klíč na portálu Azure
Na **klíče** okně klikněte na tlačítko **kopie** tlačítku klíče, které chcete kopírovat.

![Zobrazení a zkopírování přístupového klíče na portálu Azure, okna klíče](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Opětovné vygenerování přístupových klíčů
Měli byste změnit přístupové klíče k účtu Azure Cosmos DB pravidelně pro zvýšení zabezpečení připojení. Abyste mohli udržovat připojení k účtu Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč jsou přiřazeny dva přístupové klíče.

> [!WARNING]
> Opětovné generování přístupových klíčů ovlivní všechny aplikace, které jsou závislé na aktuální klíč. Všichni klienti, kteří používají přístupový klíč pro přístup k účtu Azure Cosmos DB musí být aktualizovány na používání nového klíče.
> 
> 

Pokud máte aplikace nebo cloudové služby, pomocí účtu Azure Cosmos DB, ztratíte připojení Pokud obnovit klíče, klíče nezaregistrujete. Následující kroky popisují proces účastní vrácení klíče.

1. Aktualizujte přístupový klíč v kódu aplikace tak, aby odkazovaly sekundární přístupový klíč účtu Azure Cosmos DB.
2. Znovu vygenerujte primární přístupový klíč pro účet Azure Cosmos DB. V [portálu Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.
3. V **Azure Cosmos DB účet** okně klikněte na tlačítko **klíče**.
4. Na **klíče** okně klikněte na tlačítko znovu generovat a pak klikněte na **Ok** potvrďte, že chcete vygenerovat nový klíč.
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-keys.png)
5. Jakmile si ověříte, že nový klíč je k dispozici pro použití (přibližně 5 minut po opětovné generování), aktualizujte přístupový klíč v kódu aplikace tak, aby odkazovaly nový primární přístupový klíč.
6. Stejným způsobem pak opětovně vygenerujte sekundární přístupový klíč.
   
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Může trvat několik minut, než nově vygenerovaný klíč můžete použít pro přístup k účtu Azure Cosmos DB.
> 
> 

## <a name="get-the--connection-string"></a>Získat připojovací řetězec
Pokud chcete načíst připojovací řetězec, postupujte takto: 

1. V [portál Azure](https://portal.azure.com), přístup k účtu Azure Cosmos DB.
2. V nabídce prostředků, klikněte na tlačítko **klíče**.
3. Klikněte **kopie** vedle položky **primární připojovací řetězec** nebo **sekundární připojovací řetězec** pole. 

Pokud používáte připojovací řetězec [nástroj pro migraci databáze Azure Cosmos DB](import-data.md), připojte na konec připojovací řetězec název databáze. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Odstranit účet Azure Cosmos DB
Chcete-li odebrat účet pro Azure Cosmos DB z portálu Azure, který už nepoužíváte, klikněte pravým tlačítkem na název účtu a klikněte na **odstranit účet**.

![Postup odstranění účtu Azure Cosmos DB na portálu Azure](./media/manage-account/deleteaccount.png)

1. V [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB chcete odstranit.
2. Na **účet Azure Cosmos DB** , klikněte pravým tlačítkem na účet a pak klikněte na tlačítko **odstranit účet**. 
3. V okně výsledné potvrzení zadejte název účtu Azure Cosmos DB potvrďte, že chcete odstranit účet.
4. Klikněte **odstranit** tlačítko.

![Postup odstranění účtu Azure Cosmos DB na portálu Azure](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Další kroky
Zjistěte, jak [začít pracovat s vaším účtem Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
