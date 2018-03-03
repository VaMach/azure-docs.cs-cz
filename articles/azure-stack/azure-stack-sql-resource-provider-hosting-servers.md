---
title: "Hostitelské servery v zásobníku Azure SQL | Microsoft Docs"
description: "Postup přidání instance SQL pro zřizování prostřednictvím poskytovatele prostředků adaptér SQL"
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 0a29ef133a045b2828777050f2d7a204c0add4a8
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="add-hosting-servers-for-use-by-the-sql-adapter"></a>Přidání hostitelské servery pro použití adaptérem SQL

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Instance SQL můžete použít na virtuální počítače uvnitř vaší [zásobník Azure](azure-stack-poc.md), nebo k nim mohla připojit instanci mimo prostředí Azure zásobníku, pokud poskytovatel prostředků. Obecné požadavky jsou:

* SQL instance musí být vyhrazená pro použití úlohami RP a uživatele. Nelze použít instanci SQL, který se používá jakékoli další příjemce, včetně aplikační služby.
* Adaptér RP není připojený k doméně a lze připojit pouze pomocí ověřování SQL.
* Musíte nakonfigurovat účet s náležitými oprávněními pro použití RP.
* RP a uživatelé třeba webové aplikace používat síť uživatele, takže není vyžadována možnost připojení k instanci serveru SQL v této síti. Tento požadavek se obvykle znamená, že IP adresa pro vaše instance SQL musí být ve veřejné síti.
* Správa instance SQL a jejich hostitelů závisí na vás; RP neobsahuje provedení oprav, zálohování, přihlašovací údaje otočení atd.
* SKU lze použít k vytvoření různé třídy dalo SQL, jako je například výkonu, vždycky na atd.


Počet bitové kopie virtuálních počítačů SQL IaaS jsou k dispozici prostřednictvím funkce správy Marketplace. Zajistěte, aby vám vždy stáhnout nejnovější verzi rozšíření IaaS SQL před nasazením virtuálního počítače pomocí příslušné položky Marketplace. Image s SQL serverem jsou stejné jako virtuální počítače SQL, které jsou k dispozici v Azure. Pro virtuální počítače SQL vytvořené z těchto bitových kopií, rozšíření IaaS a odpovídající portálu vylepšení poskytují funkce, jako je automatické opravy a možnosti zálohování.

Existují další možnosti pro nasazení virtuálních počítačů SQL, včetně šablon v [galerii pro rychlý start Azure zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Všechny hostitelské servery, které jsou nainstalované v zásobníku Azure několika uzly musí být vytvořeny z předplatného uživatele. Nemohou být vytvářeny ze předplatného výchozí zprostředkovatel. Musí být vytvořený z portálu pro uživatele nebo z relace prostředí PowerShell s odpovídající přihlášení. Všechny hostitelské servery jsou fakturovatelné virtuální počítače a musí mít příslušnou licenci SQL. Správce služeb _můžete_ být vlastníkem daného předplatného.


### <a name="required-privileges"></a>Požadovaná oprávnění

Můžete vytvořit nového správce s oprávněními nižší než úplné sysadmin. Určité operace, které musí být povoleny jsou:

- Databáze: Vytvořit, změna, s členství ve skupině (Always On jenom), Drop, zálohování
- Skupina dostupnosti: Alter, připojení, přidat nebo odebrat databáze
- Přihlášení: Vytvořit, vyberte, Alter, Drop, odvolání
- Vyberte operace: \[hlavní\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn), sloupec sys.availability_replicas (AlwaysOn), zobrazení sys.databases, \[hlavní\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[hlavní\].\[ sys\].\[ availability_groups\] (AlwaysOn), sys.master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Zadejte kapacitu připojením na samostatnou hostitelem SQL serveru
Můžete použít samostatné (bez-HA) servery SQL Server prostřednictvím jakékoli edici systému SQL Server 2014 nebo SQL Server 2016. Ujistěte se, že máte přihlašovací údaje k účtu s oprávněními správce systému.

Pokud chcete přidat samostatný server, který je již zřízeno hostování, postupujte takto:

1. Přihlaste se k portálu pro správu zásobník Azure jako správce služeb

2. Klikněte na tlačítko **Procházet** &gt; **prostředky pro správu** &gt; **hostitelské servery SQL**.

  ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

  **Hostování servery SQL** okno je, kde můžete připojit zprostředkovatele prostředků SQL serveru k skutečné instance systému SQL Server, které slouží jako zprostředkovatel prostředků back-end.

  ![Hostitelské servery](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Vyplňte formulář Podrobnosti připojení instanci služby SQL Server.

  ![New Hosting Server](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Volitelně můžete zahrnout název instance a číslo portu lze zadat, pokud instance není přiřazen k výchozí port 1433.

  > [!NOTE]
  > Tak dlouho, dokud SQL instance může mít přístup uživatele a správce Azure Resource Manager, mohou být umístěny pod kontrolou zprostředkovatele prostředků. SQL instance __musí__ přidělit výhradně RP.

4. Při přidávání serverů, musíte je přiřadit k nové nebo existující SKU k odlišení nabídky služeb. Například může mít instanci SQL Enterprise poskytuje:
  - kapacita databáze
  - Automatické zálohování
  - Rezervovat vysoce výkonné servery pro jednotlivá oddělení
  - a tak dále.

  Název SKU by měla odpovídat vlastnosti tak, aby uživatelé můžete umístit své databáze správně. Všechny hostitelské servery v SKU musí mít stejné funkce.

    Příklad:

![Skladové položky](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
> SKU může trvat až jednu hodinu, mají být zobrazeny v portálu. Uživatele nelze vytvořit databázi, dokud nebude plně vytvořen verze SKU.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>Zadejte kapacitu pomocí SQL skupin dostupnosti Always On
Instance SQL Always On konfigurace vyžaduje další kroky a zahrnuje minimálně tři virtuální počítače (nebo fyzického počítače).

> [!NOTE]
> Adaptér SQL RP _pouze_ podporuje SQL 2016 SP1 Enterprise nebo novější instance pro Always On, protože vyžaduje nové funkce SQL, jako je například automatická synchronizace replik indexů. Kromě předchozích seznam běžných požadavků:

* Je nutné zadat souborový server kromě počítačů SQL Always On. Je [šablony Azure Quickstart zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-ha) toto prostředí, můžete vytvořit za vás. Také mohl sloužit jako vodítko k sestavení vlastní instanci.

* Je nutné nastavit servery SQL Server. Konkrétně je nutné povolit [Automatická synchronizace replik indexů](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) pro každou skupinu dostupnosti pro každou instanci systému SQL Server.

```
ALTER AVAILABILITY GROUP [<availability_group_name>]
    MODIFY REPLICA ON 'InstanceName'
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

Na sekundární instancí
```
ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
GO

```



Pokud chcete přidat SQL Always On hostitelskými servery, postupujte takto:

1. Přihlaste se k portálu pro správu zásobník Azure jako správce služby

2. Klikněte na tlačítko **Procházet** &gt; **prostředky pro správu** &gt; **SQL hostitelské servery** &gt; **+ přidat**.

    **Hostování servery SQL** okno je, kde můžete připojit zprostředkovatele prostředků SQL serveru k skutečné instance systému SQL Server, které slouží jako zprostředkovatel prostředků back-end.


3. Vyplňte formulář Podrobnosti připojení vaší instance systému SQL Server, nezapomeňte použít plně kvalifikovaný název domény nebo IPv4 adresu vždy na naslouchacího procesu (a volitelně také portu číslo). Zadejte informace o účtu pro účet, který jste nakonfigurovali s oprávněními správce systému.

4. Zaškrtnutím tohoto políčka Povolit podporu pro instance vždy na skupiny dostupnosti systému SQL.

    ![Hostitelské servery](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Přidáte k instanci SQL Always On SKU. Samostatné servery nelze kombinovat s instancí Always On ve stejné SKU. Který určí při přidávání první server pro hostování. Probíhá pokus o různé typy později, bude mít za následek chybu.


## <a name="making-sql-databases-available-to-users"></a>Zpřístupnění databáze SQL pro uživatele

Vytvořte plány a nabízí chcete zpřístupnit databáze SQL pro uživatele. Přidání služby Microsoft.SqlAdapter k plánu a přidat buď existující kvótu nebo vytvořte novou. Pokud vytvoříte kvótu, zadáte kapacitu uživatelům.

![Vytvoření plánu a nabídky zahrnout databáze](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="maintenance-of-the-sql-adapter-rp"></a>Údržby SQL adaptéru RP

Údržba instance SQL neplatí zde, s výjimkou informace o hesle otočení. Jste zodpovědní za použití dílčích oprav a zálohování nebo obnovení databáze serverů použít s adaptérem SQL.

### <a name="patching-and-updating"></a>Opravy a aktualizace
 Adaptér SQL není servis jako součást zásobník Azure, protože se jedná o součást rozšíření. Microsoft bude poskytovat aktualizace adaptéru SQL podle potřeby. Adaptér SQL je vytvořena na _uživatele_ virtuálního počítače v rámci předplatného výchozí zprostředkovatel. Proto je potřeba zadat Windows opravy, proti antivirové atd. Windows aktualizovat balíčky, které jsou k dispozici jako součást cyklu opravy a aktualizace můžete použít pro instalaci aktualizací do virtuálního počítače Windows. Až bude vydaná aktualizovaná adaptér, skript je určen k použití aktualizace. Tento skript vytvoří nový virtuální počítač RP a migrujte nějaký stav, který už máte.

 ### <a name="backuprestoredisaster-recovery"></a>Zálohování nebo obnovení nebo zotavení po havárii
 Adaptér SQL není zálohován jako součást procesu Azure zásobníku BC-zotavení po Havárii, protože se jedná o součást rozšíření. Skripty se poskytnout pro usnadnění:
- Zálohování nezbytné stavu informace (uložené v účtu úložiště Azure zásobníku)
- V případě, že bude nezbytné celý zásobník obnovení, obnovení RP.
Databázové servery musí nejprve obnovit (v případě potřeby), než je obnoven RP.

### <a name="updating-sql-credentials"></a>Aktualizuje se přihlašovací údaje SQL

Jste zodpovědní za vytváření a Správa účtů správce systému na serverech SQL. RP potřebuje účet s těmito oprávněními ke správě databáze jménem uživatelů – nepotřebuje přístup k datům v těchto databází. Pokud je potřeba aktualizovat hesla sa na serverech SQL, můžete změnit heslo používané RP funkce aktualizace RP rozhraní správce. Tato hesla jsou uloženy v Key Vault ve vaší instanci Azure zásobníku.

Chcete-li upravit nastavení, klikněte na tlačítko **Procházet** &gt; **prostředky pro správu** &gt; **hostování servery SQL** &gt; **Přihlášeních SQL** a vyberte přihlašovací jméno. Změny musí provést na instanci SQL nejprve (a všechny repliky, a to v případě potřeby). V **nastavení** panelu a potom klikněte na **heslo**.

![Aktualizovat heslo správce](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)


## <a name="next-steps"></a>Další postup

[Přidat databáze](azure-stack-sql-resource-provider-databases.md)
