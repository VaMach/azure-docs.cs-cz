---
title: "Navrhnout první databázi SQL Azure - C# | Microsoft Docs"
description: "Naučte se navrhnout první databáze Azure SQL a k nim připojit pomocí programu v C# pomocí ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 01/12/2018
ms.author: genemi
ms.openlocfilehash: d99456b488302c9b3fa44510af84641876861afe
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/13/2018
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Návrh Azure SQL database a připojení s C & #x23; a ADO.NET

Databáze SQL Azure je relační databáze jako a služba (DBaaS) v Microsoft cloudu (Azure). V tomto kurzu můžete další informace o použití portálu Azure a ADO.NET pomocí sady Visual Studio: 

> [!div class="checklist"]
> * Vytvoření databáze na portálu Azure
> * Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure
> * Připojení k databázi s ADO.NET a Visual Studio
> * Vytváření tabulek s ADO.NET
> * Vložit, aktualizovat a odstranit data s ADO.NET 
> * Dotaz na data ADO.NET

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Další postup

V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit databázi do předchozího bodu v čase. Naučili jste se tyto postupy:
> [!div class="checklist"]
> * Vytvoření databáze
> * Nastavit pravidlo brány firewall
> * Připojení k databázi s [Visual Studio a C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Vytváření tabulek
> * Vložit, aktualizovat a odstranit data
> * Dotazování dat

Přechodu na další informace o migraci dat v dalším kurzu.

> [!div class="nextstepaction"]
>[Migrovat databázi SQL serveru do Azure SQL Database](sql-database-migrate-your-sql-server-database.md)

