---
title: "Použít Robomongo pro Azure Cosmos DB | Microsoft Docs"
description: "Další informace o použití Robomongo se Azure DB Cosmos: rozhraní API pro MongoDB účet"
keywords: robomongo
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
ms.openlocfilehash: 8983594776a1bbe413a6d7cf2cd518f0e327648a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Pomocí Azure DB Cosmos Robomongo: rozhraní API pro MongoDB účet
Pro připojení k databázi Azure Cosmos: rozhraní API pro MongoDB účtu pomocí Robomongo, musíte:

* Stáhněte a nainstalujte [Robomongo](https://robomongo.org/)
* Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace

## <a name="connect-using-robomongo"></a>Připojte se pomocí Robomongo
Přidání vaší Azure DB Cosmos: rozhraní API pro MongoDB účet pro připojení MongoDB Robomongo, proveďte následující kroky.

1. Načíst vaše Azure DB Cosmos: rozhraní API pro účet informace o připojení MongoDB pomocí pokynů [zde](connect-mongodb-account.md).

    ![Snímek obrazovky okna řetězec připojení](./media/mongodb-robomongo/connectionstringblade.png)
2. Spustit *Robomongo.exe*

3. Klikněte na tlačítko připojení v části **souboru** ke správě připojení. Potom klikněte na **vytvořit** v **MongoDB připojení** okno, které se otevře **nastavení připojení** okno.

4. V **nastavení připojení** okno, zvolte název. Najdete **hostitele** a **Port** z vašich informací připojení v kroku 1 a zadejte je do **adresu** a **Port**, v uvedeném pořadí.

    ![Snímek obrazovky Robomongo spravovat připojení](./media/mongodb-robomongo/manageconnections.png)
5. Na **ověřování** , klikněte na **provést ověřování**. Potom zadejte vaše databáze (výchozí hodnota je *správce*), **uživatelské jméno** a **heslo**.
Obě **uživatelské jméno** a **heslo** lze nalézt v informace o připojení v kroku 1.

    ![Snímek obrazovky s kartou ověřování Robomongo](./media/mongodb-robomongo/authentication.png)
6. Na **SSL** zkontrolujte **protokol SSL použití**, změňte **metodu ověřování** k **certifikát podepsaný svým držitelem**.

    ![Snímek obrazovky s kartou SSL Robomongo](./media/mongodb-robomongo/SSL.png)
7. Nakonec klikněte na **Test** a ověřte, zda lze pro připojení, potom **Uložit**.

## <a name="next-steps"></a>Další kroky
* Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).
