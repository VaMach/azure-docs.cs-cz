---
title: "Návrh svoji první databázi Azure SQL | Microsoft Docs"
description: "Postup návrhu svoji první databázi Azure SQL na portálu Azure a SQL Server Management Studio."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 08/25/2017
ms.author: carlrab
ms.openlocfilehash: 329003c7c4abe89f4af04473ee3664605b2ea81f
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="design-your-first-azure-sql-database"></a>Návrh svoji první databázi Azure SQL

Databáze SQL Azure je relační databáze jako a služba (DBaaS) v Microsoft cloudu (Azure). V tomto kurzu zjistíte, jak pomocí portálu Azure a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) na: 

> [!div class="checklist"]
> * Vytvoření databáze na portálu Azure
> * Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure
> * Připojení k databázi pomocí SSMS
> * Vytváření tabulek pomocí SSMS
> * Hromadné načtení dat pomocí BCP
> * Dotaz na data pomocí SSMS
> * Obnovit databázi do předchozí [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) na portálu Azure

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, ujistěte se, že jste nainstalovali:
- Nejnovější verzi [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- Nejnovější verzi [BCP a SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

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

Nyní se můžete z této IP adresy připojit k serveru SQL Database a jeho databázím pomocí aplikace SQL Server Management Studio nebo jiného nástroje podle vašeho výběru použitím účtu správce serveru vytvořeného dříve.

> [!IMPORTANT]
> Standardně je přístup přes bránu firewall služby SQL Database povolený pro všechny služby Azure. Kliknutím na **OFF** na této stránce provedete zákaz pro všechny služby Azure.

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database. Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí aplikace SQL Server Management Studio.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**.

   ![informace o připojení](./media/sql-database-get-started-portal/server-name.png)

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

## <a name="create-tables-in-the-database"></a>Vytváření tabulek v databázi 

Vytvořte schéma databáze s čtyři tabulek, které model student systém správy pro použití vysoké školy [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):

- Osoba
- Kurz
- Student
- Úvěrového tohoto modelu systém správy student pro vysoké školy

Následující diagram znázorňuje, jak tyto tabulky jsou vzájemně souvisí. Některé z těchto tabulek odkazovat na sloupce v jiné tabulky. Například studenty tabulka odkazuje **PersonId** sloupec **osoba** tabulky. Studie diagramu na pochopit, jak jsou vzájemně propojeny v tabulkách v tomto kurzu. Podrobný rozbor toho, jak vytvořit efektivní databázové tabulky, najdete v části [vytvořit efektivní databázových tabulek](https://msdn.microsoft.com/library/cc505842.aspx). Informace o výběru datových typů najdete v tématu [datové typy](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Můžete také [návrháře tabulky v aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) vytvoření a tabulek. 

![Relace mezi tabulkami](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.

2. V okně dotazu spusťte následující dotaz, který vytvořit čtyři tabulky v databázi: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Vytváření tabulek](./media/sql-database-design-first-database/create-tables.png)

3. Rozbalte uzel 'tabulky' v Průzkumníku objekt SQL Server Management Studio zobrazíte tabulky, které jste vytvořili.

   ![vytvoření tabulek aplikace ssms](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Načtení dat do tabulky

1. Vytvořte složku s názvem **SampleTableData** ve složce soubory ke stažení pro uložení ukázkových dat pro vaši databázi. 

2. Klikněte pravým tlačítkem na následující odkazy a uložit je do **SampleTableData** složky. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Otevřete okno příkazového řádku a přejděte do složky SampleTableData.

4. Spuštěním následujících příkazů vložení ukázková data do tabulky nahrazení hodnoty **ServerName**, **DatabaseName**, **uživatelské jméno**, a  **Heslo** s hodnotami pro vaše prostředí.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Ukázková data mají nyní načtena do tabulky, které jste vytvořili dříve.

## <a name="query-data"></a>Dotazování dat

Spusťte tyto dotazy k načtení informací z tabulek databáze. V tématu [zápis dotazů SQL](https://technet.microsoft.com/library/bb264565.aspx) Další informace o vytváření dotazů SQL. První dotaz spojí všechny čtyři tabulky najít všechny studenti výukové podle ' Dominick Pope', kteří mají vyšší než 75 % úrovni ve své třídě. Druhý dotaz spojuje všechny čtyři tabulky a vyhledá všechny kurzy, ve kterých má někdy zapsaná 'Noe Colemane'.

1. V okně dotazu SQL Server Management Studio spusťte následující dotaz:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. V okně dotazu SQL Server Management Studio spusťte následující dotaz:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase

Představte si, že jste omylem odstranili tabulku. Toto je něco, které nelze snadno obnovit z. Databáze SQL Azure můžete přejít zpět do libovolného bodu v čase v poslední až do 35 dní a obnovit tento bod v čase pro novou databázi. Můžete tuto databázi obnovit odstraněná data. Následující kroky obnovení ukázkové databáze do bodu předtím, než byly přidány tabulky.

1. Na stránce databáze SQL pro vaši databázi, klikněte na **obnovení** na panelu nástrojů. **Obnovení** otevře se stránka.

   ![Obnovení](./media/sql-database-design-first-database/restore.png)

2. Vyplňte **obnovení** formuláře se požadované informace:
    * Název databáze: Zadejte název databáze. 
    * Bod v čase: Vyberte **v daném okamžiku** kartu na formuláři obnovení 
    * Bod obnovení: vyberte čas, ke kterému dochází před změnou databáze
    * Cílový server: tuto hodnotu nelze změnit, při obnovení databáze 
    * Fond elastické databáze: vyberte **None**  
    * Cenová úroveň: vyberte **20 Dtu** a **40 GB** úložiště.

   ![bod obnovení](./media/sql-database-design-first-database/restore-point.png)

3. Klikněte na tlačítko **OK** k obnovení databázi do [obnovit k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) předtím, než byly přidány tabulky. Obnovení databáze do jiného bodu v čase vytvoří duplicitní databázi na stejném serveru jako původní databáze od bodu v čase, které zadáte, dokud je v rámci dobu uchování vašeho [vrstvy služby](sql-database-service-tiers.md).

## <a name="next-steps"></a>Další kroky 
V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit databázi do předchozího bodu v čase. Naučili jste se tyto postupy:
> [!div class="checklist"]
> * Vytvoření databáze
> * Nastavit pravidlo brány firewall
> * Připojení k databázi s [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Vytváření tabulek
> * Hromadné načítání dat
> * Dotaz na data
> * Obnovení databáze do předchozího bodu v čase pomocí SQL Database [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) možnosti

Přechodu na v dalším kurzu se dozvíte o návrhu databáze pomocí sady Visual Studio a C#.

> [!div class="nextstepaction"]
>[Návrh Azure SQL database a připojení s C# a ADO.NET](sql-database-design-first-database-csharp.md)
