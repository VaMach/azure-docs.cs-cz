---
title: "Přehled pravidel brány firewall služby SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak nakonfigurovat bránu firewall databáze SQL s pravidly brány firewall na úrovni serveru a databáze pro správu přístupu."
keywords: "brána firewall databáze"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/23/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: ae1cacf0ff003e69a16d6beac48abc36a7f18896


---
# <a name="overview-of-azure-sql-database-firewall-rules"></a>Přehled pravidel brány firewall služby Azure SQL Database 
> [!div class="op_single_selector"]
> * [Přehled](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. Brána firewall pomáhá chránit vaše data tím, že brání veškerému přístupu k vašemu databázovému serveru, dokud neurčíte, které počítače k tomu mají oprávnění. Brána firewall uděluje přístup k databázím v závislosti na zdrojové IP adrese každého požadavku.

Bránu firewall nakonfigurujete tak, že vytvoříte pravidla brány firewall určující rozsahy přípustných IP adres. Můžete vytvořit pravidla brány firewall na úrovni serveru nebo databáze.

* **Pravidla brány firewall na úrovni serveru:** Tato pravidla umožňují klientům přístup k celému serveru SQL Azure, jinými slovy ke všem databázím na stejném logickém serveru. Tato pravidla se ukládají v **hlavní** databázi. Pravidla brány firewall na úrovni serveru můžete konfigurovat pomocí portálu nebo pomocí příkazů jazyka Transact-SQL. Pokud chcete vytvořit pravidla brány firewall na úrovni serveru pomocí portálu Azure Portal nebo PowerShellu, musíte být vlastníkem nebo přispěvatelem předplatného. Pokud chcete vytvořit pravidlo brány firewall na úrovni serveru pomocí příkazu Transact-SQL, musíte se připojit k instanci služby SQL Database jako přihlášení objektu zabezpečení nebo správce Azure Active Directory (to znamená, že musí být pravidlo brány firewall na úrovni serveru nejdřív vytvořené uživatelem s oprávněními na úrovni Azure).
* **Pravidla brány firewall na úrovni databáze:** Tato pravidla umožňují klientům přístup k jednotlivým databázím na serveru služby Azure SQL Database. Můžete je vytvořit pro každou databázi – ukládají se v jednotlivých databázích. (Pravidla brány firewall na úrovni databáze můžete vytvořit i pro **hlavní** databázi) Tato pravidla mohou být užitečná při omezování přístupu k určitým (zabezpečeným) databázím na stejném logickém serveru. Pravidla brány firewall na úrovni databáze lze konfigurovat pouze pomocí příkazů jazyka Transact-SQL.

**Doporučení:** Společnost Microsoft doporučuje pro zvýšení zabezpečení a přenositelnosti databází používat pravidla brány firewall na úrovni databáze kdykoli je to možné. Použijte pravidla brány firewall na úrovni serveru pro správce a pokud máte mnoho databází se stejnými požadavky na přístup a nechcete ztrácet čas konfigurací jednotlivých databází.

> [!Note]
> Informace o přenosných databázích v kontextu kontinuity podnikových procesů najdete v tématu [Požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md).
>

## <a name="firewall-overview"></a>Přehled brány firewall
Zpočátku je veškerý přístup Transact-SQL k vašemu serveru SQL Azure blokován branou firewall. Abyste mohli začít používat svůj server SQL Azure, musíte přejít na web Azure Portal a zadat jedno nebo více pravidel brány firewall na úrovni serveru, která umožní přístup na váš server SQL Azure. Pomocí pravidel brány firewall určete, které rozsahy IP adres z internetu jsou povolené a zda se mohou aplikace Azure pokoušet připojovat k vašemu serveru SQL Azure.

Pokud chcete selektivně udělit přístup pouze k jedné z databází na vašem serveru SQL Azure, musíte pro příslušnou databázi vytvořit pravidlo na úrovni databáze. Zadejte pro pravidlo brány firewall databáze rozsah IP adres, který je mimo rozsah IP adres zadaný v pravidlu brány firewall na úrovni serveru, a ujistěte se, že IP adresa klienta spadá do rozsahu zadaného v pravidlu na úrovni databáze.

Pokusy o připojení z internetu a z Azure musí nejdříve projít přes bránu firewall a až potom mají přístup k vašemu serveru SQL Azure nebo službě SQL Database. Znázorňuje to následující diagram:

   ![Diagram popisující konfiguraci brány firewall.][1]

## <a name="connecting-from-the-internet"></a>Připojení z Internetu
Když se počítač pokusí z internetu připojit k vašemu databázovému serveru, brána firewall pomocí úplné sady pravidel brány firewall zkontroluje zdrojovou IP adresu požadavku:

* Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni serveru, připojení k vašemu serveru služby Azure SQL Database je povoleno.
* Pokud IP adresa požadavku není v žádném z rozsahů určených v pravidlech brány firewall na úrovni serveru, zkontrolují se pravidla brány firewall na úrovni databáze. Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni databáze, je povoleno pouze připojení k databázi s odpovídajícím pravidlem na úrovni databáze.
* Pokud IP adresa požadavku není v žádném z rozsahů určených v pravidlech brány firewall na úrovni serveru nebo databáze, požadavek na připojení selže.

> [!NOTE]
> Pro přístup ke službě Azure SQL Database z místního počítače se ujistěte, že brána firewall na vaší síti i místní počítač umožňují odchozí komunikaci na portu TCP 1433.
> 

## <a name="connecting-from-azure"></a>Připojení z Azure
Pokud chcete umožnit aplikacím z Azure připojení k vašemu serveru SQL Azure, musí být povolená připojení Azure. Když se aplikace z Azure pokusí připojit k vašemu databázovému serveru, brána firewall ověří, zda jsou povolená připojení Azure. Nastavení brány firewall s počáteční i koncovou adresou odpovídající 0.0.0.0 znamená, že jsou tato připojení povolená. Pokud pokus o připojení není povolený, požadavek se k serveru služby Azure SQL Database vůbec nedostane.

> [!IMPORTANT]
> Touto možností se brána firewall nakonfiguruje tak, aby povolovala všechna připojení z Azure, včetně připojení z předplatných ostatních zákazníků. Když vyberete tuto možnost, ujistěte se, že vaše přihlašovací a uživatelská oprávnění omezují přístup pouze na autorizované uživatele.
> 

> [!NOTE]
>  Další informace najdete v části **SQL Database: Vnější vs. vnitřní** v tématu [Porty nad 1433 pro technologii ADO.NET 4.5 a službu SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
>  

## <a name="creating-the-first-server-level-firewall-rule"></a>Vytvoření prvního pravidla brány firewall na úrovni serveru
První nastavení brány firewall na úrovni serveru můžete vytvořit pomocí webu [Azure Portal](https://portal.azure.com/) nebo programově pomocí rozhraní REST API nebo Azure PowerShellu. Další pravidla brány firewall na úrovni serveru můžete vytvářet a spravovat těmito způsoby nebo prostřednictvím jazyka Transact-SQL. Pro zvýšení výkonu se pravidla brány firewall na úrovni serveru dočasně ukládají do mezipaměti na úrovni databáze. Pokud chcete mezipaměť aktualizovat, podívejte se na příkaz [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Další informace o pravidlech brány firewall na úrovni serveru najdete v tématu [Postup: Konfigurace brány firewall serveru SQL Azure pomocí webu Azure Portal](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Vytváření pravidel brány firewall na úrovni databáze
Po dokončení konfigurace první brány firewall na úrovni serveru možná budete chtít omezit přístup k určitým databázím. Pokud v pravidlu brány firewall na úrovni databáze zadáte rozsah IP adres, který je mimo rozsah zadaný v pravidlu brány firewall na úrovni serveru, budou mít k dané databázi přístup pouze klienti, jejichž IP adresa je v rozsahu na úrovni databáze. Pro jednu databázi můžete mít maximálně 128 pravidel brány firewall na úrovni databáze. Pravidla brány firewall na úrovni databáze pro hlavní a uživatelské databáze můžete vytvářet a spravovat prostřednictvím jazyka Transact-SQL. Další informace o konfiguraci pravidel brány firewall na úrovni databáze najdete v článku [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě webu Azure Portal můžete pravidla brány firewall spravovat programově pomocí jazyka Transact-SQL, rozhraní REST API nebo Azure Powershellu. V následujících tabulkách jsou popsány sady příkazů dostupné pro jednotlivé metody.

### <a name="transact-sql"></a>Transact-SQL
| Zobrazení katalogu nebo uložená procedura | Úroveň | Popis |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Zobrazí aktuální pravidla brány firewall na úrovni serveru. |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni serveru. |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Databáze |Zobrazí aktuální pravidla brány firewall na úrovni databáze. |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Databáze |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni databáze. |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databáze |Odebere pravidla brány firewall na úrovni databáze. |

### <a name="rest-api"></a>REST API
| Rozhraní API | Úroveň | Popis |
| --- | --- | --- |
| [Výpis pravidel brány firewall](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Server |Zobrazí aktuální pravidla brány firewall na úrovni serveru. |
| [Vytvoření pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Server |Vytvoří nebo aktualizuje pravidla brány firewall na úrovni serveru. |
| [Nastavení pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Server |Aktualizuje vlastnosti existujícího pravidla brány firewall na úrovni serveru. |
| [Odstranění pravidla brány firewall](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |

### <a name="azure-powershell"></a>Azure PowerShell
| Rutina | Úroveň | Popis |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Server |Vrátí aktuální pravidla brány firewall na úrovni serveru. |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Server |Vytvoří nové pravidlo brány firewall na úrovni serveru |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Server |Aktualizuje vlastnosti existujícího pravidla brány firewall na úrovni serveru. |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Server |Odebere pravidla brány firewall na úrovni serveru. |

> [!NOTE]
> Může trvat až pět minut, než se změny nastavení brány firewall projeví.
> 
> 

## <a name="troubleshooting-the-database-firewall"></a>Řešení potíží s branou firewall databáze
Pokud se přístup ke službě Microsoft Azure SQL Database nechová podle očekávání, zvažte následující:

* **Konfigurace místní brány firewall:** Než bude váš počítač moci přistupovat ke službě Azure SQL Database, možná bude nutné vytvořit pro váš počítač výjimku brány firewall pro port TCP 1433. Pokud provádíte připojení v rámci cloudu Azure, možná bude nutné otevřít další porty. Další informace najdete v části **SQL Database verze V12: Vnější vs. vnitřní** tématu [Porty nad 1433 pro technologii ADO.NET 4.5 a službu SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Překlad adres (NAT):** Je možné, že kvůli překladu adres (NAT) se bude lišit IP adresa, kterou váš počítač používá pro připojení ke službě Azure SQL Database, a IP adresa, která se zobrazuje na vašem počítači v nastavení konfigurace IP adresy. Pokud chcete zobrazit IP adresu, kterou váš počítač používá pro připojení k Azure, přihlaste se na portál a přejděte na kartu **Konfigurovat** na serveru, který je hostitelem vaší databáze. V části **Povolené IP adresy** se zobrazí **Aktuální IP adresa klienta**. Kliknutím na **Přidat** mezi **Povolené IP adresy** umožníte tomuto počítači přístup k serveru.
* **Změny seznamu povolených se ještě neprojevily:** Může trvat až pět minut, než se změny konfigurace brány firewall služby Azure SQL Database projeví.
* **Přihlášení není autorizováno nebo bylo použito nesprávné heslo:** Pokud přihlášení nemá oprávnění k serveru služby Azure SQL Database nebo pokud bylo použito nesprávné heslo, připojení k serveru služby Azure SQL Database je zamítnuto. Vytvoření nastavení brány firewall klientům pouze poskytuje možnost pokusit se o připojení k vašemu serveru – každý klient musí dodat potřebné zabezpečené přihlašovací údaje. Další informace o přípravě přihlášení najdete v tématu Správa databází, přihlášení a uživatelů ve službě Azure SQL Database.
* **Dynamická IP adresa:** Pokud vaše internetové připojení používá dynamické přidělování IP adres a máte problémy dostat se přes bránu firewall, můžete zkusit jedno z následujících řešení:
  
  * Požádejte svého poskytovatele internetových služeb (ISP) o rozsah IP adres přidělený vašim klientským počítačům, které přistupují k serveru služby Azure SQL Database, a následně přidejte tento rozsah IP adres jako pravidlo brány firewall.
  * Získejte pro své klientské počítače statické přidělování IP adres a následně přidejte tyto IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další kroky
V těchto článcích najdete informace o vytváření pravidel brány firewall na úrovni serveru a na úrovni databáze:

* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí webu Azure Portal](sql-database-configure-firewall-settings.md)
* [Konfigurace pravidel brány firewall na úrovni serveru a databáze služby Azure SQL Database pomocí jazyka T-SQL](sql-database-configure-firewall-settings-tsql.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytvoření databáze najdete v článku [Vytvoření databáze SQL během několika minut pomocí webu Azure Portal](sql-database-get-started.md).
S připojováním k databázi SQL Azure z open source aplikací nebo aplikací třetích stran vám pomůžou [Ukázky kódu pro rychlý start klientů se službou SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
S pochopením přecházení do databází vám pomůže článek [Správa přístupu k databázi a zabezpečení přihlášení](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Další zdroje
* [Zabezpečení databáze](sql-database-security-overview.md)
* [Security Center pro databázový stroj SQL Server a Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png



<!--HONumber=Dec16_HO4-->


