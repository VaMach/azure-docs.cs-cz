---
title: "Připojení sdílené složky Azure a přístup k ní v systému Windows | Dokumentace Microsoftu"
description: "Připojení sdílené složky Azure a přístup k ní v systému Windows."
services: storage
documentationcenter: na
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
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: a6d3a6f6e3457c84c5a7dc7d3601ef9495c060fe
ms.contentlocale: cs-cz
ms.lasthandoff: 07/12/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Připojení sdílené složky Azure a přístup k ní v systému Windows
[Azure File Storage](storage-dotnet-how-to-use-files.md) je snadno použitelný cloudový systém souborů od Microsoftu. Sdílené složky Azure je možné připojit v systémech Windows a Windows Server. Tento článek ukazuje tři různé způsoby připojení sdílené složky Azure v systému Windows: pomocí uživatelského rozhraní Průzkumníka souborů, přes PowerShell a přes příkazový řádek. 

Aby bylo možné připojit sdílenou složku Azure mimo oblast, ve které je hostovaná, například v místním prostředí nebo jiné oblasti Azure, operační systém musí podporovat protokol SMB 3.x. Následující tabulka uvádí verze protokolu SMB v posledních vydaných verzí systému Windows:

| Verze systému Windows | Verze protokolu SMB | Podporuje připojení z virtuálního počítače Azure | Podporuje připojení z místního prostředí | Minimální doporučená aktualizace KB |
|----|----|----|----|----|
| Windows 10 verze 1703 | SMB 3.1.1 | Ano | Ano | |
| Windows Server 2016 | SMB 3.1.1 | Ano | Ano | [KB4015438](https://support.microsoft.com/help/4015438) |
| Windows 10 verze 1607 | SMB 3.1.1 | Ano | Ano | [KB4015438](https://support.microsoft.com/help/4015438) | 
| Windows 10 verze 1511 | SMB 3.1.1 | Ano | Ano | [KB4013198](https://support.microsoft.com/help/4013198) |
| Windows 10 verze 1507 | SMB 3.1.1 | Ano | Ano | [KB4012606](https://support.microsoft.com/help/4012606) | 
| Windows 8.1 | SMB 3.0.2 | Ano | Ano | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 R2 | SMB 3.0.2 | Ano | Ano | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 | SMB 3.0 | Ano | Ano | [KB4012214](https://support.microsoft.com/help/4012214) |
| Windows 7 | SMB 2.1 | Ano | Ne | [KB4012215](https://support.microsoft.com/help/4012215) |
| Windows Server 2008 R2 | SMB 2.1 | Ano | Ne | [KB4012215](https://support.microsoft.com/help/4012215) |

> [!Note]  
> Vždy doporučujeme získat nejnovější aktualizaci KB pro vaši verzi systému Windows. Smyslem minimální doporučené aktualizace KB je poskytnout nejnovější balíček obsahující opravy protokolu SMB pro správce IT, kteří odmítají aktualizace.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Požadavky pro připojení sdílené složky Azure v systému Windows 
* **Název účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat název účtu úložiště.

* **Klíč účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat primární (nebo sekundární) klíč úložiště. Klíče SAS aktuálně nejsou pro připojení podporovány.

* **Ujistěte se, že je otevřený port 445:** Azure File Storage používá protokol SMB. Protokol SMB komunikuje přes protokol TCP 445 – zkontrolujte, že brána firewall neblokuje port TCP 445 z klientského počítače.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>Připojení sdílené složky Azure pomocí Průzkumníka souborů
> [!Note]  
> Mějte na paměti, že následující pokyny jsou ukázané na systému Windows 10 a ve starších vydaných verzích se můžou mírně lišit. 

1. **Otevřete Průzkumníka souborů:** Můžete to provést otevřením z nabídky Start nebo stisknutím klávesové zkratky Win+E.

2. **Na levé straně okna přejděte na položku Tento počítač. Tím se změní dostupné nabídky na pásu karet. Z nabídky Počítač vyberte Připojit síťovou jednotku**.
    
    ![Snímek obrazovky s rozevírací nabídkou Připojit síťovou jednotku](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Zkopírujte cestu UNC z podokna Připojit na webu Azure Portal:** Podrobný popis toho, jak tuto informaci najít, najdete [tady](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![Cesta UNC z podokna Připojit služby Azure File Storage](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Vyberte písmeno jednotky a zadejte cestu UNC.** 
    
    ![Snímek obrazovky s dialogovým oknem Připojit síťovou jednotku](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Použijte název účtu úložiště s předponou `Azure\` jako uživatelské jméno a klíč účtu úložiště jako heslo.**
    
    ![Snímek obrazovky s dialogovým oknem Přihlašovací údaje k síti](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Používejte sdílenou složku Azure, jak potřebujete**.
    
    ![Sdílená složka Azure je teď připojená](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Až budete připraveni sdílenou složku Azure odpojit, můžete to provést tak, že v Průzkumníku souborů kliknete pravým tlačítkem na položku sdílené složky v části Umístění v síti a vyberete Odpojit**.

## <a name="mount-the-azure-file-share-with-powershell"></a>Připojení sdílené složky Azure pomocí PowerShellu
1. **Pomocí následujícího příkazu připojte sdílenou složku Azure:** Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` správnými informacemi.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Používejte sdílenou složku Azure, jak potřebujete**.

3. **Až budete hotovi, odpojte sdílenou složku Azure pomocí následujícího příkazu**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> U příkazu `New-PSDrive` můžete použít parametr `-Persist`, který zajistí, že sdílená složka Azure bude během připojení viditelná v celém operačním systému.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>Připojení sdílené složky Azure pomocí příkazového řádku
1. **Pomocí následujícího příkazu připojte sdílenou složku Azure:** Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` správnými informacemi.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Používejte sdílenou složku Azure, jak potřebujete**.

3. **Až budete hotovi, odpojte sdílenou složku Azure pomocí následujícího příkazu**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Sdílenou složku Azure můžete nakonfigurovat tak, aby se po restartování automaticky znovu připojila díky uchování přihlašovacích údajů v systému Windows. Následující příkaz zajistí uchování přihlašovacích údajů:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak používat Azure File Storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Podpora nástrojů pro službu Azure File Storage
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Řešení potíží se službou Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat do služby Soubory Azure](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
