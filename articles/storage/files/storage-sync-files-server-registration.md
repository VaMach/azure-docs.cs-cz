---
title: "Registrace a zrušení registrace serveru se synchronizací souboru Azure (preview) | Microsoft Docs"
description: "Naučte se registrovat a zrušit registraci serveru systému Windows pomocí služby Azure Storage synchronizace souboru synchronizace."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 13a75d5cafd94435346660614721399f2d77919b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/19/2017
---
# <a name="registerunregister-a-server-with-azure-file-sync-preview"></a>Registrace a zrušení registrace serveru se synchronizací souboru Azure (preview)
Azure File Sync (Preview) umožňuje centralizovat sdílené složky organizace ve službě Soubory Azure bez ztráty flexibility, výkonu a kompatibility místního souborového serveru. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

V následujícím článku ukazuje způsob registrace a zrušení registrace serveru se službou Sync úložiště. To může potřeby, pokud vyřazuje serveru nebo pokud se požaduje nový koncový bod serveru ve skupině pro synchronizaci. V tématu [nasazení synchronizace souboru Azure (preview)](storage-sync-files-deployment-guide.md) informace o tom, jak nasadit synchronizace souboru Azure klient server.

## <a name="prerequisites"></a>Požadavky
K registraci serveru systému Windows pomocí služby synchronizace úložiště, musíte nejdřív připravit na Windows Server s nezbytných předpokladů:

* Ujistěte se, že byla nasazena synchronizační služby úložiště. Další informace o tom, jak nasadit službu synchronizace úložiště najdete v tématu [nasazení synchronizace souboru Azure (preview)](storage-sync-files-deployment-guide.md).
* Ujistěte se, že je server připojen k Internetu a zda je přístupný Azure.
* Zakažte konfigurace rozšířeného zabezpečení pro správce pomocí uživatelského rozhraní správce serveru.
    
    ![Konfigurace rozšířeného zabezpečení Internet Exploreru zvýrazněná uživatelské rozhraní správce serveru](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Zkontrolujte, zda je na serveru nainstalován modul AzureRM PowerShell. Pokud váš server je členem clusteru s podporou převzetí služeb při selhání, každý uzel v clusteru, bude vyžadovat AzureRM modulu. Další informace o tom, jak nainstalovat modul AzureRM naleznete na [instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Doporučujeme používat nejnovější verze modulu AzureRM PowerShell registrace nebo odregistrace serveru. Pokud balíček AzureRM byl dříve nainstalován na tomto serveru (a verze prostředí PowerShell na tomto serveru je 5.* nebo vyšší), můžete použít `Update-Module` rutiny tento balíček aktualizace. 

## <a name="register-a-server-with-storage-sync-service"></a>Registraci serveru se službou Sync úložiště
Před použitím serveru systému Windows jako *koncový bod serveru* v souboru synchronizace služby Azure *skupiny synchronizace*, musí být zaregistrovaný u *úložiště synchronizační služba*. Server se dají registrovat jenom s jedním *úložiště synchronizační služba* najednou.

### <a name="install-the-azure-file-sync-agent"></a>Nainstalujte agenta synchronizace souboru Azure
1. [Stáhnout agenta synchronizace souboru Azure](https://go.microsoft.com/fwlink/?linkid=858257).
2. Spusťte instalační program agenta synchronizace souboru Azure.
    
    ![V první podokně instalační program agenta synchronizace souboru Azure](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Je nutné povolit aktualizace agenta synchronizace souboru Azure pomocí služby Microsoft Update. Je důležité, protože kritické zabezpečení opravy a nové funkce a vylepšení balíčku serveru jsou sice přes Microsoft Update.

    ![Ujistěte se, že služba Microsoft Update je povolena v podokně Microsoft Update instalační program agenta synchronizace souboru Azure](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Pokud server nebyl dříve registrován, registrace serveru uživatelského rozhraní objeví se okamžitě po dokončení instalace.

> [!Important]  
> Pokud je server členem clusteru s podporou převzetí služeb při selhání, agent synchronizace souboru Azure musí být nainstalovaný na všech uzlech v clusteru.

### <a name="register-the-server-using-the-server-registration-ui"></a>Registrace serveru pomocí registrace serveru uživatelského rozhraní

> [!Important]  
> Cloud Solution Provider odběry nelze použít registrace serveru uživatelského rozhraní. Místo toho používejte PowerShell (níže v této části).

1. Pokud registrace serveru uživatelského rozhraní se nepodařilo spustit okamžitě po dokončení instalace agenta Azure souboru Sync, ho můžete spustit ručně spuštěním `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Klikněte na tlačítko *přihlášení* pro přístup k předplatnému Azure. 

    ![Otevřete dialogové okno uživatelského rozhraní registrace serveru](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Vyberte správné předplatné, skupinu prostředků a úložiště synchronizační služby z tohoto dialogového okna.

    ![Informace o synchronizaci služby úložiště](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Ve verzi preview jeden další přihlášení je předpokladem pro dokončení procesu. 

    ![Přihlaste se dialogové okno](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Pokud je server členem clusteru s podporou převzetí služeb při selhání, každý server je potřeba spustit registrací serveru. Při zobrazení servery zaregistrované na portálu Azure, Azure souboru Sync automaticky rozpozná každý uzel jako členem stejného clusteru převzetí služeb při selhání a skupin je společně správně.

### <a name="register-the-server-with-powershell"></a>Registrace serveru pomocí prostředí PowerShell
Můžete také provést registraci serveru pomocí prostředí PowerShell. Toto je jediným podporovaným způsobem registrace serveru pro poskytovatele Cloud Solution Provider odběry:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

## <a name="unregister-the-server-with-storage-sync-service"></a>Zrušte registraci serveru se službou Sync úložiště
Existuje několik kroků, potřebné ke zrušení registrace serveru se službou Sync úložiště. Podívejme se na to, jak se správně zrušit registraci serveru.

### <a name="optional-recall-all-tiered-data"></a>(Volitelné) Odvolat vrstvené všechna data
Když je povolené pro koncový bod serveru, cloudové vrstvení bude *vrstvy* soubory do vaší sdílené složky Azure File. To umožňuje místní sdílené složky tak, aby fungoval jako mezipaměť, nikoli úplnou kopii datovou sadu, aby efektivní využití místa na souborovém serveru. Ale pokud serveru koncového bodu se odebere při vrstvené soubory stále místně na serveru, tyto soubory se stanou nelze získat přístup. Proto v případě, že se požaduje přístup k souborům, je nutné odvolat všechny vrstvené soubory ze souborů Azure před pokračováním v zrušení registrace. 

To můžete provést pomocí rutiny prostředí PowerShell, jak je uvedeno níže:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Pokud místní svazek, který je hostitelem serveru nemá dostatek volného místa pro navrácení všech vrstvené souborů `Invoke-StorageSyncFileRecall` rutiny se nezdaří.  

### <a name="remove-the-server-from-all-sync-groups"></a>Odeberte tento server ze všech skupin synchronizace
Před zrušení registrace serveru na synchronizační služby úložiště, je třeba odebrat všechny koncové body serveru pro tento server. To lze provést prostřednictvím portálu:

1. Přejděte do služby Sync úložiště, kde je server zaregistrován.
2. Odeberte všechny koncové body serveru pro tento server v každé skupině synchronizace v úložišti služby synchronizace. To můžete provést kliknutím pravým tlačítkem na příslušné serveru koncového bodu v podokně synchronizace skupiny.

    ![Odebrání koncového bodu serveru ze skupiny synchronizace](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

To můžete udělat taky pomocí jednoduchého skriptu prostředí PowerShell:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Login-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

### <a name="unregister-the-server"></a>Zrušte registraci serveru
Teď, když byl odvolán všechna data a serveru byla odebrána ze všech skupin, synchronizace, může se zrušit registraci serveru. 

1. Na portálu Azure přejděte v části "Registrované servery" synchronizace služby úložiště.
2. Klikněte pravým tlačítkem myši na serveru, kterou chcete zrušit registraci a klikněte na tlačítko "Zrušit registraci serveru".

    ![Zrušení registrace serveru](media/storage-sync-files-server-registration/unregister-server-1.png)