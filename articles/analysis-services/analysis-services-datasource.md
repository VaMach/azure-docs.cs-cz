---
title: "Zdroje dat podporované ve službě Azure Analysis Services | Microsoft Docs"
description: "Popisuje zdroje dat, které jsou podporovány pro modely dat v Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: 33115ee35670407c3b046f70a5fbebc47284b4b9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Zdroje dat podporované ve službě Azure Analysis Services
Servery Azure Analysis Services podporují připojení ke zdrojům dat v cloudu a místně ve vaší organizaci. Další podporované zdroje dat se přidávají vždy. Vraťte se často. 

Aktuálně jsou podporovány následující zdroje dat:

| Cloud  |
|---|
| Azure Blob Storage*  |
| Azure SQL Database  |
| Azure datového skladu |


| Lokálně  |   |   |   |
|---|---|---|---|
| Databáze Access  | Složky * | Oracle Database  | Databáze Teradata |
| Active Directory *  | Dokument JSON *  | Databáze Postgre SQL *  |XML tabulky * |
| Analysis Services  | Řádky z binárního souboru *  | SAP HANA*  |
| Analytics Platform System  | Databáze MySQL  | SAP Business Warehouse *  | |
| Dynamics CRM*  | Informační kanál OData *  | SharePoint*  |
| Sešit aplikace Excel  | Dotaz rozhraní ODBC  | Databáze SQL  |
| Exchange*  | OLE DB  | Databáze Sybase  |

\* Tabulkové 1400 modely jenom. 

> [!IMPORTANT]
> Připojení k místním zdrojům dat vyžadují [místní brána dat](analysis-services-gateway.md) nainstalovat do počítače ve vašem prostředí.

## <a name="data-providers"></a>Zprostředkovatelé dat

Modely dat v Azure Analysis Services může vyžadovat různé datové zprostředkovatele při připojování k určité zdroje dat. V některých případech může vrátit tabulkové modely služby připojení ke zdrojům dat pomocí nativního zprostředkovatele, jako je například SQL Server Native Client (SQLNCLI11) k chybě.

Zdroje pro datové modely, které se připojují k data v cloudu, například Azure SQL Database, pokud používáte nativní zprostředkovatelé než SQLOLEDB, mohou se zobrazit chybová zpráva: **"Zprostředkovatele 'SQLNCLI11.1' není zaregistrován."** Nebo, pokud máte model DirectQuery připojení k místním zdrojům dat, pokud používáte nativní poskytovatelů, mohou se zobrazit chybová zpráva: **"Chyba při vytváření sady řádků OLE DB. Nesprávná syntaxe poblíž textu 'LIMIT' "**.

Následující zdroje dat zprostředkovatele jsou podporovány v paměti nebo datové modely DirectQuery při připojování ke zdrojům dat v cloudu nebo místně:

### <a name="cloud"></a>Cloud
| **Zdroj dat** | **V paměti** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |Zprostředkovatel dat .NET framework pro SQL Server |Zprostředkovatel dat .NET framework pro SQL Server |
| Azure SQL Database |Zprostředkovatel dat .NET framework pro SQL Server |Zprostředkovatel dat .NET framework pro SQL Server | |

### <a name="on-premises-via-gateway"></a>Místní (prostřednictvím brány)
|**Zdroj dat** | **V paměti** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0 |Zprostředkovatel dat .NET framework pro SQL Server |
| SQL Server |Zprostředkovatel Microsoft OLE DB pro SQL Server |Zprostředkovatel dat .NET framework pro SQL Server | |
| SQL Server |Zprostředkovatel dat .NET framework pro SQL Server |Zprostředkovatel dat .NET framework pro SQL Server | |
| Oracle |Zprostředkovatel Microsoft OLE DB pro Oracle |Zprostředkovatel dat Oracle pro .NET | |
| Oracle |Zprostředkovatel dat Oracle pro .NET |Zprostředkovatel dat Oracle pro .NET | |
| Teradata |Zprostředkovatel OLE DB pro Teradata |Zprostředkovatel dat Teradata pro rozhraní .NET | |
| Teradata |Zprostředkovatel dat Teradata pro rozhraní .NET |Zprostředkovatel dat Teradata pro rozhraní .NET | |
| Analytics Platform System |Zprostředkovatel dat .NET framework pro SQL Server |Zprostředkovatel dat .NET framework pro SQL Server | |

> [!NOTE]
> Zkontrolujte, zda 64-bit poskytovatelé jsou nainstalovaní při použití místní brána.
> 
> 

Při migraci místní SQL Server Analysis Services tabulkový model Azure Analysis Services, může být nutné změnit zprostředkovatele.

**Pokud chcete zadat zprostředkovatele zdroje dat**

1. V sadě SSDT > **tabulkový Model Explorer** > **zdroje dat**, klikněte pravým tlačítkem na připojení ke zdroji dat a pak klikněte na tlačítko **upravit zdroj dat**.
2. V **upravit připojení**, klikněte na tlačítko **Upřesnit** a otevřete okno Vlastnosti zálohy.
3. V **nastavit rozšířené vlastnosti** > **zprostředkovatelé**, pak vyberte příslušného poskytovatele.

## <a name="impersonation"></a>Zosobnění
V některých případech může být nutné zadat účet jiné zosobnění. Účet zosobnění lze zadat v rozšíření SSDT nebo SSMS.

Pro místní zdroje dat:

* Pokud používáte ověřování SQL, musí být zosobnění účtu služby.
* Pokud používáte ověřování systému Windows, nastavte uživatele nebo heslo systému Windows. Ověřování systému Windows s konkrétní zosobnění účtu systému SQL Server, je podporováno pouze pro modely dat v paměti.

Pro cloudové zdroje dat:

* Pokud používáte ověřování SQL, musí být zosobnění účtu služby.

## <a name="next-steps"></a>Další postup
Pokud máte místní zdroje dat, je nutné nainstalovat [místní brána](analysis-services-gateway.md).   
Další informace o správě váš server v rozšíření SSDT nebo SSMS najdete v tématu [správě serveru](analysis-services-manage.md).

