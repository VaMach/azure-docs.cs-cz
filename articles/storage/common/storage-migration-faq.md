---
title: "Azure migrace úložiště – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi na časté otázky týkající se migrace úložiště Azure"
services: storage
documentationcenter: na
author: genlin
manager: timlt
editor: tysonn
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 11/16/2017
ms.author: genli
ms.openlocfilehash: 516a0487afe11ef6915a002375661a23eaf13edc
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Nejčastější dotazy o migraci úložiště Azure

Tento článek obsahuje odpovědi na časté otázky týkající se migrace úložiště Azure. 

## <a name="faq"></a>Nejčastější dotazy

**Vytvoření skriptu pro kopírování souborů z jednoho kontejneru typu do jiného**

Pokud chcete zkopírovat soubory mezi kontejnery, můžete pomocí nástroje AzCopy. Podívejte se na následující příklad:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

Používá AzCopy [kopírovat objekt Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) udělat kopii pro každý soubor v kontejneru.  
  
Můžete použít všechny virtuální počítače nebo místní počítač, který má přístup k Internetu ke spuštění nástroje AzCopy. Azure Batch plán můžete také použít k tomu automaticky, ale má složitěji.  
  
Skriptu pro automatizaci je určená pro nasazení Azure Resource Manager místo úložiště obsahu manipulaci. Další informace najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Je k dispozici žádné poplatků zahrnuté pro kopírování dat mezi dvěma sdílenými složkami jiný soubor ve stejném účtu úložiště ve stejné oblasti?**

Ne. Není nijak zpoplatněn pro tento proces.

**Jak mohu zálohovat Moje celý účet úložiště na jiný účet úložiště?**

Neexistuje žádná možnost zálohovat přímo celý účet úložiště. Ale můžete ručně přesunout kontejneru v daném účtu úložiště na jiný účet pomocí AzCopy nebo Storage Explorer. Následující kroky ukazují, jak pomocí nástroje AzCopy pro přesun kontejneru:  
 

1.  Nainstalujte [AzCopy](storage-use-azcopy.md) nástroj příkazového řádku. Tento nástroj umožňuje přesunout soubor virtuálního pevného disku mezi účty úložiště.

2.  Po instalaci nástroje AzCopy v systému Windows pomocí Instalační služby, otevřete okno příkaz POromprt a pak přejděte do složky pro instalaci AzCopy ve vašem počítači. Ve výchozím nastavení, je nainstalován nástroj AzCopy k **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** nebo **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.   Spusťte následující příkaz pro přesun kontejneru. Text je potřeba nahradit aktuální hodnotu.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

        - / source: Zadejte účet úložiště zdrojové identifikátor URI (až kontejner)  
        - / dest: Zadejte účet úložiště, cílový identifikátor URI (až kontejner)  
        - /sourcekey: Zadejte zdrojového úložiště primární klíč účtu, můžete zkopírovat tento klíč z portálu výběrem účtu úložiště.  
        - /destkey: Zadejte úložiště v cíli primární klíč účtu, můžete zkopírovat tento klíč z portálu výběrem účtu úložiště.

Po provedení tohoto příkazu kontejneru soubory přesunou do cílového účtu úložiště.

**Rozhraní příkazového řádku AzCopy nefunguje společně s přepínačem "Vzor" při kopírování z jednoho Azure blob do jiného.**

Můžete přímo kopírovat a upravit AzCopy cmd a zkontrolovat a ujistěte se, že vzor odpovídá zdroji. Také zkontrolujte, zda **/S** zástupné znaky jsou platné. Další informace najdete v tématu [AzCopy parametry](storage-use-azcopy.md).

**Přesunutí dat z jednoho kontejneru úložiště do druhého?**

Postupujte přitom takto:

1.  Vytvoření kontejneru (složka) v cílovém objektu blob.

2.  Použití [Azcopy](https://azure.microsoft.com/en-us/blog/azcopy-5-1-release/) zkopírovat obsah z původní kontejner objektů blob do různých kontejneru objektů Blob.

**Vytvoření skriptu prostředí PowerShell pro přesun dat z jedné sdílenou složku Azure do jiného úložiště Azure**

Pomocí nástroje AzCopy pro přesun dat z jednu sdílenou složku Azure do jiného úložiště azure. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Jak načíst .csv velké soubory do úložiště Azure?**

Pomocí nástroje AzCopy k nahrání souborů .csv velké do úložiště Azure. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Je nutné přesunout protokoly z "jednotka D" k mému účtu úložiště Azure každý den. Jak to mohu automatizovat?**

Můžete pomocí nástroje AzCopy a vytvořit úlohu v Plánovači úloh. Nahrání souborů do účtu úložiště Azure pomocí skriptu batch AzCopy. Další informace najdete v tématu [postupy pro konfiguraci a spuštění úlohy spuštění pro cloudové služby](../../cloud-services/cloud-services-startup-tasks.md).

**Jak přesunout svůj účet úložiště mezi předplatnými?**

Pomocí nástroje AzCopy přesunout účtu úložiště mezi předplatnými. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Jak lze přesunout 10 TB dat do úložiště v jiné oblasti?**

Pomocí nástroje AzCopy přesouvat data. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Zkopírování dat z místního do úložiště Azure?**

Pomocí AzCopy kopírovat data. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Jak lze přesunout data z místního souboru službě Azure?**

Pro přesun dat pomocí nástroje AzCopy. Další informace najdete další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Mapování kontejneru složky na virtuálním počítači**

Použijte sdílenou složku Azure.

**Jak mohu zálohovat Azure file storage?**

Neexistuje žádné řešení zálohování. Soubory Azure ale také podporují asynchronní kopírování. Ano, můžete zkopírovat soubory ze sdílené složky do jiné sdílené složce (v rámci účtu úložiště nebo na jiný účet úložiště) nebo na kontejner objektů blob (v rámci účtu úložiště nebo na jiný účet úložiště).
Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md).

**Přesunutí spravovaných disků na jiný účet úložiště**

Postupujte přitom takto:

1.  Zastavte virtuální počítač, který je spravovaný disk připojen k.

2.  Spravované disku VHD kopírovat z jedné oblasti do jiného spuštěním následujícího skriptu prostředí Azure PowerShell:

    ```
    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId <ID>

    $sas = Grant-AzureRmDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzureStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  Vytvoření spravovaného disku pomocí souboru virtuálního pevného disku v jiné oblasti, do které jste zkopírovali virtuální pevný disk. Chcete-li to provést, spusťte následující skript prostředí PowerShell Azure:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ``` 

Další informace o tom, jak nasadit virtuální počítač z se spravovaným diskem, najdete v části [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**Jak lze stáhnout o 1 – 2 TB dat z portálu Azure?**

Chcete-li stáhnout data pomocí nástroje AzCopy. Další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).

**Změna sekundárního umístění pro Evropu oblast pro účet úložiště**

Pokud vytvoříte účet úložiště, vyberte primární oblasti pro účet. Výběr sekundární oblast je založený na primární oblasti a nelze ji změnit. V tématu [replikace Azure Storage](storage-redundancy.md).

**Kde je můžete získat další informace o šifrování pro služby úložiště Azure (SSE)?**  
  
Viz následující články:

-  [Průvodce zabezpečením služby Azure Storage](storage-security-guide.md)

-   [Šifrování služby úložiště Azure pro Data v klidovém stavu](storage-service-encryption.md)

**Jak přesunout nebo stahování dat z účtu úložiště?**

Chcete-li stáhnout data pomocí nástroje AzCopy. Další informace najdete další informace najdete v tématu [přenos dat pomocí AzCopy v systému Windows](storage-use-azcopy.md) a [přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md).


**Jak šifrují data v účtu úložiště?**

Jakmile povolíte šifrování v účtu úložiště, stávající data nejsou šifrována. K šifrování stávající data, je potřeba načíst znovu k datům a účet úložiště.  Postupujte přitom takto:

Pomocí nástroje AZcopy kopírovat data do jiný účet úložiště a poté přesuňte zpět na účet úložiště. Můžete také použít [šifrování v klidovém stavu](storage-service-encryption.md).

**Jak můžete stáhnout virtuální pevný disk na místním počítači, jiné než pomocí možnosti stahování na portálu?**

Můžete použít [Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) ke stažení virtuální pevný disk.

**Jsou všechny požadavky pro změna replikace účtu úložiště z GRS na LRS?**

Ne. 

**Jak získám přístup k Azure Files redundantní úložiště?**

Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) je nutné pro přístup k redundantní úložiště. Soubory Azure však podporuje pouze LRS a standardní GRS, který neumožňuje přístup jen pro čtení. 

**Přesunutí ze služby Storage úrovně Premium do standardního úložiště?**

Postupujte přitom takto:

1.  Vytvořit nový účet úložiště Standard Storage (nebo můžete použít existující účet standardního úložiště v rámci vašeho předplatného).

2.  Stáhněte si nástroj AzCopy. Spusťte jeden z následujících příkazů AzCopy.
      
    Zkopírujte celou disky v účtu úložiště:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Pokud chcete zkopírovat jenom jeden disk, zadejte název disku ve vzoru

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
Může trvat několik hodin k dokončení operace.

Abyste měli jistotu, že přenos byla úspěšně dokončena, zkontrolujte cílový kontejner účet úložiště na portálu Azure. Po disky se zkopírují do účet standardního úložiště, můžete je připojit k virtuálnímu počítači jako existující disk. Další informace najdete v tématu [jak připojit spravované datový disk k virtuálnímu počítači Windows na portálu Azure](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Jak převést do úložiště úrovně Premium pro sdílené složky?**

Storage úrovně Premium není povolena na sdílenou složku Azure.

**Jak se dá upgradovat z standardní úložiště pro účet služby Premium Storage? Jak se ponížit ze služby Premium Storage na standardní účet úložiště?**

- Musíte vytvořit cílový účet úložiště, kopírování dat z účet zdrojového do cílového účtu a pak odstraňte účet zdrojové.

- Pro kopírování dat můžete použít nástroje, jako je AzCopy.

- Pokud máte také virtuální počítače, existuje několik dalších kroků, které je nutné provést před zahájením migrace data účtu úložiště. Další informace najdete v tématu [migrace na Storage úrovně Premium (nespravované disky)](storage-migration-to-premium-storage.md).

**Přesunutí z účtu úložiště classic k účtu úložiště Azure Resource Manager?**

1.  Můžete použít rutinu Move-AzureStorageAccount.

2.  Tato rutina má několik kroků (ověřením, Příprava, potvrzení) a před provedením ve skutečnosti můžete ověřit přesunutí.

3.  Pokud máte také virtuální počítače, existuje několik dalších kroků, které je nutné provést před migrací dat účet úložiště. Další informace najdete v tématu [IaaS migraci prostředků z klasického do Azure Resource Manageru pomocí prostředí Azure PowerShell](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Jak stahování dat do počítače se systémem Linux z účtu úložiště Azure, a ukládat data z počítač s Linuxem?**

Pomocí rozhraní příkazového řádku Azure.

-   Stáhnout jediného objektu blob

        azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

-   Odešlete jediného objektu blob: 

        azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Jak můžete I poskytnout ostatním přístup k Moje prostředky úložiště?**

Ostatní uživatelé poskytnout přístup k prostředkům úložiště:

-   Pomocí tokenu sdíleného přístupového podpisu (SAS) k poskytování přístupu k prostředku. 

-   Zadejte uživatele s primární nebo sekundární klíč pro účet úložiště. Další informace najdete v tématu [spravovat váš účet úložiště](storage-create-storage-account.md#manage-your-storage-account).

-   Změňte nastavení zásad přístupu k povolení anonymního přístupu. Další informace najdete v tématu [udělit anonymním uživatelům oprávnění ke kontejnerům a objektům blob](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**Kde je nainstalován nástroj AzCopy?**

-   Pokud máte přístup k AzCopy z "příkazového řádku Microsoft Azure Storage" typ "AzCopy." Na příkazovém řádku je nainstalována spolu s AzCopy.

-   Pokud jste nainstalovali 32bitovou verzi, bude umístěn zde: **% ProgramFiles(x86) %\\Microsoft SDKs\\Azure\\AzCopy.**

-   Pokud jste nainstalovali verzi 64-bit, bude umístěn zde: **% ProgramFiles %\\Microsoft SDKs\\Azure\\AzCopy**.

**Jak se pro účet replikované úložiště (například ZRS, GRS nebo RA-GRS) přístup data, která je uložená v sekundární oblasti?**

-   Pokud používáte Zónově redundantní úložiště (ZRS) nebo geograficky redundantní úložiště (GRS), můžete nelze přistupovat k datům ze sekundární oblasti pokud dojde k selhání. Další informace o procesu převzetí služeb při selhání najdete v tématu [co mají očekávat, pokud dojde k selhání úložiště](storage-disaster-recovery-guidance.md#what-to-expect-if-a-storage-failover-occurs).

-   Pokud používáte geograficky redundantní úložiště s přístupem pro čtení (**RA-GRS**), můžete přistupujete k datům ze sekundární oblasti kdykoli. K tomuto účelu použijte jednu z následujících metod:  
      
    AzCopy: připojit "-sekundární se k názvu účtu úložiště v adrese URL pro přístup sekundárního koncového bodu. Například:  
     
    https://storageaccountname-Secondary.BLOB.Core.Windows.NET/vhds/BlobName.VHD

    SAS Token: pomocí tokenu SAS pro přístup k datům z koncového bodu. Další informace najdete v tématu [pomocí sdílené přístupové podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).

**Jak používat vlastní doménu HTTPS s svůj účet úložiště? Příklad, jak lze vytvořit "https://mystorageaccountname.blob.core.windows.net/images/image.gif" zobrazí jako "https://www.contoso.com/images/image.gif"?**

SSL není aktuálně podporován na účty úložiště s vlastní domény.
Ale můžete použít vlastní domény není HTTPS. Další informace najdete v tématu [konfigurace vlastního názvu doménu pro koncový bod služby Blob storage](../blobs/storage-custom-domain-name.md).

**Jak pomocí protokolu FTP přístup k datům, která je v účtu úložiště**

Neexistuje žádný způsob, jak získat přístup k účtu úložiště přímo pomocí protokolu FTP. Můžete však nastavit virtuální počítač Azure a pak nainstalujte serveru FTP na virtuálním počítači. Server FTP, ukládat soubory ve sdílené složce Azure Files nebo datový disk, který je k dispozici pro virtuální počítač může mít.
Pokud chcete pouze ke stahování dat bez nutnosti použití Průzkumníka úložiště nebo jiné aplikace, pravděpodobně moci používat tokenu SAS. Další informace najdete v tématu [pomocí sdílené přístupové podpisy (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.