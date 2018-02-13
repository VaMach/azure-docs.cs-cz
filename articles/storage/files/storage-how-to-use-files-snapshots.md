---
title: "Práce s sdílené složce snímků (preview) | Microsoft Docs"
description: "Sdílené složky snímků je jen pro čtení verze Azure Files sdílené složky, která je provedená v bodě v čase, jako způsob, jak zálohovat sdílenou složku."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: c4a5f7d28601867c383b8b348568e4bb580a81eb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-share-snapshots-preview"></a>Práce s sdílené složce snímků (preview)
Sdílenou složku snímku (preview) je jen pro čtení verze Azure Files sdílené složky, která je provedená v bodě v čase. Po vytvoření snímku sdílené složky ji můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Sdílenou složku snímku poskytuje způsob, jak zálohovat sdílenou složku, jak se objevuje v časovém okamžiku. 

V tomto článku budete zjistěte, jak vytvářet, spravovat a odstraňovat snímky sdílené složky. Další informace najdete v tématu [sdílet snímku přehled](storage-snapshots-files.md) nebo [snímku – nejčastější dotazy](storage-files-faq.md).

## <a name="create-a-share-snapshot"></a>Vytvoření snímku sdílené složky

Snímek sdílené složky můžete vytvořit pomocí portálu Azure, PowerShell, rozhraní příkazového řádku Azure, rozhraní API REST nebo všechny sady SDK úložiště Azure. Následující části popisují, jak vytvořit sdílenou složku snímků pomocí portálu, rozhraní příkazového řádku a prostředí PowerShell. 

Sdílenou složku snímku sdílené složky můžete využít i když je používán. Sdílené složky snímků však zachytit pouze data, která již byla zapsána do sdílené složky v době, vydání příkazu ke sdílené složce snímku. To může vyloučit všechna data, která se uloží do mezipaměti všechny aplikace nebo operační systém.

### <a name="create-a-share-snapshot-by-using-the-portal"></a>Vytvoření snímku sdílenou složku pomocí portálu  
Pokud chcete vytvořit sdílenou složku v daném okamžiku snímek, přejděte k souboru sdílené složky v portálu a vyberte **vytvoření snímku**.

>   ![Nabídky snímku na portálu](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-a-share-snapshot-by-using-azure-cli-20"></a>Vytvoření snímku sdílenou složku pomocí Azure CLI 2.0
Můžete vytvořit sdílenou složku snímku pomocí `az storage share snapshot` příkaz:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Příklad výstupu:
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="create-a-share-snapshot-by-using-powershell"></a>Vytvoření snímku sdílenou složku pomocí prostředí PowerShell
Můžete vytvořit sdílenou složku snímku pomocí `$share.Snapshot()` příkaz:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

```

## <a name="perform-common-share-snapshot-operations"></a>Provedení běžných operací snímku sdílené složky

Můžete vytvořit výčet sdílené složky snímků přidružených sdílené složky pomocí **předchozí verze** ve Windows prostřednictvím REST, klientské knihovny, prostředí PowerShell a portálu. Po připojena sdílené složky, můžete zobrazit všechny předchozí verze souboru s použitím **předchozí verze** ve Windows. 

Následující části popisují, jak pomocí portálu Azure, Windows a Azure CLI 2.0 přejděte na a obnovte sdílené složky snímků.

### <a name="share-snapshot-operations-in-the-portal"></a>Operace snímku sdílené složky v portálu

Můžete si prohlédnout všechny vaše snímky sdílené složky pro soubor sdílet v portálu a přejděte do sdílené složky snímek, který chcete zobrazit jeho obsah.

#### <a name="view-a-share-snapshot"></a>Zobrazit sdílenou složku snímku
Ve sdílené složce v části **snímku**, vyberte **zobrazit snímky**.

![Příkaz "Zobrazit snímky" na portálu](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-to-share-snapshot-content"></a>Seznam a procházet a sdílet obsah snímku
Zobrazení seznamu snímky sdílenou složku a potom vyhledejte obsah jeden snímek přímo výběrem požadované časové razítko.

![Vybraný snímek v seznamu časová razítka](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

Můžete také vybrat **připojit** tlačítko ve vašem seznamu snímku zobrazení získat `net use` příkaz a cesta k adresáři na konkrétní sdílenou složku snímek. Můžete pak přímo vyhledat tohoto snímku.


![Připojit podokně se seznamem příkaz](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-a-share-snapshot"></a>Stažení nebo obnovení ze snímku sdílené složky
Z portálu, stažení nebo obnovit soubor ze snímku s použitím **Stáhnout** nebo **obnovení** tlačítko v uvedeném pořadí.

![Stahování a obnovení tlačítka](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="share-snapshot-operations-in-windows"></a>Operace snímku sdílenou složku v systému Windows
Pokud jste už udělali snímky sdílenou složku Sdílené složky, můžete zobrazit předchozích verzích sdílené složky, adresáře nebo určitého souboru ze připojené sdílené složky v systému Windows. Jako příklad zde je, jak můžete použít funkci předchozí verze k zobrazení a obnovit předchozí verze adresáře v systému Windows.

> [!Note]  
> Můžete provádět stejné operace na úrovni sdílené složky a úrovni souborů. V seznamu se zobrazí pouze verze, která obsahuje změny, které pro tento adresář nebo soubor. Pokud konkrétní adresář nebo soubor nezměnila mezi dvěma snímky sdílené složky, se zobrazí v seznamu úrovni sdílené složky pro předchozí verze, ale není v adresáři nebo souboru předchozí verze seznamu sdílenou složku snímku.

#### <a name="mount-a-file-share"></a>Připojit sdílenou složku
Nejdřív připojit sdílenou složku pomocí `net use` příkaz.

#### <a name="open-a-mounted-share-in-file-explorer"></a>Otevřete připojenou složku v Průzkumníku souborů
Přejděte do Průzkumníka souborů a najít připojenou složku.

![Připojenou složku v Průzkumníku souborů](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Seznam předchozí verze
Přejděte na položku nebo nadřazenou položku, která je nutné obnovit. Poklikejte na Přejít do požadovaného adresáře. Klikněte pravým tlačítkem a vyberte **vlastnosti** z nabídky.

![Klikněte pravým tlačítkem na nabídku pro vybraný adresář](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Vyberte **předchozí verze** zobrazíte seznam snímky sdílené složky pro tento adresář. V seznamu může trvat několik sekund, načíst, v závislosti na rychlosti sítě a počet snímků, sdílené složky v adresáři.

![Karta předchozí verze](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

Můžete vybrat **otevřete** otevřete konkrétní snímek. 

![Otevřít snímku](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Obnovení z předchozí verze
Vyberte **obnovení** zkopírovat obsah rekurzivně celý adresář v okamžiku vytvoření snímku sdílenou složku na původní umístění.
 ![Obnovení tlačítka na zprávu upozornění](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="share-snapshot-operations-in-azure-cli-20"></a>Sdílenou složku snímku operace v Azure CLI 2.0
Azure CLI 2.0 můžete použít k provedení operací, jako je výpis snímky sdílenou složku, procházení sdílet obsah snímku, obnovení nebo stahování souborů ze sdílené složky snímků nebo odstranění sdílet snímky.

#### <a name="list-share-snapshots"></a>Snímky sdílené položky seznamu

Můžete vytvořit seznam snímky konkrétní sdílenou složku pomocí [ `az storage share list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) s `--include-snapshots`:

```azurecli-interactive 
az storage share list --include-snapshots
```

Příkaz vám poskytne seznam sdílené složky snímků, společně s jeho přidružené vlastnosti. Tento výstup je příklad:

```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

#### <a name="browse-to-a-share-snapshot"></a>Přejděte do složky snímku
Můžete vyhledat konkrétní sdílenou složku snímku a zobrazit jeho obsah pomocí [ `az storage file list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Zadejte název sdílené složky a časové razítko, které chcete procházet, jak je znázorněno v následujícím příkladu:

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

Ve výstupu zobrazí, obsah sdílené složky snímků je stejný jako obsah sdílené složky v bodě v čase byl vytvořen tento snímek sdílené složky:

```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
#### <a name="restore-from-a-share-snapshot"></a>Obnovení ze snímku sdílené složky

Soubor můžete obnovit pomocí kopírování nebo stahování ze snímku sdílené složky. Použití `az storage file download` příkaz, jak je znázorněno v následujícím příkladu:

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

Ve výstupu uvidíte, že obsah stažený soubor a jeho vlastnosti jsou stejné jako obsah a vlastnosti v bodě v čase, které sdílejí snímku byla vytvořena:

```json
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```

<<<<<<< HEAD
### <a name="file-share-snapshot-operations-in-azure-powershell"></a>Sdílenou složku snímku operací v prostředí Azure PowerShell
Můžete použít Azure Powershell provádět stejné operace jako je například seznam sdílené složky snímků, procházení obsahu snímku sdílené složky, obnovení nebo stahování souborů ze sdílené složky snímku nebo odstranění sdílené složky snímků.

#### <a name="list-share-snapshots"></a>Snímky sdílené položky seznamu

Lze zadat seznam konkrétní sdílenou složku pomocí snímků sdílené složky`Get-AzureStorageShare`

```powershell
Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
```

#### <a name="browse-share-snapshots"></a>Procházet snímky sdílené složky
Může také procházet do konkrétní sdílenou složku snímku na serveru obsahu pomocí `Get-AzureStorageFile` s hodnotou `-Share` odkazující na konkrétní snímku

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
Get-AzureStorageFile -Share $snapshot
```

#### <a name="restore-from-share-snapshots"></a>Obnovení ze sdílené složky snímků

Soubor můžete obnovit pomocí kopírování nebo stažení souboru ze sdílené složky snímek pomocí `Get-AzureStorageFileContent` příkaz

```powershell
$download='C:\Temp\Download'
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $download
```

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
$directory = Get-AzureStorageFile -ShareName "ContosoShare06" -Path "ContosoWorkingFolder" | Get-AzureStorageFile
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $directory
```


## <a name="delete-azure-files-share-snapshot"></a>Odstranit snímek sdílené složky Azure Files
=======
## <a name="delete-a-share-snapshot"></a>Odstranit snímek sdílené složky
>>>>>>> 6a1833e10031fbf1ab204bb1f30cb54cf5fbcada

Snímky sdílené složky můžete odstranit pomocí portálu Azure, PowerShell, rozhraní příkazového řádku, REST API nebo všechny sady SDK úložiště. Následující části popisují postup odstranění sdílené složky snímků pomocí portálu Azure, rozhraní příkazového řádku a prostředí PowerShell.

Můžete vyhledat a sdílet snímky zobrazení rozdílů mezi dvěma snímky pomocí libovolného nástroje porovnání. Potom můžete určit, které sdílenou složku snímku chcete odstranit. 

Nelze odstranit sdílenou složku, která má sdílenou složku snímku. Abyste mohli odstranění sdílené složky, musíte nejprve odstranit všechny snímky jeho sdílení.

### <a name="delete-a-share-snapshot-by-using-the-portal"></a>Odstranění snímku sdílenou složku pomocí portálu  
Na portálu, přejděte do okna vaší soubor sdílené složky a použít **odstranit** tlačítko odstranit jednu nebo více snímků sdílené složky.

>   ![Odstranění tlačítka](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-by-using-azure-cli-20"></a>Odstranění snímku sdílenou složku pomocí Azure CLI 2.0
Odstraněním snímku sdílenou složku pomocí `[az storage share delete]` příkaz. Používat vaše časové razítko pro sdílenou složku snímku `--snapshot '2017-10-04T23:28:35.0000000Z' ` parametr v následujícím příkladu:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Příklad výstupu:
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-by-using-powershell"></a>Odstranění snímku sdílenou složku pomocí prostředí PowerShell
Odstraněním snímku sdílenou složku pomocí `Remove-AzureStorageShare -Share` příkaz:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##Create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

##Delete snapshot
Remove-AzureStorageShare -Share $snapshot

```

## <a name="next-steps"></a>Další postup
* [Přehled snímku](storage-snapshots-files.md)
* [Snímek – nejčastější dotazy](storage-files-faq.md)
