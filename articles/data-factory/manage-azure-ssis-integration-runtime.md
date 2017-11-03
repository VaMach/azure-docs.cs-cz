---
title: "Správa Azure SSIS integrace runtime | Microsoft Docs"
description: "Naučte se znovu nastavit runtime integrace Azure SSIS v Azure Data Factory po jste ho už zřízené."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: spelluru
ms.openlocfilehash: cc0ed958a9e1018ed9f06fdcc94873ae5420ba95
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="manage-an-azure-ssis-integration-runtime"></a>Správa modulu runtime integrace Azure SSIS
[Vytvoření modulu runtime integrace Azure SSIS](create-azure-ssis-integration-runtime.md) článek ukazuje, jak vytvořit modulu runtime integrace Azure SSIS pomocí Azure Data Factory. Tento článek doplňuje ho tím, že poskytuje informace o tom, jak zastavení, spuštění, znovu nakonfigurovat nebo odebrat modulu runtime integrace Azure SSIS.  

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-introduction.md).

## <a name="stop"></a>Zastavit 
Zastavte modul runtime integrace Azure SSIS. Tento příkaz uvolní všechny jeho uzly a zastaví fakturace.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
```

## <a name="start"></a>Start 
Spusťte modul runtime integrace Azure SSIS. Tento příkaz přiděluje všemi uzly a spustí fakturace.   

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
```

## <a name="reconfigure"></a>Překonfigurujte
Po zřízení a spuštění instance služby Azure SSIS integrace runtime, můžete ji překonfigurovat spuštěním posloupnost `Stop`  -  `Set`  -  `Start` rutiny prostředí PowerShell za sebou. Například následující skript prostředí PowerShell změní počet uzlů přidělených pro instanci modulu runtime integrace Azure SSIS 5.

1. Nejdřív zastavte modul runtime integrace Azure SSIS spuštěním následujícího příkazu:

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Nyní škálování modulu runtime Azure SSIS vaše integrace pět uzlů.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Spusťte modul runtime integrace Azure SSIS. To přiděluje všemi uzly pro spouštění balíčků SSIS.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

## <a name="remove"></a>Odebrat
Odebrat modulu runtime pro existující integrace Azure SSIS, spusťte následující příkaz: 

```powershell
Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
```

## <a name="next-steps"></a>Další kroky
Další informace o běhu Azure SSIS najdete v následujících tématech: 

- [Modul Runtime integrace Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Tento článek obsahuje koncepční informace o integraci runtimes obecně včetně infračerveného signálu Azure SSIS. 
- [Kurz: Nasazení balíčků SSIS do Azure](tutorial-deploy-ssis-packages-azure.md) Tento článek obsahuje podrobné pokyny pro vytvoření Azure-SSIS IR a využívá databázi Azure SQL k hostování katalogu SSIS. 
- [Postup: Vytvoření prostředí Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) Tento článek dál navazuje na tento kurz a obsahuje pokyny k použití spravované instance SQL Azure (privátní verze Preview) a připojení IR k virtuální síti. 
- [Monitorování Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Tento článek ukazuje, jak načíst informace o Azure-SSIS IR a popisy stavů ve vrácených informacích. 
- [Připojení Azure-SSIS IR k virtuální síti](join-azure-ssis-integration-runtime-virtual-network.md) Tento článek obsahuje koncepční informace o připojení Azure-SSIS IR k virtuální síti Azure (VNet). Poskytuje také kroky pro využití webu Azure Portal ke konfiguraci virtuální sítě, aby se k ní prostředí Azure-SSIS IR mohlo připojit. 
