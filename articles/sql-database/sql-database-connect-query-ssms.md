---
title: "Připojení k SQL Database – SQL Server Management Studio | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit k SQL Database na Azure pomocí služby SQL Server Management Studio (SSMS). Potom spusťte ukázkový dotaz pomocí jazyka Transact-SQL (T-SQL)."
metacanonical: 
keywords: "Připojení k SQL Database, SQL Server Management Studio"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0eb25eb76c6c6c2446ac0b2b07c65975c3719db0


---
# <a name="connect-to-sql-database-with-sql-server-management-studio-and-execute-a-sample-tsql-query"></a>Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL
> [!div class="op_single_selector"]
> * [Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

Tento článek ukazuje, jak se připojit k Azure SQL Database pomocí služby SQL Server Management Studio (SSMS). Po úspěšném připojení spustíme jednoduchý dotaz jazyka Transact-SQL (T-SQL), abychom ověřili komunikaci s databází.

[!INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[!INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

## <a name="run-sample-queries"></a>Spuštění ukázkových dotazů
Po připojení k serveru se můžete připojit k databázi a spustit ukázkový dotaz. Pokud je pro vás psaní dotazů novinkou, přečtěte si téma [Psaní příkazů Transact-SQL](https://msdn.microsoft.com/library/ms365303.aspx).

1. V **Průzkumníku objektů** přejděte k databázi na serveru, například k ukázkové databázi **AdventureWorks**.
2. Klikněte na databázi pravým tlačítkem myši a pak klikněte na **Nový dotaz**:
   
    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)
3. Zkopírujte následující kód a vložte ho do okna dotazu:
   
        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;
4. Klikněte na tlačítko **Provést**:
   
    ![Úspěch Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## <a name="next-steps"></a>Další kroky
Pomocí příkazů T-SQL můžete vytvářet a spravovat databáze v Azure v podstatě stejným způsobem jako pomocí SQL Serveru. Pokud umíte používat T-SQL s SQL Serverem, najdete v tématu [Informace o příkazech jazyka Transact-SQL služby Azure SQL Database)](sql-database-transact-sql-information.md) souhrnné informace o rozdílech.

Pokud s T-SQL teprve začínáte, projděte si [Kurz: Psaní příkazů Transact-SQL](https://msdn.microsoft.com/library/ms365303.aspx) a [Referenční informace k Transact-SQL (databázový stroj)](https://msdn.microsoft.com/library/bb510741.aspx).

Informace o tom, jak začít s vytvářením uživatelů a správců databází, najdete v tématu [Začínáme se zabezpečení služby Azure SQL Database](sql-database-get-started-security.md).

Další informace o SSMS najdete v tématu [Použití SQL Server Management Studia](https://msdn.microsoft.com/library/ms174173.aspx).




<!--HONumber=Nov16_HO2-->


