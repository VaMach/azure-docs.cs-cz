---
title: "Vytvořte kontejner cloudové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vysvětluje, jak vytvořit kontejner cloudové služby pomocí prostředí PowerShell. Kontejner hostuje webové a pracovní role."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 2023fa7b318f9f76ce1e1ea0a46110297be9a001
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Použít příkaz prostředí Azure PowerShell k vytvoření kontejner prázdný cloudové služby
Tento článek vysvětluje, jak rychle vytvořit kontejner cloudové služby pomocí rutin prostředí Azure PowerShell. Postupujte podle následujících kroků:

1. Nainstalovat rutiny Powershellu pro Microsoft Azure z [prostředí Azure PowerShell stáhne](http://aka.ms/webpi-azps) stránky.
2. Otevřete příkazový řádek prostředí PowerShell.
3. Použití [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) k přihlášení.

   > [!NOTE]
   > Další pokyny k instalaci rutiny Azure Powershellu a připojení k předplatnému Azure, najdete v části [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Použití **New-AzureService** vytvořte kontejner prázdný Azure cloud service.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Použijte tento příklad k vyvolání rutiny:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Další informace o vytváření Azure cloudové služby spusťte příkaz:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Další kroky
* Ke správě nasazení cloudové služby, naleznete [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), a [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) příkazy. Můžete také použít odkaz na [postup konfigurace cloudové služby](cloud-services-how-to-configure.md) Další informace.
* Publikovat projekt cloudové služby Azure, najdete v tématu **PublishCloudService.ps1** ukázka kódu z [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).
