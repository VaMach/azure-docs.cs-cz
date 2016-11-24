---
title: "Začínáme s Azure File Storage ve Windows | Dokumentace Microsoftu"
description: "Ukládejte data souborů do cloudu pomocí Azure File Storage a připojte svou cloudovou sdílenou složku z virtuálního počítače Azure nebo z lokální aplikace s Windows."
services: storage
documentationcenter: .net
author: mine-msft
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/18/2016
ms.author: minet
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: bf8acbd37f717c0e055facde781966665ca67d2d


---
# <a name="get-started-with-azure-file-storage-on-windows"></a>Začínáme s úložištěm Azure File ve Windows
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

[!INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

Informace o používání File Storage s Linuxem najdete v tématu [Používání Azure File Storage s Linuxem](storage-how-to-use-files-linux.md).

Informace o škálovatelnosti a cílech výkonnosti pro File Storage najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files).

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

## <a name="video-using-azure-file-storage-with-windows"></a>Video: Používání Azure File Storage s Windows
Toto video ukazuje, jak ve Windows vytvořit a používat sdílené složky v Azure File.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-File-Storage-with-Windows/player]
> 
> 

## <a name="about-this-tutorial"></a>O tomto kurzu
Tento úvodní kurz ukazuje základy používání Microsoft Azure File Storage. V tomto kurzu provedeme následující:

* Pomocí Portálu Azure nebo PowerShellu vytvoříme novou sdílenou složku Azure File, přidáme adresář, uložíme místní soubor do sdílené složky a zobrazíme seznam souborů v adresáři.
* Připojte sdílenou složku tak, jako byste připojili jakoukoli jinou sdílenou složku SMB.
* Pro přístup ke sdílené složce z lokální aplikace použijte Klientskou knihovnu pro úložiště Azure pro .NET. Vytvořte konzolovou aplikaci a proveďte tyto akce se sdílenou složkou:
  * Vypište obsah souboru ve sdílené složce do okna konzoly.
  * Nastavte kvótu (maximální velikost) pro sdílenou složku.
  * Vytvořte sdílený přístupový podpis pro soubor, který používá zásady sdíleného přístupu definované ve sdílené složce.
  * Zkopírujte soubor do jiného souboru ve stejném účtu úložiště.
  * Zkopírujte soubor do objektu blob ve stejném účtu úložiště.
* Použijte Azure Storage Metrics pro řešení potíží.

Úložiště souborů se teď podporuje pro všechny účty úložiště, takže buď můžete použít existující účet úložiště, nebo můžete vytvořit nový účet úložiště. Další informace o tom, jak vytvořit nový účet úložiště najdete v tématu [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account).

## <a name="use-the-azure-portal-to-manage-a-file-share"></a>Použijte Portál Azure ke správě složky
[Azure Portal](https://portal.azure.com) zákazníkům nabízí uživatelské prostředí pro správu sdílených složek. Z portálu můžete:

* Vytvořit sdílenou složku
* Ukládat soubory do sdílené složky a stahovat soubory ze sdílené složky
* Sledovat aktuální využití každé sdílené složky
* Upravit kvótu velikosti sdílené složky
* Pomocí příkazu `net use` připojit sdílenou složku z klienta Windows

### <a name="create-file-share"></a>Vytvoření sdílené složky
1. Přihlaste se k portálu Azure.
2. V navigační nabídce klikněte na **Účty úložiště** nebo **Účty úložiště (klasické)**.
   
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-dotnet-how-to-use-files/files-create-share-0.png)
3. Vyberte svůj účet úložiště
   
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-dotnet-how-to-use-files/files-create-share-1.png)
4. Vyberte službu „Files“.
   
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-dotnet-how-to-use-files/files-create-share-2.png)
5. Klikněte na „Sdílené složky“ a pomocí odkazu vytvořte svou první sdílenou složku.
   
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-dotnet-how-to-use-files/files-create-share-3.png)
6. Zadejte název sdílené složky a její velikost (max. 5120 GB) a vaše první složka se vytvoří. Po vytvoření sdílené složky ji můžete připojit z jakéhokoli souborové systému, který podporuje SMB 2.1 nebo SMB 3.0.
   
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-dotnet-how-to-use-files/files-create-share-4.png)

### <a name="upload-and-download-files"></a>Ukládání a stahování souborů
1. Vyberte jednu sdílenou složku, kterou jste už vytvořili.
   
    ![Snímek obrazovky, který ukazuje ukládání a stahování souborů z portálu.](./media/storage-dotnet-how-to-use-files/files-upload-download-1.png)
2. Klikněte na **Uložit** a otevře se uživatelské prostředí pro ukládání souborů.
   
    ![Snímek obrazovky, který ukazuje ukládání souborů z portálu.](./media/storage-dotnet-how-to-use-files/files-upload-download-2.png)
3. Klikněte pravým tlačítkem na jeden soubor a vyberte možnost **Stáhnout**, soubor se stáhne do místního úložiště.
   
    ![Snímek obrazovky, který ukazuje stahování souborů z portálu.](./media/storage-dotnet-how-to-use-files/files-upload-download-3.png)

### <a name="manage-file-share"></a>Správa sdílené složky
1. Klikněte na **Kvóta** a změňte velikost sdílené složky (až 5120 GB).
   
    ![Snímek obrazovky, který ukazuje nastavení kvóty pro sdílenou složku.](./media/storage-dotnet-how-to-use-files/files-manage-1.png)
2. Klikněte na **Připojit** a zobrazí se příkazový řádek pro připojení sdílené složky z Windows.
   
    ![Snímek obrazovky, který ukazuje připojení sdílené složky.](./media/storage-dotnet-how-to-use-files/files-manage-2.png)
   
    ![Snímek obrazovky, který ukazuje připojení sdílené složky.](./media/storage-dotnet-how-to-use-files/files-manage-3.png)
   
   > [!TIP]
   > Pokud chcete najít přístupový klíč účtu úložiště pro připojení, klikněte na **Nastavení** účtu úložiště, a potom klikněte na **Přístupové klíče**.
   > 
   > 
   
    ![Snímek obrazovky, který ukazuje vyhledání přístupového klíče účtu úložiště.](./media/storage-dotnet-how-to-use-files/files-manage-4.png)
   
    ![Snímek obrazovky, který ukazuje vyhledání přístupového klíče účtu úložiště.](./media/storage-dotnet-how-to-use-files/files-manage-5.png)

## <a name="use-powershell-to-manage-a-file-share"></a>Správa sdílené složky pomocí PowerShellu
K vytvoření a správě sdílených složek také můžete použít Azure PowerShell.

### <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Instalace rutin PowerShellu pro Azure Storage
K používání PowerShellu budete potřebovat stáhnout a nainstalovat rutiny modulu Azure PowerShell. Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md).

> [!NOTE]
> Doporučuje se stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell.
> 
> 

Klikněte na **Start** a zadejte **Windows PowerShell**, tím otevřete okno Azure PowerShell. Okno PowerShell pro vás nahraje modul Azure PowerShell.

### <a name="create-a-context-for-your-storage-account-and-key"></a>Vytvoření kontextu pro účet úložiště a klíč
Teď vytvořte kontext účtu úložiště. Kontext obsahuje název účtu úložiště a klíč účtu. Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Místo `storage-account-name` a `storage-account-key` zadejte název svého účtu úložiště a klíč v následujícím příkladu.

```powershell
    # create a context for account and key
    $ctx=New-AzureStorageContext storage-account-name storage-account-key
```

### <a name="create-a-new-file-share"></a>Vytvoření nové sdílené složky
Dál vytvořte novou sdílenou složku s názvem `logs`.

```powershell
    # create a new share
    $s = New-AzureStorageShare logs -Context $ctx
```

Teď máte sdílenou složku v úložišti File. Dál přidáme nový adresář a soubor.

> [!IMPORTANT]
> Název vaší sdílené složky musí obsahovat jen malá písmena. Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

### <a name="create-a-directory-in-the-file-share"></a>Vytvoření adresáře ve sdílené složce
Dál ve sdílené složce vytvořte adresář. V následujícím příkladu má adresář název `CustomLogs`.

```powershell
    # create a directory in the share
    New-AzureStorageDirectory -Share $s -Path CustomLogs
```

### <a name="upload-a-local-file-to-the-directory"></a>Uložení místního souboru do adresáře
Teď do adresáře uložte místní soubor. V následujícím příkladu se uloží soubor z `C:\temp\Log1.txt`. Upravte cestu k souboru tak, aby odkazovala na platný soubor na vašem místním počítači.

```powershell
    # upload a local file to the new directory
    Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

### <a name="list-the-files-in-the-directory"></a>Zobrazení seznamu souborů v adresáři
Pokud chcete soubor zobrazit v adresáři, můžete vyvolat seznam všech souborů v adresáři. Tento příkaz vrátí soubory a podadresáře v adresáři CustomLogs (pokud tam nějaké jsou).

```powershell
    # list files in the new directory
    Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile vrátí seznam souborů a adresářů pro jakýkoli adresář, ve kterém se objekt předá. „Get-AzureStorageFile -Share $s“ vrátí seznam souborů a adresářů v kořenovém adresáři. Pokud chcete získat seznam souborů v podadresáři, musíte předat podadresář pro  Get-AzureStorageFile. Tohle to udělá – první část příkazu vrátí instanci adresáře podadresáře CustomLogs. To se potom předá do Get-AzureStorageFile, a to vrátí soubory a adresáře v CustomLogs.

### <a name="copy-files"></a>Kopírování souborů
Od Azure PowerShell verze 0.9.7 můžete kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru. Dole ukážeme, jak se operace kopírovaní dají provést pomocí rutin PowerShell.

```powershell
    # copy a file to the new directory
    Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

    # copy a blob to a file directory
    Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```

## <a name="mount-the-file-share"></a>Připojení sdílené složky
Díky podpoře SMB 3.0 teď úložiště File podporuje šifrování a trvalé popisovače od klientů SMB 3.0. Podpora šifrování znamená, že klienti SMB 3.0 můžou připojit sdílenou složku odkudkoli, jako třeba z:

* Virtuálního počítače Azure ve stejné oblasti (podporuje se už v SMB 2.1)
* Virtuálního počítače Azure v jiné oblasti (jen SMB 3.0)
* Aplikace lokálního klienta (jen SMB 3.0)

Když klient přistupuje k úložišti File, použitá verze SMB závisí na verzi SMB podporované operačním systémem. V tabulce dole je přehled podpory pro klienty Windows. Další informace o [verzích SMB](http://blogs.technet.com/b/josebda/archive/2013/10/02/windows-server-2012-r2-which-version-of-the-smb-protocol-smb-1-0-smb-2-0-smb-2-1-smb-3-0-or-smb-3-02-you-are-using.aspx) najdete v tomto blogu.

| Klient Windows | Podporovaná verze SMB |
|:--- |:--- |
| Windows 7 |SMB 2.1 |
| Windows Server 2008 R2 |SMB 2.1 |
| Windows 8 |SMB 3.0 |
| Windows Server 2012 |SMB 3.0 |
| Windows Server 2012 R2 |SMB 3.0 |
| Windows 10 |SMB 3.0 |

### <a name="mount-the-file-share-from-an-azure-virtual-machine-running-windows"></a>Připojení sdílené složky z virtuálního počítače Azure s Windows
Abychom ukázali, jak připojit sdílenou složku Azure, vytvoříme teď virtuální počítač Azure s Windows a vzdáleně se k němu přihlásíme, abychom mohli připojit sdílenou složku.

1. Nejdřív vytvořte nový virtuální počítač Azure podle pokynů v tématu [Vytvoření virtuálního počítače s Windows na webu Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Dál se do něj vzdáleně přihlaste podle pokynů v tématu [Přihlášení do virtuálního počítače s Windows přes web Azure Portal](../virtual-machines/virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
3. Ve virtuálním počítači otevřete okno PowerShell.

### <a name="persist-your-storage-account-credentials-for-the-virtual-machine"></a>Nastavení přihlašovacích údajů pro účet úložiště ve virtuálním počítači jako trvalé
Než připojíte sdílenou složku, nejdřív svoje přihlašovací údaje účtu úložiště nastavte ve virtuálním počítači jako trvalé. Tento krok systému Windows umožní vás po restartu virtuálního počítače automaticky znovu připojit ke sdílené složce. Pokud chcete svoje přihlašovací údaje nastavit jako trvalé, spusťte v okně PowerShell ve virtuálním počítači příkaz `cmdkey`. Místo `<storage-account-name>` zadejte název svého účtu úložiště a místo `<storage-account-key>` zadejte klíč účtu úložiště.

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>

Windows vás teď po restartu virtuálního počítače automaticky znovu připojí ke sdílené složce. Úspěšné automatické připojení můžete zkontrolovat spuštěním příkazu `net use` z okna PowerShell.

Poznámka: Přihlašovací údaje jsou nastavené jako trvalé jen v kontextu, ve kterém se spustí `cmdkey`. Pokud vyvíjíte aplikaci, která běží jako služba, budete muset svoje přihlašovací údaje nastavit jako trvalé i pro takový kontext.

### <a name="mount-the-file-share-using-the-persisted-credentials"></a>Připojení sdílené složky pomocí přihlašovacích údajů nastavených jako trvalé
Jakmile se vzdáleně přihlásíte do virtuálního počítače, můžete spustit příkaz `net use` a připojit tak sdílenou složku – použijte tuto syntaxi. Místo `<storage-account-name>` zadejte název svého účtu úložiště a místo `<share-name>` zadejte název sdílené složky File.

    net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>

    example :
    net use z: \\samples.file.core.windows.net\logs

Protože jsou vaše přihlašovací údaje z předchozího kroku nastavené jako trvalé, nemusíte je s příkazem `net use` zadávat. Pokud jste svoje přihlašovací údaje ještě nenastavili jako trvalé, zadejte je jako parametr předaný příkazu `net use`, jak je vidět na následujícím příkladu.

    net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /u:<storage-account-name> <storage-account-key>

    example :
    net use z: \\samples.file.core.windows.net\logs /u:samples <storage-account-key>

Teď můžete se sdílenou složkou pracovat z virtuálního počítače stejně jako s jakoukoli jinou jednotkou. Můžete zadávat standardní souborové příkazy v příkazovém řádku nebo připojenou složku a její soubory zobrazit v Průzkumníku souborů. Taky můžete spustit kód na virtuálním počítači, který přistupuje k souborové složce přes standardní API Windows pro vstup a výstup souborů, jako třeba API v [oborech názvů System.IO](http://msdn.microsoft.com/library/gg145019.aspx) v platformě .NET Framework.

Sdílenou složku taky můžete připojit z role, která běží v cloudové službě Azure, když se do ní vzdáleně přihlásíte.

### <a name="mount-the-file-share-from-an-on-premises-client-running-windows"></a>Připojení sdílené složky z lokálního klienta s Windows
Pokud se chcete připojit ke sdílené složce z lokálního klienta, musíte nejdřív udělat tři kroky:

* Nainstalovat verzi Windows, která podporuje SMB 3.0. Windows využije šifrování v SMB 3.0 pro bezpečný přenos dat mezi vaším lokálním klientem a sdílenou složkou Azure v cloudu.
* V místní síti otevřete přístup k internetu pro port 445 (odchozí TCP), protože to potřebuje protokol SMB.

> [!NOTE]
> Někteří poskytovatelé internetu můžou port 445 blokovat, v takovém případě se s nimi budete muset domluvit.
> 
> 

## <a name="develop-with-file-storage"></a>Vývoj s úložištěm File
Pokud chcete psát kód, který volá úložiště File, můžete použít knihovny klienta úložiště pro .NET a Javu nebo REST API pro Azure Storage. Příklad v této části ukazuje, jak pracovat se sdílenou složkou pomocí [Klientské knihovny pro úložiště Azure pro .NET](https://msdn.microsoft.com/library/mt347887.aspx) z jednoduché konzolové aplikace běžící v desktopovém prostředí.

### <a name="create-the-console-application-and-obtain-the-assembly"></a>Vytvoření konzolové aplikace a získání sestavení
Pokud chcete novou aplikaci vytvořit ve Visual Studiu a nainstalovat balíček NuGet s Klientskou knihovnou pro úložiště Azure pro .NET:

1. Ve Visual Studiu vyberte **Soubor > Nový projekt**, a pak v seznamu šablon pro Visual C# vyberte **Windows > Konzolová aplikace**.
2. Zadejte název konzolové aplikace a klikněte na **OK**.
3. Po vytvoření projektu klikněte v Průzkumníku řešení pravým tlačítkem na projekt a vyberte **Správa balíčků NuGet**. Vyhledejte online text „WindowsAzure.Storage“ a klikněte na **Instalovat**, tím se nainstaluje balíček Klientské knihovny pro úložiště Azure pro .NET a její závislosti.

Příklady kódu v tomto článku také používají [Knihovnu Microsoft Azure Configuration Manager](https://msdn.microsoft.com/library/azure/mt634646.aspx) k získání připojovacího řetězce úložiště ze souboru app.config v konzolové aplikaci. S nástrojem Azure Configuration Manager můžete získat připojovací řetězec na modulu runtime bez ohledu na to, jestli vaše aplikace běží v Microsoft Azure nebo je spuštěná z desktopové, mobilní nebo webové aplikace.

Pokud chcete nainstalovat balíček Azure Configuration Manager, klikněte pravým tlačítkem na projekt a vyberte **Správa balíčků NuGet**. Vyhledejte online text „ConfigurationManager“ a klikněte na **Instalovat**, tím balíček nainstalujete.

Použití nástroje Azure Configuration Manager není povinné. Taky můžete použít API jako třeba [třídu ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) platformy .NET Framework.

### <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Uložení přihlašovacích údajů pro účet úložiště do souboru app.config
Dál uložte svoje přihlašovací údaje do souboru app.config vašeho projektu. Upravte soubor app.config tak, aby vypadal podobně jako v následujícím příkladu, `myaccount` nahraďte názvem svého účtu úložiště a `mykey` nahraďte svým klíčem účtu úložiště.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
        </startup>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
        </appSettings>
    </configuration>


> [!NOTE]
> Nejnovější verze emulátoru úložiště Azure nepodporuje úložiště File. Aby váš připojovací řetězec mohl pracovat s úložištěm File, musí jako cíl mít účet úložiště Azure v cloudu.
> 
> 

### <a name="add-namespace-declarations"></a>Přidání deklarací oboru názvů
V Průzkumníku řešení otevřete soubor `program.cs` a na začátek souboru přidejte následující deklarace oborů názvů.

```csharp
    using Microsoft.Azure; // Namespace for Azure Configuration Manager
    using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage
    using Microsoft.WindowsAzure.Storage.File; // Namespace for File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="access-the-file-share-programmatically"></a>Programový přístup ke sdílené složce
Dál přidejte následující kód k metodě `Main()` (po výše uvedeném kódu) pro získání připojovacího řetězce. Tento kód získá odkaz na soubor, který jsme vytvořili předtím, a vypíše obsah do okna konzoly.

```csharp
    // Create a CloudFileClient object for credentialed access to File storage.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Get a reference to the file share we created previously.
    CloudFileShare share = fileClient.GetShareReference("logs");

    // Ensure that the share exists.
    if (share.Exists())
    {
        // Get a reference to the root directory for the share.
        CloudFileDirectory rootDir = share.GetRootDirectoryReference();

        // Get a reference to the directory we created previously.
        CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

        // Ensure that the directory exists.
        if (sampleDir.Exists())
        {
            // Get a reference to the file we created previously.
            CloudFile file = sampleDir.GetFileReference("Log1.txt");

            // Ensure that the file exists.
            if (file.Exists())
            {
                // Write the contents of the file to the console window.
                Console.WriteLine(file.DownloadTextAsync().Result);
            }
        }
    }
```

Výstup zobrazíte spuštěním konzolové aplikace.

### <a name="set-the-maximum-size-for-a-file-share"></a>Nastavení maximální velikosti sdílené složky
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje nastavit kvótu (maximální velikost) sdílené složky v gigabajtech. Můžete se taky podívat, kolik data je aktuálně uloženo ve sdílené složce.

Pokud nastavíte kvótu sdílené složky, můžete omezit celkovou velikost souborů uložených ve sdílené složce. Pokud celková velikost souborů ve sdílené složce překročí kvótu nastavenou pro sdílenou složku, klienti nebudou moct zvyšovat velikost existujících souborů, s výjimkou situace, když je velikost souborů nulová.

Dole uvedený příklad ukazuje, jak můžete zkontrolovat aktuální využití sdílené složky a jak nastavit kvótu pro sdílenou složku.

```csharp
    // Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create a CloudFileClient object for credentialed access to File storage.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Get a reference to the file share we created previously.
    CloudFileShare share = fileClient.GetShareReference("logs");

    // Ensure that the share exists.
    if (share.Exists())
    {
        // Check current usage stats for the share.
        // Note that the ShareStats object is part of the protocol layer for the File service.
        Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
        Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

        // Specify the maximum size of the share, in GB.
        // This line sets the quota to be 10 GB greater than the current usage of the share.
        share.Properties.Quota = 10 + stats.Usage;
        share.SetProperties();

        // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
        share.FetchAttributes();
        Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
    }
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo konkrétní soubor. Můžete taky vytvořit sdílené zásady přístupu pro sdílenou složku ke správě sdílených přístupových podpisů. Vytvoření sdílených zásad přístupu se doporučuje, protože se tím nabízí způsob odvolání SAS v případě narušení jeho integrity nebo důvěryhodnosti.

V následujícím příkladu se vytvoří sdílená zásada přístupu pro sdílenou složku a ta se pak použije k omezení pro SAS na souboru ve sdílené složce.

```csharp
    // Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create a CloudFileClient object for credentialed access to File storage.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Get a reference to the file share we created previously.
    CloudFileShare share = fileClient.GetShareReference("logs");

    // Ensure that the share exists.
    if (share.Exists())
    {
        string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

        // Create a new shared access policy and define its constraints.
        SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
                Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
            };

        // Get existing permissions for the share.
        FileSharePermissions permissions = share.GetPermissions();

        // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        share.SetPermissions(permissions);

        // Generate a SAS for a file in the share and associate this access policy with it.
        CloudFileDirectory rootDir = share.GetRootDirectoryReference();
        CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
        CloudFile file = sampleDir.GetFileReference("Log1.txt");
        string sasToken = file.GetSharedAccessSignature(null, policyName);
        Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

        // Create a new CloudFile object from the SAS, and write some text to the file.
        CloudFile fileSas = new CloudFile(fileSasUri);
        fileSas.UploadText("This write operation is authenticated via SAS.");
        Console.WriteLine(fileSas.DownloadText());
    }
```

Další informace o vytváření a používání sdílených přístupových podpisů najdete v tématech [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md) a [Vytvoření a použití SAS se službou Blob Storage](storage-dotnet-shared-access-signature-part-2.md).

### <a name="copy-files"></a>Kopírování souborů
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru. V dalších kapitolách ukážeme, jak se operace kopírovaní dají provést programově.

Pomocí nástroje AzCopy taky můžete zkopírovat jeden soubor do jiného nebo zkopírovat objekt blob do souboru a obráceně. Viz téma [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

> [!NOTE]
> Pokud kopírujete objekt blob do souboru nebo soubor do objektu blob, musíte použít sdílený přístupový klíč (SAS) k ověření zdroje objektu, a to i když kopírujete v rámci jednoho účtu úložiště.
> 
> 

**Kopírování souboru do jiného souboru**

V následujícím příkladu se zkopíruje soubor do jiného souboru ve stejné sdílené složce. Protože tato operace kopírování kopíruje soubory ve stejném účtu úložiště, můžete pro kopírování použít ověření Sdíleným klíčem.

```csharp
    // Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create a CloudFileClient object for credentialed access to File storage.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Get a reference to the file share we created previously.
    CloudFileShare share = fileClient.GetShareReference("logs");

    // Ensure that the share exists.
    if (share.Exists())
    {
        // Get a reference to the root directory for the share.
        CloudFileDirectory rootDir = share.GetRootDirectoryReference();

        // Get a reference to the directory we created previously.
        CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

        // Ensure that the directory exists.
        if (sampleDir.Exists())
        {
            // Get a reference to the file we created previously.
            CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

            // Ensure that the source file exists.
            if (sourceFile.Exists())
            {
                // Get a reference to the destination file.
                CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

                // Start the copy operation.
                destFile.StartCopy(sourceFile);

                // Write the contents of the destination file to the console window.
                Console.WriteLine(destFile.DownloadText());
            }
        }
    }
```

**Kopírování souboru do objektu blob**

V následujícím příkladu se vytvoří soubor a zkopíruje se do objektu blob ve stejném účtu úložiště. V příkladu se vytvoří SAS pro zdrojový soubor a služba ho při kopírování použije k ověření přístupu ke zdrojovému souboru.

```csharp
    // Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create a CloudFileClient object for credentialed access to File storage.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Create a new file share, if it does not already exist.
    CloudFileShare share = fileClient.GetShareReference("sample-share");
    share.CreateIfNotExists();

    // Create a new file in the root directory.
    CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
    sourceFile.UploadText("A sample file in the root directory.");

    // Get a reference to the blob to which the file will be copied.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();
    CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

    // Create a SAS for the file that's valid for 24 hours.
    // Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
    // to authenticate access to the source object, even if you are copying within the same
    // storage account.
    string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
    {
        // Only read permissions are required for the source file.
        Permissions = SharedAccessFilePermissions.Read,
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
    });

    // Construct the URI to the source file, including the SAS token.
    Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

    // Copy the file to the blob.
    destBlob.StartCopy(fileSasUri);

    // Write the contents of the file to the console window.
    Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
    Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Stejným způsobem můžete kopírovat objekt blob do souboru. Pokud je zdrojovým objektem objekt blob, vytvořte SAS k ověření přístupu k tomuto objektu blob při kopírování.

## <a name="troubleshooting-file-storage-using-metrics"></a>Řešení problémů s úložištěm File pomocí metrik
Azure Storage Analytics teď podporuje metriky pro úložiště File. S údaji z metriky můžete sledovat žádosti a diagnostikovat potíže.

Metriky pro úložiště File můžete povolit v [webu Azure Portal](https://portal.azure.com). Metriky taky můžete zapnout programově zavoláním operace Set File Service Properties přes REST API nebo některou z podobných operací v Klientské knihovně pro úložiště.

Následující ukázka kódu ukazuje, jak můžete použít Klientskou knihovnu pro úložiště pro .NET k zapnutí metrik pro úložiště File.

Nejdříve do souboru program.cs vedle příkazů, které jste přidali výše, přidejte následující `using` příkazy:

```csharp
    using Microsoft.WindowsAzure.Storage.File.Protocol;
    using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Pamatujte, že zatímco úložiště Blob, Table a Queue používají typ sdílený `ServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.Shared.Protocol`, úložiště File používá vlastní typ `FileServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.File.Protocol`. Aby se ale následující kód mohl zkompilovat, musí se z vašeho kódu odkazovat oba obory názvů.

```csharp
    // Parse your storage connection string from your application's configuration file.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
    // Create the File service client.
    CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

    // Set metrics properties for File service.
    // Note that the File service currently uses its own service properties type,
    // available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
    fileClient.SetServiceProperties(new FileServiceProperties()
    {
        // Set hour metrics
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 14,
            Version = "1.0"
        },
        // Set minute metrics
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        }
    });

    // Read the metrics properties we just set.
    FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
    Console.WriteLine("Hour metrics:");
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
    Console.WriteLine();
    Console.WriteLine("Minute metrics:");
    Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Podrobné pokyny, jak postupovat při řešení problémů, najdete v článku [Azure Files Troubleshooting Article](storage-troubleshoot-file-connection-problems.md) (Soubory Azure – řešení problémů). 

## <a name="file-storage-faq"></a>Nejčastější dotazy týkající se úložiště File
1. **Podporuje úložiště File ověření založené na Active Directory?**
   
    Aktuálně nepodporujeme ověření založené na AD nebo ACL, je to však na našem seznamu požadovaných funkcí. Prozatím se k ověření ke sdílené složce používají klíče účtů Azure Storage. Nabízíme alternativní řešení, které využívá sdílené přístupové podpisy (SAS) přes REST API nebo knihovny klienta. Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné po nastavenou dobu. Například můžete pro daný soubor vygenerovat token s přístupem jen pro čtení. Každý, kdo takový token má po dobu jeho platnosti, má k souboru přístup jen pro čtení.
   
    SAS se podporuje jen přes REST API nebo knihovny klienta. Když připojíte sdílenou složku přes protokol SMB, nemůžete použít SAS pro delegování přístupu k jejímu obsahu.
2. **Jsou sdílené složky Azure File viditelné veřejně přes internet nebo jsou dostupné jen přes Azure?**
   
    Když je otevřený port 445 (odchozí TCP) a váš klient podporuje protokol SMB 3.0 (*např.*, Windows 8 nebo Windows Server 2012), je vaše sdílená složka dostupná přes internet.  
3. **Počítá se síťový provoz mezi virtuálním počítačem Azure a sdílenou složkou jako externí datové přenosy, které jsou zpoplatněné nad rámec předplatného?**
   
    Pokud jsou sdílená složka a virtuální počítač v různých oblastech, provoz mezi nimi bude účtován jako externí datové přenosy.
4. **Pokud síťový provoz probíhá mezi virtuálním počítačem a sdílenou složkou ve stejné oblasti, je bezplatný?**
   
    Ano. Je bezplatný, pokud probíhá v jedné a té samé oblasti.
5. **Závisí připojení z lokálních virtuálních počítačů k Azure File Storage na Azure ExpressRoute?**
   
    Ne. Pokud nemáte ExpressRoute, můžete ke sdílené složce přistupovat z lokálního prostředí za předpokladu, že máte port 445 (odchozí TCP) otevřený pro přístup k internetu. Pokud ale chcete, ExpressRoute s úložištěm File použít můžete.
6. **Je „určující sdílená složka“ pro cluster s podporou převzetí služeb při selhání jedním z případů využití Azure File Storage?**
   
    To se aktuálně nepodporuje.
7. **Úložiště File se teď replikuje jen přes LRS nebo GRS, je to tak?**  
   
    Plánujeme podporu pro RA-GRS, ale nemáme pro to teď žádný časový horizont.
8. **Kdy můžu začít používat existující účty úložiště pro Azure File Storage?**
   
    Služba Azure File Storage je teď zapnutá pro všechny účty úložiště.
9. **Přidáte do REST API taky operaci Přejmenovat?**
   
    Operace Přejmenovat se zatím v našem REST API nepodporuje.
10. **Může mít člověk vnořené sdílené složky, jinými slovy sdílenou složku ve sdílené složce?**
    
    Ne. Sdílená složka je virtuální jednotka, kterou můžete připojit, takže vnořené sdílené složky se nepodporují.
11. **Dá se pro složky ve sdílené složce specifikovat oprávnění jen pro čtení nebo jen pro zápis?**
    
    Takovou míru kontroly nad oprávněními nemáte, pokud sdílenou složku připojíte přes SMB. Můžete toho však dosáhnout vytvořením sdíleného přístupového podpisu (SAS) přes REST API nebo knihovny klienta.  
12. **Když jsem se pokusil/a rozbalit soubory do úložiště File, šlo to velmi pomalu. Co bych měl/a dělat?**
    
    Pokud chcete do úložiště File přenášet větší počet souborů, doporučujeme použít AzCopy, Azure PowerShell (Windows) nebo Azure CLI (Linux/Unix), protože tyto nástroje jsou optimalizované pro síťové přenosy.
13. **Vydali jsme opravu problému s nízkou rychlostí Azure Files.**
    
    Tým Windows nedávno vydal opravu problému s nízkou rychlostí, když zákazník přistupuje k Azure Files Storage z Windows 8.1 nebo Windows Serveru 2012 R2. Další informace najdete v článku znalostní báze [Pomalý výkon při přístupu k Azure Files Storage z Windows 8.1 nebo Windows Serveru 2012 R2](https://support.microsoft.com/en-us/kb/3114025).
14. **Používání Azure File Storage s IBM MQ**
    
    Společnost IBM vydala dokument, který zákazníky IBM MQ provede konfigurací Azure File Storage pro jejich službu. Další informace najdete v tématu [Nastavení Manažera fronty více instancí IBM MQ se službou Microsoft Azure File](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).
15. **Jak mám řešit problémy s Azure File Storage?**
    
    Podrobné pokyny, jak postupovat při řešení problémů, najdete v článku [Azure Files Troubleshooting Article](storage-troubleshoot-file-connection-problems.md) (Soubory Azure – řešení problémů).               

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure Files Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Používání Azure File Storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Podpora nástrojů pro úložiště File
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Nastavení trvalých připojení k Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)




<!--HONumber=Nov16_HO2-->


