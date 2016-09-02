## Nastavení vývojového prostředí

Potom si nastavte vývojové prostředí v sadě Visual Studio, abyste byli připraveni vyzkoušet příklady kódů z této příručky.

### Vytvoření projektu konzolové aplikace pro Windows

V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows, jak je uvedeno níže:

![Vytvoření konzolové aplikace pro Windows](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Všechny příklady kódu v tomto kurzu můžete přidat do metody **Main()** v souboru `program.cs` z vaší konzolové aplikace.

Všimněte si, že můžete použít knihovnu klienta služby Azure Storage z libovolného typu .NET, včetně cloudové služby Azure, webové aplikace Azure, desktopové nebo mobilní aplikace. V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.

### Použití balíčku NuGet k instalaci požadovaných balíčků

Abyste mohli tento kurz dokončit, nainstalujte si do projektu dva nezbytné balíčky:

- [Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup k datovým prostředkům na účtu úložiště.
- [Microsoft Azure Configuration Manager library for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce z konfiguračního souboru bez ohledu na to, kde je aplikace spuštěná.

K získání obou balíčků můžete použít balíček NuGet. Postupujte následovně:

1. Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.
2. Proveďte online hledání textu „WindowsAzure.Storage“ a kliknutím na **Instalovat** nainstalujte balíček knihovny klienta úložiště a jeho závislosti.
3. Proveďte online hledání textu „ConfigurationManager“ a kliknutím na **Instalovat** nainstalujete správce konfigurace Azure.

>[AZURE.NOTE] Balíček knihovny klienta služby Storage je také součástí [sady Azure SDK for .NET](https://azure.microsoft.com/downloads/). Přesto doporučujeme, abyste si knihovnu klienta služby Storage nainstalovali také z balíčku NuGet, protože tím si zajistíte nejnovější verzi knihovny klienta.
>
>Závislosti ODataLib v knihovně klienta služby Storage pro .NET jsou vyřešené prostřednictvím balíčků ODataLib (verze 5.0.2 a novější), které jsou dostupné prostřednictvím balíčku NuGet a ne prostřednictvím služby WCF Data Services. Knihovny ODataLib můžete stáhnout přímo nebo z odkazu ve vašem kódovém projektu prostřednictvím balíčku NuGet. Konkrétní balíčky ODataLib používané knihovnou klienta služby Storage jsou [OData](http://nuget.org/packages/Microsoft.Data.OData/5.0.2), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/5.0.2), a [Spatial](http://nuget.org/packages/System.Spatial/5.0.2). I když tyto knihovny používají třídy Azure Table Storage, představují požadované závislosti pro programování s knihovnou klienta úložiště.


### Určení cílového prostředí

Ke spuštění příkladů z této příručky máte dvě možnosti prostředí:

- Svůj kód můžete spustit na účtu služby Azure Storage v cloudu. 
- Svůj kód můžete spustit v emulátoru úložiště Azure. Emulátor úložiště je místní prostředí, které emuluje účet služby Azure Storage v cloudu. Emulátor je bezplatnou možností pro testování a ladění kódu během vývoje aplikace. Emulátor používá známý účet a klíč. Další podrobnosti najdete v článku [Použití emulátoru služby Azure Storage pro vývoj a testování](../articles/storage/storage-use-emulator.md)

Pokud se zaměřujete na účet úložiště v cloudu, zkopírujte z webu Azure Portal primární přístupový klíč svého účtu úložiště. Další informace najdete v článku [Zobrazení a kopírování přístupového klíče k úložišti](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [AZURE.NOTE] Pokud se chcete vyhnout nákladům spojeným se službou Azure Storage, můžete se zaměřit na emulátor úložiště. I když se zaměříte na účet úložiště Azure v cloudu, budou náklady na vyzkoušení postupů z tohoto kurzu zanedbatelné.

### Konfigurace připojovacího řetězce úložiště

Knihovna klienta služby Azure Storage pro .NET podporuje použití připojovacího řetězce úložiště ke konfiguraci koncových bodů a přihlašovacích údajů pro přístup ke službám úložiště. Připojovací řetězec úložiště se nejlépe uchovává v konfiguračním souboru. 

Další informace o připojovacích řetězcích najdete v článku [Konfigurace připojovací řetězce pro službu Azure Storage](../articles/storage/storage-configure-connection-string.md).

> [AZURE.NOTE] Klíč účtu úložiště je podobný kořenovému heslu vašeho účtu úložiště. Vždy klíč účtu úložiště pečlivě chraňte. Nedávejte ho jiným uživatelům, nezakódovávejte ho ani ho neukládejte do souboru ve formátu prostého textu, který je přístupný ostatním uživatelům. Pokud se domníváte, že je klíč ohrožený, vygenerujte ho znovu pomocí webu Azure Portal.

Pokud chcete konfigurovat připojovací řetězec, otevřete v sadě Visual Studio Průzkumníka řešení a v něm soubor `app.config`. Přidejte obsah níže uvedeného prvku `<appSettings>`. `account-name` nahraďte názvem svého účtu úložiště a místo `account-key` zadejte svůj klíč účtu úložiště:

    <configuration>
        <startup> 
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
        </startup>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
        </appSettings>
    </configuration>

Nastavení konfigurace může vypadat následovně:

    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln6fT7mvY+rxu2iWAEyzPKITGkhM88J8HUoyofvK7C6fHcZc2kRZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />

Pokud se chcete zaměřit na emulátor úložiště, můžete vytvořit zástupce, který se namapuje na název a klíč známého účtu. V takovém případě bude nastavení připojovacího řetězce vypadat následovně:

    <add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />




<!--HONumber=Aug16_HO4-->


