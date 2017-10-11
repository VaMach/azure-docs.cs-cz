---
title: "Transparentní šifrování dat v SQL Data Warehouse (portál) | Microsoft Docs"
description: "Transparentní šifrování dat (šifrování TDE) v SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: b1db3bdfdfb54bda325c9b971cfcb4dd5efa333a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Začínáme s transparentní dat šifrování (TDE) v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled zabezpečení](sql-data-warehouse-overview-manage-security.md)
> * [Ověřování](sql-data-warehouse-authentication.md)
> * [Šifrování (portál)](sql-data-warehouse-encryption-tde.md)
> * [Šifrování (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Požadovaná oprávnění
Pokud chcete povolit šifrování transparentní dat (TDE), musíte být správce nebo člen dbmanager role.

## <a name="enabling-encryption"></a>Povolení šifrování
Pokud chcete povolit šifrování TDE pro SQL Data Warehouse, postupujte podle následujících kroků:

1. Otevřít v databázi [portálu Azure](https://portal.azure.com)
2. V okně databáze klikněte na **nastavení** tlačítko
3. Vyberte **transparentní šifrování dat** možnost![][1]
4. Vyberte **na** nastavení![][2]
5. Vyberte **uložit**
   ![][3]  

## <a name="disabling-encryption"></a>Zakázáním šifrování
Zakázat šifrování TDE pro SQL Data Warehouse, postupujte podle následujících kroků:

1. Otevřít v databázi [portálu Azure](https://portal.azure.com)
2. V okně databáze klikněte na **nastavení** tlačítko
3. Vyberte **transparentní šifrování dat** možnost![][1]
4. Vyberte **vypnout** nastavení![][4]
5. Vyberte **uložit**
   ![][5]  

## <a name="encryption-dmvs"></a>Šifrování zobrazení dynamické správy
Šifrování lze potvrdit s následující zobrazení dynamické správy:

* [zobrazení Sys.Databases]
* [Sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[zobrazení Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
