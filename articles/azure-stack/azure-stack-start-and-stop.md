---
title: "Spuštění a zastavení zásobník Azure | Microsoft Docs"
description: "Zjistěte, jak pro spuštění a vypnutí zásobník Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: mabrigg
ms.openlocfilehash: 09c22b22b49664a55b8a9e77936df3b1362d5b88
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="start-and-stop-azure-stack"></a>Spuštění a zastavení Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy (verze 1712 a novější)*

## <a name="stop-azure-stack"></a>Zastavit Azure zásobníku 

Pomocí následujících kroků vypněte zásobník Azure:

1. Otevřete privilegovaný koncový bod relace období k virtuálním počítačům Azure zásobníku ERCS z počítače s přístupem k síti. Pokyny najdete v tématu [pomocí privilegované koncový bod v zásobníku Azure](azure-stack-privileged-endpoint.md).

2. Z období spusťte:

    ```powershell
      Stop-AzureStack
    ```

3. Počkejte všechny fyzické uzly zásobník Azure napájení vypnuto.

> [!Note]  
> Podle pokynů od výrobce (OEM) kdo zadaný hardwaru zásobník Azure můžete ověřit stav napájení fyzického uzlu. 

## <a name="get-the-startup-status-for-azure-stack"></a>Získat stav spuštění Azure zásobníku

Získáte spuštění pro spuštění rutiny zásobník Azure pomocí následujících kroků:

1. Otevřete relaci privilegované koncového bodu z počítače s přístupem k síti na virtuálních počítačích Azure zásobníku ERCS.

2. Z období spusťte:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="start-azure-stack"></a>Spustit Azure zásobníku 

Spusťte zásobník Azure pomocí následujících kroků. Postupujte podle těchto kroků bez ohledu na to, jak Azure zásobníku zastavena.

1. Napájení na všech fyzických uzlů ve vašem prostředí Azure zásobníku. Zapnutí pokyny pro fyzických uzlů ověřte podle pokynů od výrobce (OEM) kdo zadaná hardware pro vaši Azure zásobníku.

2. Počkejte na spuštění služby infrastruktury Azure zásobníku. Služby infrastruktury Azure zásobníku může vyžadovat dvou hodin k dokončení procesu spuštění. Počáteční stav zásobníku Azure s si můžete ověřit [ **Get-ActionStatus** rutiny](#get-the-startup-status-for-azure-stack).

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Řešení potíží s spuštění a vypnutí Azure zásobníku

Pokud nemáte služby infrastruktury a klientů úspěšně spustit po zapnutí prostředí Azure zásobníku, proveďte následující kroky. 

1. Otevřete relaci privilegované koncového bodu z počítače s přístupem k síti na virtuálních počítačích Azure zásobníku ERCS.

2. Spusťte: 

    ```powershell
      Test-AzureStack
      ```

3. Zkontrolujte výstup a vyřešte případné chyby stavu. Další informace najdete v tématu [spustit test pro ověření Azure zásobníku](azure-stack-diagnostic-test.md).

4. Spusťte:

    ```powershell
      Start-AzureStack
    ```

5. Pokud systém **Start-AzureStack** důsledkem chyby, obraťte se na zákaznickou podporu služeb společnosti Microsoft. 

## <a name="next-steps"></a>Další postup 

Další informace o Azure zásobníku nástroj pro diagnostiku a vydat protokolování najdete v tématu [Zásobník Azure diagnostické nástroje. Diagnostické nástroje Azure zásobníku. Diagnostické nástroje Azure zásobníku. Diagnostické nástroje Azure zásobníku.