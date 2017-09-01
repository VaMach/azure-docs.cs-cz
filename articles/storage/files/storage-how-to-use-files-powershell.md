---
title: "Použití PowerShellu ke správě služby Azure File Storage | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí PowerShellu spravovat službu Azure File Storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: ce62d4423ce711a6902aed7b8174ff4e827f6083
ms.contentlocale: cs-cz
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a>Použití PowerShellu ke správě služby Azure File Storage
Azure PowerShell můžete použít k vytváření a správě sdílených složek.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Instalace rutin PowerShellu pro Azure Storage
K používání PowerShellu budete potřebovat stáhnout a nainstalovat rutiny modulu Azure PowerShell. Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Doporučuje se stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure PowerShell.
> 
> 

Klikněte na **Start** a zadejte **Windows PowerShell**, tím otevřete okno Azure PowerShell. Okno PowerShell pro vás nahraje modul Azure PowerShell.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Vytvoření kontextu pro účet úložiště a klíč
Vytvořte kontext účtu úložiště. Kontext obsahuje název účtu úložiště a klíč účtu. Pokyny pro zkopírování klíče účtu z [webu Azure Portal](https://portal.azure.com) najdete v tématu [Zobrazení a zkopírování přístupových klíčů k úložišti](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

Místo `storage-account-name` a `storage-account-key` zadejte název svého účtu úložiště a klíč v následujícím příkladu.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Vytvoření nové sdílené složky
Vytvořte novou sdílenou složku s názvem `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Teď máte sdílenou složku v úložišti File. Dál přidáme nový adresář a soubor.

> [!IMPORTANT]
> Název vaší sdílené složky musí obsahovat jen malá písmena. Kompletní informace o zadávání názvů sdílených složek a souborů najdete v tématu [Pojmenování a odkazování na sdílené složky, soubory a metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Vytvoření adresáře ve sdílené složce
Ve sdílené složce vytvořte adresář. V následujícím příkladu má adresář název `CustomLogs`.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Uložení místního souboru do adresáře
Teď do adresáře uložte místní soubor. V následujícím příkladu se uloží soubor z `C:\temp\Log1.txt`. Upravte cestu k souboru tak, aby odkazovala na platný soubor na vašem místním počítači.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>Zobrazení seznamu souborů v adresáři
Pokud chcete soubor zobrazit v adresáři, můžete vyvolat seznam všech souborů v adresáři. Tento příkaz vrátí soubory a podadresáře v adresáři CustomLogs (pokud tam nějaké jsou).

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile vrátí seznam souborů a adresářů pro jakýkoli adresář, ve kterém se objekt předá. „Get-AzureStorageFile -Share $s“ vrátí seznam souborů a adresářů v kořenovém adresáři. Pokud chcete získat seznam souborů v podadresáři, musíte předat podadresář pro  Get-AzureStorageFile. Tohle to udělá – první část příkazu vrátí instanci adresáře podadresáře CustomLogs. To se potom předá do Get-AzureStorageFile, a to vrátí soubory a adresáře v CustomLogs.

## <a name="copy-files"></a>Kopírování souborů
Od Azure PowerShell verze 0.9.7 můžete kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru. Dole ukážeme, jak se operace kopírovaní dají provést pomocí rutin PowerShell.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](../storage-files-faq.md)
* [Řešení potíží ve Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Řešení potíží v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)    
