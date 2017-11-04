---
title: "Opravte chyby připojení SQL, přechodné chybě. | Microsoft Docs"
description: "Informace o odstraňování potíží, diagnostikovat a zabránit chyba připojení SQL nebo přechodné chybě ve službě Azure SQL Database. "
keywords: "připojení SQL, připojovací řetězec, problémy s připojením, přechodná chyba, došlo k chybě připojení"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: daleche
ms.openlocfilehash: dda284b45e2e8a35a7228d77afef0ad058c8ea42
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Oprava a diagnostika chyb připojení SQL a přechodných chyb služby SQL Database a jejich předcházení
Tento článek popisuje, jak zabránit, odstraňování, diagnostikovat a opravit chyby připojení a přechodné chyby, které klientské aplikace, zaznamená při komunikuje se službou Azure SQL Database. Zjistěte, jak nakonfigurovat logika opakovaných pokusů, sestavení připojovacího řetězce a další nastavení připojení.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Přechodné chyby (přechodné chyby)
Přechodná chyba - navíc přechodná chyba - má základní příčinu, který bude brzy vyřeší. Příležitostně příčinu přechodné chyby je, když Azure systému rychle posune hardwarové prostředky lepší vyrovnávání zatížení různé úlohy. Většinu těchto událostí Rekonfigurace často dokončit za méně než 60 sekund. Během zadaného období Rekonfigurace může mít problémy s připojením k databázi SQL Azure. Připojení k databázi SQL Azure aplikace by měly být vytvořeny očekávat tyto přechodné chyby, je zpracování, které implementují logiku opakovaných pokusů v svůj kód místo zpřístupnění je uživatelům jako chyby aplikace.

Pokud váš klientský program nepoužívá ADO.NET, je vašeho programu podle throw z vás o přechodné chybě vyzval **SqlException**. **Číslo** vlastnost může být porovná seznam přechodné chyby v horní části tématu: [kódy chyb SQL pro klientské aplikace SQL Database](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz
Budete pokus o připojení SQL nebo znovu vytvořit v závislosti na následující:

* **Při pokusu o připojení dojde k přechodné chybě**: připojení je třeba opakovat po zpozdit pro několik sekund.
* **Příkaz dotazu SQL došlo k přechodné chybě**: příkaz nesmí opakovat okamžitě. Místo toho po prodlevě, připojení by se mělo čerstvě vytvořit. Příkaz pak můžete opakovat.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Logika opakování pro přechodné chyby
Klient programy, které někdy dojde k přechodné chybě jsou robustnější při obsahují logiku opakovaných pokusů.

Pokud váš program komunikuje s Azure SQL Database pomocí 3. stran middleware, Dotázat se s dodavatelem zda middleware obsahuje logiku opakovaných pokusů pro přechodné chyby.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Zásady pro opakování
* Pokud je chyba přechodný je třeba opakovat pokus o otevření připojení.
* Příkaz SELECT, který selže s přechodná chyba by neměl přímo opakovat.
  
  * Místo toho udržování připojení a poté opakujte SELECT.
* Při aktualizaci SQL příkazu selže s přechodná chyba transakce, udržování připojení by se mělo vytvořit předtím, než se pokus o aktualizaci.
  
  * Logika opakovaných pokusů musí zajistit, že buď celé databáze transakce byla dokončena, nebo že celá transakce bude vrácena zpět.

#### <a name="other-considerations-for-retry"></a>Další důležité informace pro opakování
* Batch program, který se automaticky spustí po pracovní době, a před ráno, která dokončí si může dovolit velmi pacienta s dlouho časových intervalů mezi jeho počet pokusů o opakování.
* Program uživatelského rozhraní by měl účet pro lidské tendence, že uvolňovat po příliš dlouho čekání.
  
  * Však řešení nesmí být opakovat každých několik sekund, protože tyto zásady můžete vyplnění systému s žádostí.

#### <a name="interval-increase-between-retries"></a>Zvyšte interval mezi opakovanými pokusy
Doporučujeme, abyste zpoždění pro 5 sekund před první opakování. Opakování po prodlevě kratší než 5 sekund rizika zahltí cloudové služby. Pro každý další pokusy exponenciálnímu, růst zpoždění až do maximálního počtu 60 sekund.

Popis *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL sdružování připojení serveru (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Také můžete chtít nastavit maximální počet opakovaných pokusů, než program samoobslužné ukončí.

#### <a name="code-samples-with-retry-logic"></a>Ukázky kódu s logika opakovaných pokusů
Ukázky kódu s logikou opakování v různých programovacích jazyků, jsou k dispozici na:

* [Knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testování logika opakovaných pokusů
K testování logika opakovaných pokusů, musíte simulovat nebo způsobit chybu, než může být vyřešen při běhu programu.

##### <a name="test-by-disconnecting-from-the-network"></a>Test odpojení od sítě
Jedním ze způsobů, které můžete testovat logika opakovaných pokusů je klientský počítač odpojit od sítě, když je aplikace spuštěna. Chyba bude:

* **SqlException.Number** = 11001
* Zpráva: "znám žádný takový hostitel je"

Jako součást první pokus opakovat můžete program opravte chyby v pravopisu a pak se pokusíte připojit.

Chcete-li to praktické, můžete odpojit počítači ze sítě, než aplikace. Potom váš program rozpozná běhu parametr, který vede k:

1. 11001 dočasně přidáte do seznamu chyb vzít v úvahu jako přechodný.
2. Pokus první připojení jako obvykle.
3. Poté, co je chyba zachycení, odeberte 11001 ze seznamu.
4. Zobrazí zprávu, že připojit počítač k síti.
   * Další pozastavit provádění pomocí **Console.ReadLine** metoda nebo dialogovém okně tlačítko OK. Uživatel stisknutím klávesy Enter po počítače připojené k síti.
5. Pokusí znovu připojit, byla očekávána úspěch.

##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Test Chyba v pravopisu název databáze při připojování
Váš program můžete účelově chybně uživatelské jméno před první pokus o připojení. Chyba bude:

* **SqlException.Number** = 18456
* Zpráva: "přihlášení se nezdařilo pro uživatele 'WRONG_MyUserName'."

Jako součást první pokus opakovat můžete program opravte chyby v pravopisu a pak se pokusíte připojit.

Chcete-li to praktické, může váš program rozpoznat běhu parametr, který vede k:

1. Dočasně 18456 přidejte do jeho seznam chyb vzít v úvahu jako přechodný.
2. Přidejte účelově 'WRONG_' na jméno uživatele.
3. Poté, co je chyba zachycení, odeberte 18456 ze seznamu.
4. Odeberte 'WRONG_' uživatelského jména.
5. Pokusí znovu připojit, byla očekávána úspěch.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parametry objektu SqlConnection .NET pro opakování připojení
Pokud váš klientský program připojí k databázi SQL Azure s použitím třídy rozhraní .NET Framework **System.Data.SqlClient.SqlConnection**, měli byste použít .NET 4.6.1 nebo novější (nebo .NET Core), můžete použít funkci opakování jeho připojení. Podrobnosti o funkce jsou [zde](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Při vytváření [připojovací řetězec](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) pro vaše **SqlConnection** objektu, by měly koordinovat hodnoty mezi následujícími parametry:

* ConnectRetryCount &nbsp; &nbsp; *(výchozí hodnota je 1. Rozsah je 0 až 255).*
* ConnectRetryInterval &nbsp; &nbsp; *(výchozí hodnota je 1 sekunda. Rozsah je 1 až 60).*
* Časový limit připojení &nbsp; &nbsp; *(výchozí hodnota je 15 sekund. Rozsah hodnot je 0 až 2147483647)*

Konkrétně vybrané hodnoty měli následující true rovnosti:

* Časový limit připojení = ConnectRetryCount * ConnectionRetryInterval

Například pokud je počet = 3 a interval = 10 sekund, vypršení časového limitu z jen 29 sekund nebude poměrně poskytnout systému dostatek času pro jeho 3. a finální opakování v připojení: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz
**ConnectRetryCount** a **ConnectRetryInterval** parametry umožní vaše **SqlConnection** objekt opakujte operaci připojení bez informuje nebo bothering programu, například vrácení řízení vašeho programu. Opakované pokusy se může objevit v následujících situacích:

* volání metody mySqlConnection.Open
* volání metody mySqlConnection.Execute

Není k dispozici subtlety. Pokud dojde k přechodné chybě. při vaší *dotazu* je spouštěna, vaše **SqlConnection** objekt neopakuje operace připojení a jeho určitě neopakuje dotazu. Ale **SqlConnection** velmi rychle zkontroluje připojení před odesláním dotazu pro provedení. Pokud Rychlá kontrola zjistí problému s připojením **SqlConnection** opakování operace připojení. Pokud opakovaném úspěšné, odešle dotaz je pro provedení.

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Měli ConnectRetryCount nelze kombinovat s logika opakovaných pokusů aplikace?
Předpokládejme, že aplikace má robustní vlastní opakování logiku. Může opakujte operaci připojení 4 časy. Pokud přidáte **ConnectRetryInterval** a **ConnectRetryCount** = 3 pro připojovací řetězec, zvýšíte počet opakování do 4 * 3 = 12 opakování. Hodláte nemusí takové velký počet opakování.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Připojení k databázi Azure SQL
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Připojení: Připojovací řetězec
Připojovací řetězec potřebné pro připojení k databázi SQL Azure se mírně liší z řetězce pro připojení k serveru Microsoft SQL Server. Připojovací řetězec můžete zkopírovat pro vaši databázi z [portál Azure](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Připojení: IP adresa
Je nutné nakonfigurovat databázi SQL serveru tak, aby přijímal komunikaci z IP adresy počítače, který hostuje váš klientský program. To uděláte tak, že upravíte nastavení brány firewall pomocí [portál Azure](https://portal.azure.com/).

Pokud zapomenete konfigurace IP adresy, váš program selže s užitečný chybovou zprávu, která stavy nezbytné IP adresu.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Další informace najdete v tématu: [postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Připojení: porty
Obvykle potřebujete jenom zajistěte, aby byl port 1433 otevřený pro odchozí komunikaci, na počítači, který je hostitelem program klienta.

Například pokud je váš klientský program hostované na počítači se systémem Windows, brána Windows Firewall na hostiteli vám umožní otevřít port 1433:

1. Otevřete ovládací panely
2. &gt;Všechny položky Ovládacích panelů
3. &gt;Brána Windows Firewall
4. &gt;Upřesnit nastavení
5. &gt;Odchozí pravidla
6. &gt;Akce
7. &gt;Nové pravidlo

Pokud je váš klientský program hostované na virtuální počítač Azure (VM), měli byste si přečíst:<br/>[Porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Základní informace o konfigurace portů a IP adresy, najdete v tématu: [brány firewall databáze Azure SQL Database](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Připojení: ADO.NET 4.6.1
Pokud váš program používá třídy ADO.NET jako **System.Data.SqlClient.SqlConnection** pro připojení k databázi SQL Azure, doporučujeme použít rozhraní .NET Framework verze 4.6.1 nebo vyšší.

ADO.NET 4.6.1:

* Pro databázi SQL Azure, je lepší spolehlivostí při otevření připojení pomocí **SqlConnection.Open** metoda. **Otevřete** metoda nyní zahrnuje nejlepší mechanismy úsilí opakování v reakci na přechodných chyb pro určitým chybám v časovém limitu připojení.
* Podporuje sdružování připojení. To zahrnuje efektivní ověření, který funguje objekt připojení nabízí vašeho programu.

Použijete-li objekt připojení z fondu připojení, doporučujeme vám, že vašeho programu dočasně uzavřít připojení, když není okamžitě používat. Opakovaném otevření připojení není nákladné, je způsob vytvoření nového připojení.

Pokud používáte ADO.NET 4.0 nebo starší, doporučujeme upgradovat na nejnovější ADO.NET.

* Od listopadu 2015, můžete [stáhnout ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostika
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostika: Test, zda může připojit nástroje
Pokud váš program se nedaří připojit k databázi SQL Azure, je pokusí připojit k programu nástroj jednu možnost diagnostiky. V ideálním případě by nástroj připojit pomocí stejné knihovny, který program používá.

Na libovolném počítači Windows můžete vyzkoušet tyto nástroje:

* SQL Server Management Studio (ssms.exe), který se připojuje pomocí ADO.NET.
* Sqlcmd.exe, který se připojuje pomocí [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

Po připojení otestujte, zda funguje krátké SQL SELECT dotazu.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostika: Zkontrolujte otevřené porty
Předpokládejme, že máte podezření, že pokusy o připojení se nedaří kvůli problémům s portu. V počítači můžete spustit nástroj, který sestavy na konfiguraci portů.

V systému Linux mohou být užitečné následující nástroje:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (Změnit příklad hodnota, která má být IP adresa.)

V systému Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) nástroj může být užitečné. Tady je příklad spuštění, dotaz situace port na serveru Azure SQL Database a která byla spuštěna na přenosný počítač:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostiky: Chyby protokolu
Občasný problém je někdy nejlépe zjištěném detekce obecné vzoru přes dny nebo týdny.

Váš klient může být užitečné při diagnostiku protokolováním všechny chyby, který nalezne. Je možné ke korelaci položky protokolu s daty chyba, která interně Azure SQL Database samotné protokoly.

Enterprise knihovny 6 (EntLib60) nabízí spravované rozhraní .NET třídy vám pomůže při protokolování:

* [5 - stejně snadná jako návratem vypnout protokolu: používání bloku protokolování aplikace](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostika: Zkontrolujte protokoly systému chyby
Tady jsou některé příkazy jazyka Transact-SQL, vyberte tento dotaz protokoly chyby a dalšími informacemi.

| Dotaz protokolu | Popis |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |[Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) zobrazení nabízí informace o jednotlivých událostech, včetně některých, která mohou způsobit přechodné chyby nebo chyby připojení.<br/><br/>V ideálním případě mohou korelovat **start_time** nebo **end_time** hodnoty s informacemi o když váš klientský program došlo k potížím.<br/><br/>**TIP:** musíte se připojit k **hlavní** databázi, aby ji spustit. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |[Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) zobrazení nabízí souhrnné počty typů událostí pro další diagnostiku.<br/><br/>**TIP:** musíte se připojit k **hlavní** databázi, aby ji spustit. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostika: Vyhledejte problém události v protokolu databáze SQL
Můžete hledat položky o problém události v protokolu databáze SQL Azure. Zkuste následující příkaz Transact-SQL, vyberte v **hlavní** databáze:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Několik řádků vrácená z sys.fn_xe_telemetry_blob_target_read_file
Dále je, jak může vypadat vrácený řádek. Hodnoty null, zobrazí se často není null v dalších řádcích.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Knihovna Enterprise 6
Enterprise knihovny 6 (EntLib60) je architektura tříd rozhraní .NET usnadňující implementovat robustní klienti cloudové služby, z nichž jeden je služba Azure SQL Database. Můžete vyhledat témata vyhrazený pro každou oblast, ve kterém může být užitečné EntLib60 první navštivte stránky:

* [Knihovna Enterprise 6 – duben 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Logika opakovaných pokusů pro přechodné chyby zpracování je jedné oblasti, ve kterém může být užitečné EntLib60:

* [4 - perseverance, tajný klíč všechny vítězství: použití bloku aplikace přechodná chyba zpracování](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> Zdrojový kód pro EntLib60 je k dispozici pro veřejné [Stáhnout](http://go.microsoft.com/fwlink/p/?LinkID=290898). Společnost Microsoft nemá žádné plány pro EntLib provést další funkce aktualizace nebo aktualizace údržby.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Třídy EntLib60 pro přechodné chyby a zkuste to znovu
Následující třídy EntLib60 jsou obzvláště užitečná pro logiku opakovaných pokusů. Všechny tyto v, nebo další pod oborem názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*V oboru názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **RetryPolicy** – třída
  
  * **ExecuteAction** – metoda
* **ExponentialBackoff** – třída
* **SqlDatabaseTransientErrorDetectionStrategy** – třída
* **ReliableSqlConnection** – třída
  
  * **Parametr ExecuteCommand** – metoda

V oboru názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** – třída
* **NeverTransientErrorDetectionStrategy** – třída

Tady jsou odkazy na informace o EntLib60:

* Volné [sešit stažení: Příručka pro vývojáře do knihovny Microsoft Enterprise, verze 2.](http://www.microsoft.com/download/details.aspx?id=41145)
* Osvědčené postupy: [opakujte obecné pokyny](../best-practices-retry-general.md) má vynikající podrobné informace o logika opakovaných pokusů.
* Stahování NuGet [Enterprise Library - přechodných chyb aplikace bloku 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Protokolování bloku
* Blok protokolování je velmi flexibilní a konfigurovat řešení, které vám umožní:
  
  * Vytvoření a uložení zprávy protokolu v celé řadě umístění.
  * Kategorizaci a filtrování zprávy.
  * Shromážděte kontextové informace, které jsou užitečné pro ladění a trasování, a také pro auditování a obecné protokolování požadavky.
* Protokolování bloku abstrahuje funkce protokolování cíl protokolu tak, aby je konzistentní, bez ohledu na umístění a typ úložiště cíl protokolování kódu aplikace.

Podrobnosti najdete v tématu: [5 – jako snadno jako návratem vypnout protokolu: pomocí blokem protokolování aplikace](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient metoda zdrojového kódu
Další z **SqlDatabaseTransientErrorDetectionStrategy** třídy, je C# zdrojový kód **IsTransient** metoda. Zdrojový kód vysvětluje, které chyby se považuje za přechodný a worthy z opakování od dubna 2013.

Řada **//comment** řádky byly odebrány z této kopie zdůraznit čitelnost.

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Další kroky
* Řešení potíží s další běžné problémy s připojením databáze SQL Azure, najdete v článku [řešení problémů s připojením k databázi SQL Azure](sql-database-troubleshoot-common-connection-issues.md).
* [Připojení k serveru SQL sdružování (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [*Opakováním* je Apache 2.0 licenci pro obecné účely, opakování knihovny, které jsou napsané v **Python**, aby se zjednodušila úkolu přidávání opakování chování pro jakoukoli.](https://pypi.python.org/pypi/retrying)

