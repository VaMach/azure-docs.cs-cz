---
title: "Znovu nasaďte Azure zásobníku | Microsoft Docs"
description: "Znovu nasaďte Azure zásobníku."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: erikje
ms.openlocfilehash: 891cde9b16bbbb51729129b6ad7a0f3794307baa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="redeploy-azure-stack"></a>Znovu nasaďte Azure zásobníku
K opětovnému nasazení zásobník Azure, musíte spustit přes od začátku níže popsané.

## <a name="steps-to-redeploy-azure-stack"></a>Kroky k opětovnému nasazení Azure zásobníku
1. Na hostiteli development kit, otevřete konzolu prostředí PowerShell se zvýšenými oprávněními > přejděte do skriptu asdk installer.ps1 > Spustit > klikněte na tlačítko **restartovat**.
2. Vyberte základní operační systém (ne **zásobník Azure**) a klikněte na tlačítko **Další**.
3. Po restartování počítače development kit hostitele, odstraňte soubor CloudBuilder.vhdx, který byl použit v rámci předchozí nasazení.
4. [Nasazení development kit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Další kroky
[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)

