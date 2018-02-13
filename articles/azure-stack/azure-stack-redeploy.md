---
title: "Znovu nasaďte Azure zásobníku | Microsoft Docs"
description: "Znovu nasaďte Azure zásobníku."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Znovu nasaďte Azure zásobníku
Pokud obdržíte chybu při nasazování zásobník Azure, můžete znovu spustit instalační program, pomocí následujícího příkazu prostředí PowerShell: `.\InstallAzureStackpoc.ps1 -rerun`. Tento příkaz bude znovu spusťte instalační program Azure zásobníku v místě, které se dříve nezdařilo bez nutnosti začít znovu od začátku. Pokud se zobrazí ke stejné chybě instalace může být potřeba provést úplné opětovné nasazení do adres problém. 

K opětovnému nasazení zásobník Azure, musíte spustit přes od začátku níže popsané.

## <a name="steps-to-redeploy-azure-stack"></a>Kroky k opětovnému nasazení Azure zásobníku
1. Na hostiteli development kit, otevřete konzolu prostředí PowerShell se zvýšenými oprávněními > přejděte do skriptu asdk installer.ps1 > Spustit > klikněte na tlačítko **restartovat**.
2. Vyberte základní operační systém (ne **zásobník Azure**) a klikněte na tlačítko **Další**.
3. Po restartování počítače development kit hostitele, odstraňte soubor CloudBuilder.vhdx, který byl použit v rámci předchozí nasazení.
4. [Nasazení development kit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Další postup
[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)

