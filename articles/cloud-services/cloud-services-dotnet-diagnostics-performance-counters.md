---
title: "Použití čítačů výkonu v konzoli Azure Diagnostics | Microsoft Docs"
description: "Čítače výkonu v Azure cloudové služby nebo virtuálního počítače používejte k vyhledání kritická místa a optimalizaci výkonu."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: 2cf765cb034725199127c547a9b8b997a4a6089c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Vytváření a používání čítače výkonu v aplikaci Azure
Tento článek popisuje výhody a jak převést čítače výkonu do aplikace Azure. Můžete je používat pro shromažďování dat, najít kritická místa a vyladit výkon systému a aplikací.

Čítače výkonu, které jsou k dispozici pro Windows Server, služba IIS a ASP.NET můžete také shromažďovat a používá k určení stavu Azure webové role, role pracovního procesu a virtuálních počítačů. Můžete také vytvořit a použít vlastní čítače výkonu.  

Můžete zkontrolovat data čítače výkonu

1. Přímo na hostiteli aplikace pomocí nástroje Sledování výkonu přistupovat pomocí vzdálené plochy
2. S nástrojem System Center Operations Manager pomocí sady pro správu Azure
3. Pomocí jiných nástrojů monitorování, které přístup diagnostických dat přenést do úložiště Azure. V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) Další informace.  

Další informace o monitorování výkonu aplikace v [portál Azure](http://portal.azure.com/), najdete v části [postup monitorování cloudové služby](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Další podrobné pokyny k vytváření protokolování a trasování strategie a pomocí diagnostiky a další postupy k řešení problémů a optimalizaci aplikace Azure, najdete v části [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Povolit monitorování čítače výkonu
Ve výchozím nastavení nejsou povoleny čítače výkonu. Aplikace nebo úloha spuštění musíte změnit výchozí konfiguraci agenta diagnostiky zahrnout čítače výkonu konkrétní, které chcete sledovat pro každou instanci role.

### <a name="performance-counters-available-for-microsoft-azure"></a>Čítače výkonu, které jsou k dispozici pro Microsoft Azure
Azure poskytuje podmnožinu dostupných čítačů výkonu pro Windows Server, služba IIS a ASP.NET zásobníku. Následující tabulka uvádí některé čítače výkonu konkrétní týkající se aplikace Azure.

| Kategorie čítače: Objekt (Instance) | Název čítače | Referenční informace |
| --- | --- | --- |
| CLR – výjimky na rozhraní .NET (*globální*) |# Výjimek / s |Čítače výkonu výjimky |
| Využívání paměti rozhraním .NET CLR (*globální*) |Čas |Čítače výkonu paměti |
| ASP.NET |Restartování aplikace |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Čas provádění požadavku |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Odpojené požadavky |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Restartování pracovního procesu |Čítače výkonu pro technologii ASP.NET |
| Aplikace ASP.NET (**celkový**) |Celkový počet požadavků |Čítače výkonu pro technologii ASP.NET |
| Aplikace ASP.NET (**celkový**) |Počet požadavků za sekundu |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Čas provádění požadavku |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Čekací doba požadavku |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Aktuální požadavky |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Požadavky ve frontě |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Zamítnuté požadavky |Čítače výkonu pro technologii ASP.NET |
| Memory (Paměť) |Počet MB k dispozici |Čítače výkonu paměti |
| Memory (Paměť) |Svěřené bajty |Čítače výkonu paměti |
| Procesor(_celkem) |% Času procesoru |Čítače výkonu pro technologii ASP.NET |
| TCPv4 |Počet selhání připojení |Objekt TCP |
| TCPv4 |Vytvořeno připojení |Objekt TCP |
| TCPv4 |Resetování připojení |Objekt TCP |
| TCPv4 |Segmenty odeslaných za sekundu |Objekt TCP |
| Interface(*) sítě |Přijaté bajty/s |Objekt rozhraní sítě |
| Interface(*) sítě |Odeslané bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Přijaté bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Odeslané bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Čítač Bajty celkem/s |Objekt rozhraní sítě |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Vytvořit a přidat vlastní čítače výkonu do vaší aplikace
Azure má podporu vytvářet a upravovat vlastní čítače výkonu pro webových rolí a rolí pracovního procesu. Čítače lze sledovat a monitorovat chování specifické pro aplikaci. Můžete vytvořit a kategorie čítače výkonu vlastní a specifikátory odstranit z úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.

> [!NOTE]
> Kód, který provede změny vlastní čítače výkonu, musí mít zvýšená oprávnění ke spuštění. Pokud je kód v webovou roli nebo role pracovního procesu, role musí obsahovat značky <Runtime executionContext="elevated" /> v souboru ServiceDefinition.csdef pro roli tak, aby správně inicializovat.
>
>

Můžete odeslat data čítače výkonu vlastní do úložiště Azure pomocí diagnostiky agenta.

Data čítače standardní výkonu je generován Azure procesy. Data čítače výkonu vlastní musí být vytvořeny webovou aplikací role role nebo pracovního procesu. V tématu [typy čítačů výkonu](https://msdn.microsoft.com/library/z573042h.aspx) informace o typech dat, která mohou být uloženy ve vlastní čítače výkonu. V tématu [Ukázka čítače výkonu](http://code.msdn.microsoft.com/azure/) pro příklad, který vytvoří a nastaví data čítače výkonu vlastní ve webové roli.

## <a name="store-and-view-performance-counter-data"></a>Úložiště a zobrazení data čítače výkonu
Azure data čítače výkonu pomocí jiné diagnostické informace ukládá do mezipaměti. Tato data jsou k dispozici pro vzdálené monitorování, když role instance běží, chcete-li zobrazit nástroje, jako je monitorování výkonu pomocí přístup ke vzdálené ploše. Zachování dat mimo instanci role, agent diagnostiky data musíte přenést do úložiště Azure. Omezení velikosti data čítače výkonu v mezipaměti se dá nakonfigurovat v agenta diagnostiky nebo může být nakonfigurována jako součást sdílených limitu diagnostická data. Další informace o nastavení velikosti vyrovnávací paměti najdete v tématu [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) a [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) přehled nastavení diagnostiky agenta k přenosu dat do účtu úložiště.

Každá instance čítače výkonu nakonfigurované se zaznamenává v zadané vzorkovací frekvenci a jen Vzorkovaná data se přenáší k účtu úložiště tak, že žádost o přenos plánované nebo žádost o přenos na vyžádání. Automatické přenosy může být naplánovaná tak často, jak jednou za minutu. Přenést agentem diagnostiky data čítače výkonu je uložena v tabulce, WADPerformanceCountersTable, v účtu úložiště. Tato tabulka může získat přístup a dotaz pomocí metod úložiště Azure úrovně standard rozhraní API. V tématu [Ukázka čítače výkonu Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) příklad dotazování a zobrazení data čítače výkonu z tabulky WADPerformanceCountersTable.

> [!NOTE]
> V závislosti na Diagnostika agenta přenosu četnost a fronty latence nejnovější data čítače výkonu v účtu úložiště může být zastaralý několik minut.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Povolit čítače výkonu pomocí diagnostiky konfiguračního souboru
Pomocí následujícího postupu umožníte čítače výkonu v aplikaci Azure.

## <a name="prerequisites"></a>Požadavky
Této části se předpokládá, že máte importovat monitorování diagnostiky aplikace a přidat konfiguračního souboru diagnostiku do řešení sady Visual Studio (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx SDK 2.5 nebo novější). Informace o kroky 1 a 2 v [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md)) Další informace.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Krok 1: Shromažďovat a ukládat data z čítače výkonu
Po přidání souboru diagnostiky k řešení sady Visual Studio, můžete nakonfigurovat kolekce a ukládání data čítače výkonu v aplikaci Azure. To se provádí přidáním čítačů výkonu k souboru diagnostiky. Diagnostická data, včetně čítačů výkonu, je nejprve shromážděné v instanci. Data se pak jako trvalý, do tabulky WADPerformanceCountersTable ve službě Azure Table, budete taky muset zadat účet úložiště ve vaší aplikaci. Pokud testujete aplikace místně v emulátoru výpočetní, můžete také ukládat diagnostická data místně v emulátoru úložiště. Než bude ukládat data diagnostiky, musíte nejdřív přejít na [portál Azure](http://portal.azure.com/) a vytvoření účtu úložiště classic. Osvědčeným postupem je najít váš účet úložiště ve stejném geograficky umístění jako vaše aplikace Azure. Podle uchování aplikací Azure a účet úložiště se v stejného geografického umístění, vyhněte se platícího náklady na externí šířku pásma a snižování latence.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Přidání čítačů výkonu k souboru diagnostiky
Existuje mnoho čítačů, které můžete použít. Následující příklad ukazuje několik čítače výkonu, které se doporučují pro webové a pracovní role monitorování.

Otevřete soubor diagnostiky (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující DiagnosticMonitorConfiguration element:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Atribut bufferQuotaInMB, který určuje maximální množství úložiště systému souborů, které je dostupné pro vybraný typ kolekce dat (Azure protokoly, protokoly služby IIS, atd.). Výchozí hodnota je 0. Když je dosaženo kvóty, nejstarší data je odstranit, protože se přidá nová data. Součet všech vlastností bufferQuotaInMB musí být větší než hodnota atributu OverallQuotaInMB. Podrobnější informace o určení, jak velké úložiště se bude vyžadovat pro sběr dat diagnostiky, najdete v části instalace WAD [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

ScheduledTransferPeriod atribut, který určuje interval mezi naplánované přenosů dat, zaokrouhlený nahoru na nejbližší minutu. V následujících příkladech jinak je nastavená na PT30M (30 minut). Nastavení v období na malou hodnotu, jako je 1 minuta, bude nepříznivě ovlivnit výkon vaší aplikace v provozním prostředí, ale může být užitečná pro vidět diagnostiky rychle práce při testování. V naplánované období musí být dostatečně malé, aby Ujistěte se, že není přepsán diagnostických dat v instanci, ale dostatečně velké na to, že nebude mít dopad na výkon vaší aplikace.

Atribut counterSpecifier Určuje čítače výkonu ke shromažďování. Atribut sampleRate určuje rychlost, jakou čítače výkonu se odeberou, v takovém případě 30 sekund.

Po přidání čítačů výkonu, které chcete shromáždit, uložte změny do souboru diagnostiky. Dále je třeba zadat účet úložiště, který bude formou diagnostická data.

### <a name="specify-the-storage-account"></a>Zadejte účet úložiště
Chcete-li zachovat vaše diagnostické informace k vašemu účtu úložiště Azure, musíte zadat připojovací řetězec v souboru konfigurace (souboru ServiceConfiguration.cscfg) služby.

Pro Azure SDK 2.5 můžete zadat účet úložiště v souboru diagnostics.wadcfgx.

> [!NOTE]
> Tyto pokyny platí pouze Azure SDK 2.4 a níže. Pro Azure SDK 2.5 můžete zadat účet úložiště v souboru diagnostics.wadcfgx.
>
>

Chcete-li nastavit připojovací řetězce:

1. Otevřete soubor ServiceConfiguration.Cloud.cscfg pomocí svém oblíbeném textovém editoru a nastavte připojovací řetězec pro úložiště. *AccountName* a *AccountKey* hodnoty se nacházejí v portálu Azure v řídicím panelu účet úložiště, v části přístupové klíče.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Uložte soubor ServiceConfiguration.Cloud.cscfg.
3. Otevřete soubor ServiceConfiguration.Local.cscfg a ověřte, zda je UseDevelopmentStorage nastavena na hodnotu true.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Teď, když jsou nastavené připojovací řetězce, aplikace se uchová diagnostická data do účtu úložiště při nasazení vaší aplikace.
4. Uložte a sestavení projektu a nasazení aplikace.

## <a name="step-2-optional-create-custom-performance-counters"></a>Krok 2: (Volitelné) vytvořit vlastní čítače výkonu
Kromě předdefinovaných čítače můžete přidat vlastní vlastní čítače výkonu pro monitorování role web nebo worker. Vlastní čítače výkonu lze sledovat a monitorovat chování specifické pro aplikaci a můžou vytvořit nebo odstranit v úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.

Agent Azure diagnostics aktualizuje výkonu čítač konfigurace ze souboru .wadcfg jednu minutu po spuštění.  Pokud vytvoříte vlastní čítače výkonu v metodě OnStart a úlohami spuštění trvá déle než minutu provést, vaše vlastní čítače výkonu nebudou byl vytvořen agenta Azure Diagnostics pokusí načíst je.  V tomto scénáři uvidíte, že Azure Diagnostics správně zaznamená všechny diagnostická data s výjimkou vaše vlastní čítače výkonu.  Chcete-li vyřešit tento problém, vytvořte čítače výkonu v úloze pro spuštění nebo přesunout některé vaše úloha spuštění metody OnStart fungovat po vytvoření čítače výkonu.

Proveďte následující kroky k vytvoření jednoduché vlastní výkon čítač s názvem "\MyCustomCounterCategory\MyButton1Counter":

1. Otevřete soubor definice služby (CSDEF) pro vaši aplikaci.
2. Přidáte element Runtime WebRole nebo WorkerRole elementu, který chcete povolit spuštění se zvýšenými oprávněními:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Uložte soubor.
4. Otevřete soubor diagnostiky (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující DiagnosticMonitorConfiguration

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Uložte soubor.
6. Vytvoření kategorie čítače výkonu vlastní metoda OnStart role, před vyvoláním base. OnStart. Následující příklad jazyka C# vytvoří vlastní kategorii, pokud ještě neexistuje:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Aktualizace čítačů v rámci vaší aplikace. Následující příklad aktualizací čítače výkonu vlastní Button1_Click událostí:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Uložte soubor.  

Data čítače výkonu vlastní budou shromažďována teď monitorování Azure diagnostics.

## <a name="step-3-query-performance-counter-data"></a>Krok 3: Dotaz na data čítače výkonu
Jakmile se vaše aplikace je nasazená a spuštěna, bude zahájeno monitorování diagnostiky shromažďování čítačů výkonu a uložením dat do úložiště Azure. Pomocí nástrojů, jako je Průzkumníka serveru v sadě Visual Studio, [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), nebo [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) podle Cerebrata Chcete-li zobrazit čítače výkonu dat v tabulce WADPerformanceCountersTable. Můžete také programově dotazovat pomocí služby Table [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), nebo [PHP](../cosmos-db/table-storage-how-to-use-php.md).

V následujícím příkladu C# ukazuje základní dotaz s tabulkou WADPerformanceCountersTable a uloží diagnostická data do souboru CSV. Po čítače výkonu se uloží do souboru CSV, můžete k vizualizaci dat grafovým možnosti v aplikaci Microsoft Excel nebo nějaký jiný nástroj. Nezapomeňte přidat odkaz na Microsoft.WindowsAzure.Storage.dll, který je obsažen v sadě Azure SDK pro .NET říjen 2012 a novějších. Sestavení je nainstalována k adresáři % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entity se mapují na objekty C# pomocí vlastní třídy odvozené od **TableEntity**. Následující kód definuje třídu entity, který představuje čítače výkonu ve **WADPerformanceCountersTable** tabulky.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Další kroky
[Zobrazit další články v Azure Diagnostics](../azure-diagnostics.md)
