---
title: "Přehled ochrany dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Informace o ochraně dat ve službě Azure SQL Database."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 12/21/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 67b32b224d86e8d0ed06d8248c3a2c5f5569c5cb


---
# <a name="protecting-data-within-your-sql-database"></a>Ochrana dat v rámci služby SQL Database

Služba SQL Database chrání vaše data pomocí šifrování.  

## <a name="overview"></a>Přehled

Služba SQL Database chrání vaše data zajištěním šifrování přenášených dat pomocí [protokolu TLS (Transport Layer Security)](https://support.microsoft.com/en-us/kb/3135244), neaktivních uložených dat pomocí [transparentního šifrování dat](http://go.microsoft.com/fwlink/?LinkId=526242) a používaných dat pomocí funkce [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx). Diskuzi o používání těchto funkcí ochrany dat ve službě SQL Database najdete v tématu [Ochrana a zabezpečení dat](sql-database-protect-data.md).

Existují i jiné možnosti šifrování dat:

* [Šifrování na úrovni buňky](https://msdn.microsoft.com/library/ms179331.aspx) umožňuje šifrovat určité sloupce nebo buňky dat pomocí různých šifrovacích klíčů.
* Pokud potřebujete modul hardwarového zabezpečení nebo chcete centrálně spravovat hierarchii šifrovacích klíčů, měli byste uvažovat o [řešení Azure Key Vault s SQL Serverem na virtuálním počítači v Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).


Služba SQL Database chrání vaše data poskytováním možností auditování a detekce hrozeb. 

### <a name="auditing"></a>Auditování
Auditování služby SQL Database sleduje databázové aktivity a pomáhá dodržovat legislativní předpisy díky zaznamenávání databázových událostí do protokolu auditu ve vašem účtu služby Azure Storage. Auditování vám umožňuje pochopit probíhající databázové aktivity a pomocí analýzy a zkoumání historické aktivity identifikovat potenciální hrozby nebo možné zneužití a narušení zabezpečení. Další informace najdete v tématu [Začínáme s auditováním služby SQL Database](sql-database-auditing-get-started.md).  

### <a name="threat-detection"></a>Detekce hrozeb
Detekce hrozeb doplňuje auditování a poskytuje další integrovanou vrstvu analytických funkcí zabezpečení ve službě Azure SQL Database. Nepřetržitě studuje, profiluje a detekuje neobvyklé databázové aktivity. Upozorní vás na podezřelé aktivity, potenciální ohrožení zabezpečení, útoky prostřednictvím injektáže SQL a neobvyklé vzory přístupu k databázi. Na upozornění můžete reagovat pomocí poskytnutých informativních pokynů a pokynů s možností reakce. Další informace najdete v článku [Začínáme zjišťovat hrozby ve službě SQL Database](sql-database-threat-detection-get-started.md).  

## <a name="connection-security"></a>Zabezpečení připojení
Zabezpečení připojení spočívá v použití pravidel brány firewall a šifrovaného připojení k omezení a zabezpečení připojení k databázi.

Server i databáze používají pravidla brány firewall k zamítnutí pokusů o připojení z IP adres, které nejsou výslovně povolené. Pokud chcete aplikaci nebo veřejné IP adrese klientského počítače povolit připojení k nové databázi, musíte napřed na úrovni serveru vytvořit pravidlo brány firewall. Použijte k tomu portál Azure Classic, REST API nebo PowerShell. Doporučujeme co nejvíce omezit rozsah IP adres povolených v serverové bráně firewall. Další informace najdete v tématu [Brána firewall služby Azure SQL Database](https://msdn.microsoft.com/library/ee621782).

Všechna připojení ke službě Azure SQL Database vyžadují nepřetržité šifrování (SSL/TLS) příchozích a odchozích databázových dat. V připojovacím řetězci aplikace musíte zadat parametry, které šifrují připojení, a nastavit, že *nechcete* důvěřovat certifikátu serveru (udělá se to automaticky, když zkopírujete připojovací řetězec z portálu Azure Classic), jinak připojení nemůže ověřit identitu serveru a může být napadeno útočníky, kteří se vydávají za prostředníky. Například u ovladače ADO.NET mají parametry připojovacího řetězce hodnoty **Encrypt=True** a **TrustServerCertificate=False**. Další informace najdete v tématu [Šifrování připojení a ověřování certifikátu u Azure SQL Database](https://msdn.microsoft.com/library/azure/ff394108#encryption).

## <a name="authentication"></a>Ověřování
Ověřování se týká způsobu, jakým prokážete svou identitu při připojování k databázi. SQL Database podporuje dva typy ověřování:

* **Ověřování SQL,** které používá uživatelské jméno a heslo.
* **Ověřování Azure Active Directory,** které používá identity spravované v Azure Active Directory a je podporované u spravovaných a integrovaných domén.

Když jste vytvářeli logický server databáze, zadali jste uživatelské jméno a heslo účtu „server admin“. Tyto přihlašovací údaje můžete použít k ověření a přihlášení k libovolné databázi na daném serveru jako vlastník databáze neboli „dbo“. Pokud chcete k ověřování použít Azure Active Directory, musíte vytvořit jiného správce serveru, který se jmenuje „Azure AD admin“ a který smí spravovat uživatele a skupiny služby Azure AD. Tento správce také smí provádět všechny operace jako běžný správce serveru. Postup vytvoření účtu správce služby Azure AD, který umožňuje ověřování Azure Active Directory, najdete v tématu [Připojení ke službě SQL Database s ověřením přes Azure Active Directory](sql-database-aad-authentication.md).

Doporučený postup je, aby aplikace k ověření používala jiný účet – omezíte tak oprávnění udělené aplikaci a snížíte riziko škodlivých aktivit v případě ohrožení kódu aplikace útoky v podobě injektáže SQL. Doporučený postup je vytvořit [uživatele databáze s omezením](https://msdn.microsoft.com/library/ff929188), který umožňuje ověřit aplikaci přímo v databázi. Pokud chcete vytvořit uživatele databáze s omezením, který používá ověřování SQL, připojte se k uživatelské databázi jako správce serveru a spusťte následující příkaz T-SQL:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Pokud jste vytvořili správce Azure AD, můžete vytvořit uživatele databáze s omezením, který používá ověřování Azure Active Directory, spuštěním následujícího příkazu T-SQL. Musíte při tom být připojeni k uživatelské databázi jako správce služby Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

V obou případech musí připojovací řetězec aplikace specifikovat přihlašovací údaje uživatele místo přihlašovacích údajů správce serveru, aby bylo možné se připojit k databázi.

Další informace o ověřování připojení ke službě SQL Database najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md).

## <a name="authorization"></a>Autorizace
Autorizace určuje, co můžete dělat ve službě Azure SQL Database. Vaše možnosti jsou dané členstvím a oprávněními role vašeho účtu. Doporučený postup je udělit uživatelům co nejmenší možná oprávnění. Azure SQL Database umožňuje jednoduše spravovat role v T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Účet správce serveru, který používáte k připojení, je členem skupiny db_owner. Tato skupina může s databází provádět všechny operace. Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy. Použijte účet „ApplicationUser“, který má omezenější oprávnění a umožňuje připojit se z aplikace k databázi s nejnižšími oprávněními, jaké aplikace potřebuje.

Existují i další způsoby, jak ještě více omezit možnosti uživatele služby Azure SQL Database:

* Můžete použít jiné [databázové role](https://msdn.microsoft.com/library/ms189121) než db_datareader a db_datawriter k vytvoření uživatelských účtů aplikace s větším nebo menším oprávněním.
* Granulárními [oprávněními](https://msdn.microsoft.com/library/ms191291) můžete kontrolovat, jaké operace můžete provádět s jednotlivými sloupci, tabulkami, zobrazeními, postupy a jinými objekty v databázi.
* K dočasnému bezpečnému zvýšení oprávnění můžete použít [zosobnění](https://msdn.microsoft.com/library/vstudio/bb669087) a [přihlašování k modulům](https://msdn.microsoft.com/library/bb669102).
* K omezení řádků, ke kterým má uživatel přístup, můžete použít [zabezpečení na úrovni řádku](https://msdn.microsoft.com/library/dn765131).
* K omezení rizika ohrožení citlivých dat můžete použít [maskování dat](sql-database-dynamic-data-masking-get-started.md).
* K omezení akcí, které je možné s databází provádět, můžete použít [uložené procedury](https://msdn.microsoft.com/library/ms190782).

Správa databází a logických serverů na portálu Azure Classic nebo v rozhraní API Azure Resource Manageru se řídí tím, jaké role má uživatelský účet na portálu přiřazené. Další informace k tomuto tématu najdete v článku o [řízení přístupu podle rolí na portálu Azure Portal](../active-directory/role-based-access-control-configure.md).

## <a name="encryption"></a>Šifrování
Azure SQL Database chrání vaše data tím, že je šifruje v době nečinnosti, a dále tím, že k ukládání databázových souborů a záloh používá [transparentní šifrování dat](http://go.microsoft.com/fwlink/?LinkId=526242). Pokud chcete databázi zašifrovat, připojte se jako její vlastník a spusťte tento příkaz:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Existují i jiné možnosti šifrování tajných dat:

* [Šifrování na úrovni buňky](https://msdn.microsoft.com/library/ms179331.aspx) umožňuje šifrovat určité sloupce nebo buňky dat pomocí různých šifrovacích klíčů.
* Pokud potřebujete modul hardwarového zabezpečení nebo chcete centrálně spravovat hierarchii šifrovacích klíčů, měli byste uvažovat o [řešení Azure Key Vault s SQL Serverem na virtuálním počítači v Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Funkce Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) (verze preview) činí šifrování přehlednějším pro aplikace a umožňuje klientům šifrovat citlivá data v klientských aplikacích bez sdílení šifrovacích klíčů se službou SQL Database.

## <a name="auditing"></a>Auditování
Auditování a sledování databázových událostí pomáhá dodržovat závazné předpisy a identifikovat podezřelé aktivity. Auditování služby SQL Database umožňuje zaznamenávat události v databázi do protokolu auditování v účtu Azure Storage. Auditování služby SQL Database je také integrované do služby Microsoft Power BI kvůli snadnějšímu procházení sestav a analýz. Další informace najdete v článku [Začínáme s auditem SQL Database](sql-database-auditing-get-started.md).

Detekce hrozeb ve službě SQL Database nabízí kromě auditování ještě další vrstvu zabezpečení. Bezpečnostní výstrahy na neobvyklé aktivity umožňují reagovat na hrozby hned, jak se objeví. Další informace najdete v článku [Začínáme zjišťovat hrozby ve službě SQL Database](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Dodržování předpisů
Kromě uvedených vlastností a funkcí, které aplikaci pomáhají splnit nejrůznější požadavky týkající se dodržování bezpečnostních předpisů, služba Azure SQL Database také podléhá pravidelným auditům a získala certifikáty pro celou řadu norem v oblasti dodržování předpisů. Další informace najdete v [Centru zabezpečení Microsoft Azure](https://azure.microsoft.com/support/trust-center/), kde také najdete nejnovější seznam [certifikátů služby SQL Database v oblasti dodržování předpisů](https://azure.microsoft.com/support/trust-center/services/).


## <a name="next-steps"></a>Další kroky

- Přehled všech funkcí zabezpečení služby SQL Database najdete v [přehledu zabezpečení SQL](sql-database-security-overview.md).
- Diskuzi o používání funkcí řízení přístupu ve službě SQL Database najdete v tématu [Řízení přístupu](sql-database-control-access.md).
- Diskuzi o proaktivním monitorování najdete v tématech [Začínáme s auditováním služby SQL Database](sql-database-auditing-get-started.md) a [Začínáme s detekcí hrozeb služby SQL Database](sql-database-threat-detection-get-started.md).



<!--HONumber=Dec16_HO4-->


