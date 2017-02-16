---
title: "Azure Portal: Pravidla brány firewall služby Azure SQL Database na úrovni serveru | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí webu Azure Portal nakonfigurovat pravidla brány firewall na úrovni serveru pro IP adresy, ze kterých se přistupuje k Azure SQL serveru."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 11/28/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2a85b3dc1078bad9e5e2fc0ce0bec7e994b29150
ms.openlocfilehash: 36938e642853d587cd28405f5e518ac9ac0697ac


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Vytvoření a správa pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 

Pravidla brány firewall na úrovni serveru umožňují správcům přístup k serveru služby SQL Database ze zadané IP adresy nebo rozsahu IP adres. Pravidla brány firewall na úrovni serveru můžete použít také pro uživatele, pokud máte mnoho databází se stejnými požadavky na přístup a nechcete ztrácet čas konfigurací jednotlivých databází. Společnost Microsoft doporučuje pro zvýšení zabezpečení a přenositelnosti databází používat pravidla brány firewall na úrovni databáze kdykoli je to možné. Přehled bran firewall služby SQL Database najdete v tématu [Přehled pravidel brány firewall služby SQL Database](sql-database-firewall-configure.md).

> [!Note]
> Informace o přenosných databázích v kontextu kontinuity podnikových procesů najdete v tématu [Požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Správa stávajících pravidel brány firewall na úrovni serveru na webu Azure Portal
Opakováním těchto kroků můžete spravovat pravidla brány firewall na úrovni serveru.

* Pokud chcete přidat aktuální počítač, klikněte na Přidat IP adresu klienta.
* Pro přidání dalších IP adres zadejte Název pravidla, Počáteční IP adresu a Koncovou IP adresu.
* Pokud chcete upravit stávající pravidlo, klikněte na libovolné pole pravidla a upravte ho.
* Pokud chcete odstranit stávající pravidlo, najeďte na něj myší a vyčkejte, než se na konci řádku objeví symbol X. Kliknutím na symbol X pravidlo odeberte.

Kliknutím na **Uložit** uložte změny.

## <a name="next-steps"></a>Další kroky

- Úvodní kurz najdete v článku [Kurz k SQL Database: Vytvoření serveru, pravidla brány firewall na úrovni serveru, ukázkové databáze, pravidla brány firewall na úrovni databáze a připojení k SQL Serveru](sql-database-get-started.md).
- Úvodní kurz k zabezpečení najdete v článku [Začínáme se zabezpečením](sql-database-control-access-sql-authentication-get-started.md).
- S připojováním k databázi SQL Azure z open source aplikací nebo aplikací třetích stran vám pomůžou [Ukázky kódu pro rychlý start klientů se službou SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Pochopit postup vytvoření dalších uživatelů, kteří se můžou připojovat k databázím, vám pomůže článek [Ověřování a autorizace ve službě SQL Database: Udělení přístupu](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Další zdroje
* [Zabezpečení databáze](sql-database-security-overview.md)   
* [Security Center pro databázový stroj SQL Server a Azure SQL Database](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Jan17_HO3-->


