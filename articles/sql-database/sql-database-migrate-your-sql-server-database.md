---
title: Migrace serveru SQL DB do Azure SQL Database | Microsoft Docs
description: "Naučte se migrovat databázi SQL serveru do Azure SQL Database."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,migrate
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 09/01/2017
ms.author: carlrab
ms.openlocfilehash: 526222944974c08f92aec2a8418e9b42401bc4d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrovat databázi SQL serveru do Azure SQL Database

Přesunutí serveru SQL Server databáze k databázi SQL Azure je jednoduché, vytváření prázdnou databázi SQL v Azure a pak pomocí [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) k importování databáze do Azure. V tomto kurzu jste postup:

> [!div class="checklist"]
> * Vytvořit prázdnou databázi Azure SQL na portálu Azure (pomocí nového nebo existujícího serveru Azure SQL Database)
> * Vytvoření brány firewall úrovni serveru na portálu Azure (pokud dříve vytvořili)
> * Použití [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) k importu databázi SQL serveru do prázdnou databázi Azure SQL 
> * Použití [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) Chcete-li změnit vlastnosti databáze.

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

- Nainstalovanou nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).  
- Nainstalovanou nejnovější verzi [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Zjištění a mít přístup k databázi pro migraci. Tento kurz používá [SQL Server 2008 R2 databáze AdventureWorks OLTP](https://msftdbprodsamples.codeplex.com/releases/view/59211) v instanci systému SQL Server 2008 R2 nebo novější, ale můžete použít libovolnou databázi podle svého výběru. Chcete-li opravit problémy s kompatibilitou, použijte [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Vytvořit prázdnou databázi SQL

Databáze SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](sql-database-service-tiers.md). Databáze se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) a na [logickém serveru Azure SQL Database](sql-database-features.md). 

Postupujte podle těchto kroků můžete vytvořit prázdnou databázi SQL. 

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Na stránce **Nový** vyberte **Databáze** a v části **Databáze SQL** na stránce **Nový** vyberte **Vytvořit**.

   ![Vytvořit prázdná databáze](./media/sql-database-design-first-database/create-empty-database.png)

3. Vyplňte formulář databáze SQL pomocí následujících informací, jak je vidět na předchozím obrázku:   

   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název databáze** | mySampleDatabase | Platné názvy databází najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Předplatné** | Vaše předplatné  | Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   | **Skupina prostředků** | myResourceGroup | Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Vyberte zdroj** | Prázdnou databázi | Určuje, že by měl být vytvořen prázdnou databázi. |

4. Klikněte na **Server** a vytvořte a nakonfigurujte nový server pro novou databázi. Vyplňte **nového formuláře serveru** s následujícími informacemi: 

   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název serveru** | Libovolný globálně jedinečný název | Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Přihlašovací jméno správce serveru** | Libovolné platné jméno | Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Heslo** | Libovolné platné heslo | Heslo musí mít aspoň osm znaků a musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky. |
   | **Umístění** | Libovolné platné umístění | Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/). |

   ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. Klikněte na **Vybrat**.

6. Klikněte na **Cenová úroveň** a zadejte úroveň služby, počet DTU a velikost úložiště. Prozkoumejte možnosti pro počet jednotek Dtu a úložiště, které je k dispozici pro jednotlivé úrovně služby. 

7. V tomto kurzu vyberte **standardní** úrovně služby a potom posuvníkem vyberte **100 Dtu (S3)** a **400** GB úložiště.

   ![create database-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Přijměte podmínky verze Preview pro použití možnosti **Doplňkové úložiště**. 

   > [!IMPORTANT]
   > \* Velikosti úložiště větší než velikost zahrnutého úložiště jsou ve verzi Preview a účtují se za ně další poplatky. Podrobnosti najdete na stránce s [cenami služby SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Na úrovni Premium je úložiště větší než 1 TB aktuálně dostupné v následujících oblastech: USA – východ 2, USA – západ, USA (Gov) – Virginia, Západní Evropa, Německo – střed, Jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada – střed a Kanada – východ. Viz [Aktuální omezení pro P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Po výběru úrovně služby, počtu DTU a velikosti úložiště klikněte na **Použít**.  

10. Vyberte **kolace** pro prázdnou databázi (v tomto kurzu použijte výchozí hodnotu). Další informace o kolacích najdete v tématu [kolace](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. Po vyplnění formuláře pro SQL Database klikněte na **Vytvořit** a databázi zřiďte. Zřizování trvá několik minut. 

12. Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.
    
     ![oznámení](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Služba SQL Database vytvoří bránu firewall na úrovni serveru, aby zabránila externím aplikacím a nástrojům v připojení k serveru nebo ke kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy. Postupujte podle těchto kroků a vytvořte [pravidlo brány firewall na úrovni serveru služby SQL Database](sql-database-firewall-configure.md) pro vaši IP adresu klienta a umožněte externí připojení přes bránu firewall služby SQL Database pouze pro vaši IP adresu. 

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

1. Po dokončení nasazení klikněte na **Databáze SQL** z nabídky na levé straně a klikněte na **mySampleDatabase** na stránce **Databáze SQL**. Otevře se stránka s přehledem pro vaši databázi, na které se zobrazí plně kvalifikovaný název serveru (například **mynewserver-20170824.database.windows.net**) a možnosti pro další konfiguraci. 

2. Zkopírujte tento plně kvalifikovaný název serveru, abyste ho mohli použít pro připojení k serveru a jeho databázím v následujících rychlých startech. 

   ![název serveru](./media/sql-database-get-started-portal/server-name.png) 

3. Klikněte na tlačítko **nastavení brány firewall serveru** na panelu nástrojů. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

   ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Klikněte na **Přidat IP adresu klienta** na panelu nástrojů a přidejte svoji aktuální IP adresu do nového pravidla brány firewall. Pravidlo brány firewall může otevřít port 1433 pro jednu IP adresu nebo rozsah IP adres.

5. Klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu, které otevře port 1433 na logickém serveru.

6. Klikněte na **OK** a pak zavřete stránku **Nastavení brány firewall**.

Nyní můžete připojit k serveru služby SQL Database a její databáze pomocí SQL Server Management Studio, Pomocník pro migraci dat nebo jiný nástroj podle svého výběru z tuto IP adresu pomocí účtu správce serveru, který je vytvořen v předchozím postupu.

> [!IMPORTANT]
> Standardně je přístup přes bránu firewall služby SQL Database povolený pro všechny služby Azure. Kliknutím na **OFF** na této stránce provedete zákaz pro všechny služby Azure.

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database. Název plně kvalifikovaný serveru používáte pro připojení k serveru Azure SQL pomocí nástroje pro klienta, včetně pomoci při migraci dat a SQL Server Management Studio.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**.

   ![informace o připojení](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Migrace databáze

Použijte následující postup použijte  **[Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595)**  k vyhodnocení připravenosti databáze pro migraci do Azure SQL Database a k dokončení migrace.

1. Otevřete **asistent migrace dat**. Přímý přístup do paměti můžete spustit na libovolném počítači se připojení k instanci systému SQL Server obsahující databáze, která chcete migrovat a připojení k Internetu. Nemusíte jej nainstalovat na počítač, který hostuje instanci systému SQL Server, které chcete migrovat. Pro počítač, na kterém je spuštěný Pomocníka pro migraci dat musí být pravidlo brány firewall, který jste vytvořili v předchozím postupu.

     ![Asistent migrace systému Open data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. V levé nabídce klikněte na tlačítko **+ nový** vytvořit **Assessment** projektu. Zadejte požadované hodnoty a potom klikněte na **vytvořit**:

   | Nastavení      | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Typ projektu | Migrace | Zvolit buď vyhodnocení databáze pro migraci nebo k vyhodnocení a migrace v rámci stejného pracovního postupu |
   |název projektu|Kurz migrace| Popisný název |
   |Typ zdrojového serveru| SQL Server | Toto je jediný zdroj aktuálně podporované. |
   |Typ cílového serveru| Azure SQL Database| Možnosti patří: Azure SQL Database, SQL Server, SQL Server na virtuálních počítačích Azure |
   |Migrace oboru| Schéma a data| Vybrat možnost: pouze data schématu a data, pouze, schéma |
   
   ![Nový projekt asistenta migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Na **zvolit zdroj** , zadejte požadované hodnoty a potom klikněte na tlačítko **Connect**:

    | Nastavení      | Navrhovaná hodnota | Popis | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Název serveru | Název serveru nebo IP adresa | Název serveru nebo IP adresa |
    | Typ ověřování | Typ upřednostňované ověřování| Volby: Integrované ověřování, ověřování hesla Active Directory pro Windows ověřování, ověřování systému SQL Server, Active Directory |
    | Uživatelské jméno | Vaše přihlašovací jméno | Přihlašovací jméno musí mít **ovládacího PRVKU serveru** oprávnění |
    | Heslo| heslo | heslo |
    | Vlastnosti připojení| Vyberte **připojení šifrováno** a **certifikátu serveru důvěryhodnosti** jako vhodné pro vaše prostředí. | Vyberte vlastnosti, které jsou vhodné pro připojení k serveru |

    ![Nový vyberte zdroj dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Vyberte jednu databázi ze zdrojového serveru migrovat do Azure SQL Database a potom klikněte na **Další**. V tomto kurzu je pouze jednu databázi.

6. Na **vyberte cíl** , zadejte požadované hodnoty a potom klikněte na tlačítko **připojit**:

    | Nastavení      | Navrhovaná hodnota | Popis | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Název serveru | Plně kvalifikovaný název serveru databáze Azure | Plně kvalifikovaný název serveru databáze Azure z předchozího postupu |
    | Typ ověřování | Ověřování SQL Serveru | Ověřování systému SQL Server je jedinou možností, jak tento kurz je napsán, ale integrované ověřování Active Directory a ověřování hesla Active Directory jsou podporovány také Azure SQL Database |
    | Uživatelské jméno | Vaše přihlašovací jméno | Přihlašovací jméno musí mít **CONTROL DATABASE** oprávnění k databázi zdrojové |
    | Heslo| heslo | heslo |
    | Vlastnosti připojení| Vyberte **připojení šifrováno** a **certifikátu serveru důvěryhodnosti** jako vhodné pro vaše prostředí. | Vyberte vlastnosti, které jsou vhodné pro připojení k serveru |

    ![Nový cíl vyberte možnost migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Vyberte databázi z cílového serveru, který jste vytvořili v předchozím postupu a pak klikněte na tlačítko **Další** ke spuštění procesu assessment schéma zdrojové databáze. V tomto kurzu je pouze jednu databázi. Všimněte si, že úroveň kompatibility pro tuto databázi je nastavena na 140, což je výchozí úroveň kompatibility pro všechny nové databáze v Azure SQL Database.

   > [!IMPORTANT] 
   > Po dokončení migrace databáze do Azure SQL Database, můžete pracovat databázi na úroveň kompatibility zadaného pro účely zpětné kompatibility. Další informace o důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s úrovní kompatibility.
   >

8. Na **vybrat objekty** dokončení stránky, po zdrojové databáze proces hodnocení schématu zkontrolujte objektů vybrána pro migraci a zkontrolovat objekty, které obsahují problémy. Zkontrolujte třeba **dbo.uspSearchCandidateResumes** objekt pro **SERVERPROPERTY('LCID')** změny chování a **HumanResourcesJobCandidate** objekt pro Změny fulltextové vyhledávání. 

   > [!IMPORTANT] 
   > V závislosti na návrh databáze a návrh vaší aplikace, při migraci zdrojové databáze, budete muset změnit buď nebo obě databáze nebo aplikace po migraci (a v některých případech, před migrací). Informace o rozdílech Transact-SQL, které může mít vliv na migraci najdete v tématu [rozdíly řešení Transact-SQL během migrace do SQL Database](sql-database-transact-sql-information.md).

     ![nové migrace hodnocení a objekt výběr dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Klikněte na tlačítko **skript SQL generovat** skript schématu objekty ve zdrojové databázi. 
10. Zkontrolujte generovaného skriptu a pak klikněte na tlačítko **další vydání** podle potřeby zkontrolovat assessment zjištěné problémy a doporučení. Pro fulltextové vyhledávání, například doporučuje při upgradu se k testování aplikace využívat funkce Full-Text. Můžete uložit nebo kopírovat skript, nechcete-li.

     ![nový skript generuje migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Klikněte na tlačítko **nasazení schématu** a podívejte se na migraci schématu.

     ![nové migrace schématu migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Po dokončení migrace schématu zkontrolujte výsledky pro chyby a pak za předpokladu, že neexistují žádné, klikněte na **migrovat data**.
13. Na **vyberte tabulky** , zkontrolujte v tabulkách vybrána pro migraci a pak klikněte na tlačítko **spustit migraci dat**.

     ![nové migrace dat migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Podívejte se na migraci.

     ![Nový proces migrace dat migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Připojení k databázi pomocí SSMS

Použití [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) k navázání připojení k serveru Azure SQL Database.

1. Otevřete SQL Server Management Studio.

2. V dialogovém okně **Připojení k serveru** zadejte následující informace:

   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Typ serveru | Databázový stroj | Tato hodnota je povinná. |
   | Název serveru | Plně kvalifikovaný název serveru | Název musí být přibližně takto: **mynewserver20170824.database.windows.net**. |
   | Authentication | Ověřování SQL Serveru | Ověřování SQL je jediný typ ověřování, který jsme v tomto kurzu nakonfigurovali. |
   | Přihlásit | Účet správce serveru | Jedná se o účet, který jste zadali při vytváření serveru. |
   | Heslo | Heslo pro účet správce serveru | Jedná se o heslo, které jste zadali při vytváření serveru. |

   ![Připojení k serveru](./media/sql-database-connect-query-ssms/connect.png)

3. Klikněte na **Možnosti** v dialogovém okně **Připojit k serveru**. V části **Připojit k databázi** zadejte **mySampleDatabase**, abyste se připojili k této databázi.

   ![připojení k databázi na serveru](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klikněte na **Připojit**. V aplikaci SSMS se otevře okno Průzkumníka objektů. 

5. V Průzkumníku objektů zobrazte objekty v ukázkové databázi rozbalením **Databáze** a potom **mySampleDatabase**.

   ![Objekty databáze.](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Změnit vlastnosti databáze

Můžete změnit úroveň služby, úroveň výkonu a úroveň kompatibility pomocí SQL Server Management Studio. Během fáze importu doporučujeme je importovat do databáze vyšší úroveň výkonu pro nejlepší výkon, ale po dokončení importu jak ušetřit peníze, dokud nebudete připraveni aktivně používá databázi importované škálování směrem dolů. Změna úrovně kompatibility mohou vést k vyšší výkon a přístup k nejnovější funkcím služby Azure SQL Database. Když provádíte migraci starší databázi, její úroveň kompatibility databáze je udržován na úrovni nejnižší podporovaná úroveň, který je kompatibilní s databází importována. Další informace najdete v tématu [vylepšuje výkon dotazů 130 úroveň kompatibility v Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. V Průzkumníku objektů, klikněte pravým tlačítkem na **mySampleDatabase** a pak klikněte na **nový dotaz**. Otevře se okno dotazu připojené k vaší databázi.

2. Spusťte následující příkaz pro danou vrstvu služeb **standardní** a na úroveň výkonu **S1**.

    ```sql
    ALTER DATABASE mySampleDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

## <a name="next-steps"></a>Další kroky 
V tomto kurzu jste se dozvěděli na:

> * Vytvořit prázdnou databázi Azure SQL na portálu Azure 
> * Vytvoření brány firewall úrovni serveru na portálu Azure 
> * Použití [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) k importu databázi SQL serveru do prázdnou databázi Azure SQL 
> * Použití [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) Chcete-li změnit vlastnosti databáze.

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit vaši databázi.

> [!div class="nextstepaction"]
> [Zabezpečení databáze Azure SQL](sql-database-security-tutorial.md).


