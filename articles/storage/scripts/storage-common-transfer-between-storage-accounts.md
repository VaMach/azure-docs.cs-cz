---
title: "Azure skript prostředí PowerShell ukázkový – migrace objektů BLOB mezi různými účty úložiště v systému Windows pomocí nástroje AzCopy | Microsoft Docs"
description: "Pomocí AzCopy, zkopíruje obsah objektu Blob jeden účet úložiště Azure do jiné."
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 1/3/2018
ms.author: v-rogara
ms.openlocfilehash: 5fb3771b591e92b1c55f4f9748a2e16b46ccdae2
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Migrace mezi různými účty úložiště v systému Windows pomocí nástroje AzCopy objektů BLOB

Tato ukázka zkopíruje všechny objekty blob z účtu úložiště zadaný uživatelem zdroje na zadaný uživatelem cílového účtu úložiště. 

Toho dosahuje tím, že použití `Get-AzureStorageContainer` příkaz, který vypíše všechny kontejnery v účtu úložiště. Ukázka pak vysílá příkazy AzCopy, kopírování každý kontejner z účtu úložiště zdrojového na cílový účet úložiště. Pokud dojde k jeho selhání, ukázky opakuje $retryTimes (výchozí je 3 a můžete změnit `-RetryTimes` parametr). Pokud je selhání došlo při každém opakování, uživatel může znovu skript tím, že poskytuje vzorku s poslední úspěšně zkopírovaný kontejneru pomocí `-LastSuccessContainerName` parametr. Ukázka potom pokračuje v kopírování kontejnery od tohoto okamžiku.

Tato ukázka vyžaduje verzi modulu Azure PowerShell úložiště **4.0.2** nebo novější. Můžete zkontrolovat vaše nainstalovaná verze pomocí `Get-Module -ListAvailable Azure.storage`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Tato ukázka taky vyžaduje nejnovější verzi [AzCopy v systému Windows](http://aka.ms/downloadazcopy). Je výchozí instalační adresář`C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`

Tato ukázka přebírá zdroj název účtu úložiště a klíč, název cílového účtu úložiště a klíč a úplnou cestu k souboru AzCopy.exe (pokud nejsou nainstalovány v výchozí adresář).

Následují příklady vstupu pro tuto ukázku:

Pokud AzCopy je nainstalovaná na výchozí adresář:
```PowerShell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
```

Pokud AzCopy není nainstalovaná na výchozí adresář:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```

Pokud je došlo selhání a ukázka musí znovu spustit z konkrétní kontejneru: 

`.\copyScript.ps1 -LastSuccessContainerName myContainerName`

## <a name="sample-script"></a>Ukázkový skript

```Powershell
# Run the script in a new open Powershell window, which has not run other cmdlets, or AzCopy performance could suffer .
# Install Azure PowerShell before runing the script: https://github.com/Azure/azure-powershell/releases
# Install AzCopy before runing the script: https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy
# Do not modify the Source or Destination accounts while the script is running

 param (
    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$srcStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account key.")]
    [String]$srcStorageAccountKey,
    
    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$DestStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account key.")]
    [String]$DestStorageAccountKey,

    [Parameter(Mandatory = $false, 
    HelpMessage= "Input the full filePath of the AzCopy.exe, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe")]
    [String]$AzCopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe",

    [Parameter(Mandatory = $false, 
    HelpMessage='Sets the number of retries in an event of failure. Set to 0 for no retry, set -1 for infinite retry.')]
    [Int32]$RetryTimes = 3,

    [Parameter(Mandatory = $false, 
    ValueFromPipeline = $true, 
    HelpMessage='Used for resume operation. When provided, the script will copy containers that are alphabetically after $LastSuccessContainerName')]
    [String]$LastSuccessContainerName = $nullz
 )

 if( (Get-Item $AzCopyPath).Exists)
 {

    $FileItemVersion = (Get-Item $AzCopyPath).VersionInfo
    $FilePath = ("{0}.{1}.{2}.{3}" -f  $FileItemVersion.FileMajorPart,  $FileItemVersion.FileMinorPart,  $FileItemVersion.FileBuildPart,  $FileItemVersion.FilePrivatePart)

    if([version] $FilePath -lt "7.0.0.2")
    {
        $AzCopyPath = Read-Host "Version of AzCopy found at default install directory is of a lower, unsupported version. Please input the full filePath of the AzCopy.exe that is version 7.0.0.2 or higher, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
    }
 }
 elseIf( (Get-Item $AzCopyPath).Exists -eq $false)
 {
    $AzCopyPath = Read-Host "Input the full filePath of the AzCopy.exe, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
 }

# Create and check Storage context
$Error.Clear()
$srcCtx = New-AzureStorageContext -StorageAccountName $srcStorageAccountName -StorageAccountKey $srcStorageAccountKey
if ($srcCtx -eq $null)
{
    Write-Error "Script could not create source Storage Context, possibly due to invalid StorageAccountName or StorageAccount Key terminating: $Error[0]";
    return;
}
$destCtx = New-AzureStorageContext -StorageAccountName $destStorageAccountName -StorageAccountKey $destStorageAccountKey
if ($destCtx -eq $null)
{
    Write-Error "Script could not create destination storage context, possibly due to invalid StorageAccountName or StorageAccount Key terminating: $Error[0]";
    return;
}

#Check Source and Destination Storage account Connection
$Error.Clear()
$Containers = Get-AzureStorageContainer -MaxCount 1 -Context $srcCtx
if ($Error.Count -gt 0)
{
    Write-Error "Script failed to connect to source Storage account, terminating: $Error[0]";
    return;
}
$Containers = Get-AzureStorageContainer -MaxCount 1 -Context $destCtx
if ($Error.Count -gt 0)
{
    Write-Error "Script failed to connect to destination Storage account, terminating: $Error[0]";
    return;
}

#Check AzCopy Path
if((Test-Path $AzCopyPath) -eq $false)
{
    Write-Error "Script is terminating since the provided AzCopyPath does not exist: $AzCopyPath ";
    return;
}
elseif((Get-Item $AzCopyPath).BaseName -ne "AzCopy" )
{
    Write-Error "Script is terminating since the provided AzCopyPath does not refer to the AzCopy exe: $AzCopyPath ";
    return;
}
elseif((Get-Item $AzCopyPath).BaseName -eq "AzCopy")
{
    $FileItemVersion = (Get-Item $AzCopyPath).VersionInfo
    $FilePath = ("{0}.{1}.{2}.{3}" -f  $FileItemVersion.FileMajorPart,  $FileItemVersion.FileMinorPart,  $FileItemVersion.FileBuildPart,  $FileItemVersion.FilePrivatePart)

    if([version] $FilePath -lt "7.0.0.2")
    {
        $AzCopyPath = Read-Host "Version of AzCopy found at provided path is of a lower, unsupported version. Please input the full filePath of the AzCopy.exe that is version 7.0.0.2 or higher, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe"
    }
}

$OutputLastSuccessContainer = $LastSuccessContainerName;
$HasReachedLastSuccessContainer = $false;

if ($LastSuccessContainerName -eq $null -or $LastSuccessContainerName -eq "")
{
    $HasReachedLastSuccessContainer = $true;
}

# For list container
$MaxReturn = 250
$Token = $Null

do{
    # List source containers
    $retry = 1
    $Error.Clear()
    $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx

    # If list container fail, retry it
    while(($Error.Count -gt 0) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
    {
        Write-Host "Retry List containers $retry"
        $Error.Clear()
        $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx
        $retry++
    }

    # If list container fail after retry, break script
    if ($Error.Count -gt 0){
        Write-Error "Terminating the script since listing source containers failed: $Error[0]";
        $CopyContainerFail = $true
        break;
    }
    $Token = $srcContainers[$srcContainers.Count -1].ContinuationToken;

    # Transfer containers one by one
    $CopyContainerFail = $false
    foreach ($container in $srcContainers)
    {
        if (!$HasReachedLastSuccessContainer)
        {
            if ($container.Name -eq $LastSuccessContainerName)
            {
                $HasReachedLastSuccessContainer = $true;
            }
            Write-Host "Skipping container copy: $($container.Name)"
            Continue;
        }

        Write-Host "Start copying container: $($container.Name)"
        $retry = 1

        # Get AzCopy command for transfer one container
        $destContainer = $destCtx.StorageAccount.CreateCloudBlobClient().GetContainerReference($container.Name)
        $azCopyCmd = [string]::Format("""{0}"" /source:{1} /dest:{2} /sourcekey:""{3}"" /destkey:""{4}"" /snapshot /y /s",$AzCopyPath, $container.CloudBlobContainer.Uri.AbsoluteUri, $destContainer.Uri.AbsoluteUri, $srcStorageAccountKey, $DestStorageAccountKey)
    
        # Execute the AzCopy command first time
        Write-Host "$azCopyCmd"
        $result = cmd /c $azCopyCmd
        foreach($s in $result)
        {
            Write-Host $s 
        }

        # If transfer failed, retry it
        while(($LASTEXITCODE -ne 0) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
        {
            Write-Host "Retry $retry : $azCopyCmd"
            $result = cmd /c $azCopyCmd
            foreach($s in $result)
            {
                Write-Host $s 
            }
            $retry++
        }

        # If tranfer failed after retry, print error
        if ($LASTEXITCODE -ne 0){
            Write-Error "Container copy failed: $($container.Name)";
            $CopyContainerFail = $true
            break;
        }
        else
        {
            Write-Host "Finished copying container: $($container.Name)"
            Write-Host ""
            $OutputLastSuccessContainer = $container.Name
        }
    }
    if($CopyContainerFail)
    {
        break;
    }
}
While ($Token -ne $Null)

# Summary the copy result
if ($CopyContainerFail)
{
    if(($OutputLastSuccessContainer -ne $null) -and ($OutputLastSuccessContainer -ne ""))
    {
        Write-Warning "To resume, rerun the script and append the parameter: ""-LastSuccessContainer $OutputLastSuccessContainer"""
        return $OutputLastSuccessContainer
    }
    else
    {
        Write-Warning "To resume, rerun the script."
        return $null
    }
}
else
{
    Write-Host "All Containers copied successfully."
    return ""
}
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy pro kopírování dat z jednoho účtu úložiště do jiného. Každou položku v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureStorageContainer](/powershell/module/azure.storage/Get-AzureStorageContainer) | Vrátí kontejnery přidruženého k tomuto účtu úložiště. |
| [Nové AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Vytvoří kontextu Azure Storage. |

## <a name="next-steps"></a>Další postup

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další úložiště naleznete v [ukázky pro úložiště objektů Blob v Azure PowerShell](../blobs/storage-samples-blobs-powershell.md).
