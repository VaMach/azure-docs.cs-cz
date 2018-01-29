---
title: "Přehled požadavků na používání služby Azure databáze migrace | Microsoft Docs"
description: "Další informace o přehled požadavků na používání služby Azure databáze migrace k provedení migrace databáze."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: a103bb4802bc4a20b6d82f0c6bb427133d9e5643
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Přehled požadavků na používání služby Azure databáze migrace
Existuje několik předpokladů, vyžaduje se pro zajištění, že služba migrace databáze Azure běží bez problémů při provádění migrace databáze. Některé požadované součásti aplikaci ve všech scénářích (cílový Zdroj páry) podporována službou, zatímco jiné požadavky, které jsou jedinečné pro konkrétní scénář.

Požadavky související s používáním služby Azure databáze migrace jsou uvedené v následujících částech.

## <a name="prerequisites-common-across-migration-scenarios"></a>Požadavky, které jsou společné napříč scénáře migrace
Požadavky Azure služba migrace databáze, které jsou společné pro všechny podporované scénáře migrace patří potřeba:
- Vytvoření virtuální sítě pro službu Azure databáze migrace pomocí modelu nasazení Azure Resource Manager, které poskytuje připojení site-to-site k vaší místní zdrojové servery pomocí [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) nebo [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Ujistěte se, že vaše Azure virtuální síť (VNET) skupinu zabezpečení sítě pravidla proveďte bloku následující komunikace porty 443, 53, 9354, 445, 12000. Další podrobnosti o filtrování provozu NSG virtuální síť Azure, najdete v článku [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Pokud používáte zařízení brány firewall před vaší zdrojové databáze, musíte přidat pravidla firewallu povolující službu Azure databáze migrace pro přístup k databází zdroje pro migraci.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Požadavky pro migraci systému SQL Server do Azure SQL Database 
Kromě předpokladů služba migrace databáze Azure, které jsou společné pro všechny scénáře migrace existují také požadavky, které platí konkrétně pro jeden scénář nebo jiné.

Při použití služby Azure Service migrace databáze k systému SQL Server do Azure SQL Database migrace, kromě předpokladů, které jsou společné pro všechny scénáře migrace, ujistěte se, zda je adresa následující další požadavky:
- Konfigurace vaší [brány Windows Firewall pro přístup k databázovému stroji](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Povolte protokol TCP/IP, který je zakázaný ve výchozím nastavení během instalace systému SQL Server Express, pomocí pokynů v článku [povolit nebo zakázat síťový protokol serveru](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Vytvoření instance Azure SQL Database instance, kterou provedete podle následujících podrobností v článku C[tvořit Azure SQL database na portálu Azure](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal).
- Stáhněte a nainstalujte [Data migrace pomocníka](https://www.microsoft.com/en-us/download/details.aspx?id=53595) v3.3 nebo novější.
- Otevřete vaší brány Windows firewall a povolit službu Azure databáze migrace pro přístup k zadané zdrojové databáze.
- Vytvořte úrovni serveru [pravidlo brány firewall](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) pro server Azure SQL Database, aby mohly služba migrace databáze Azure přístup k cílovým databázím. Zadejte rozsah podsíť virtuální sítě používaný pro službu Azure databáze migrace.
- Zkontrolujte, zda pověření používaná k připojení k instanci systému SQL Server zdrojové [ovládacího PRVKU serveru](https://docs.microsoft.com/en-us/sql/t-sql/statements/grant-server-permissions-transact-sql) oprávnění.
- Ujistěte se, že pověření použitá pro připojení k cílové instance Azure SQL Database mít oprávnění CONTROL DATABASE na cílovým databázím Azure SQL.

   > [!NOTE]
   > Úplný seznam požadovaných součástí pro používání služby Azure databáze migrace k provedení migrace z SQL serveru do Azure SQL Database, najdete v části kurzu [migrace systému SQL Server do Azure SQL Database](https://docs.microsoft.com/en-us/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="next-steps"></a>Další postup
Přehled služby Azure databáze migrace a místní dostupnosti během verzi Public Preview, najdete v článku [co je Azure databáze migrace služby Preview](dms-overview.md). 