---
title: "Povolit zálohování pro zásobník Azure z portálu pro správu | Microsoft Docs"
description: "Povolte službu infrastruktura zálohování prostřednictvím portálu pro správu, aby zásobník Azure můžete obnovit, pokud dojde k selhání."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 456a0db9771f5963c8d4375d54a22257f6ca1c56
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Povolit zálohování pro zásobník Azure z portálu pro správu

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Povolte službu infrastruktura zálohování prostřednictvím portálu pro správu tak, aby zásobník Azure může generovat zálohy. Tyto zálohy lze použít k obnovení vašeho prostředí v případě selhání.

> [!Note]  
> Než povolíte zálohování prostřednictvím konzoly, musíte nakonfigurovat službu zálohování. Můžete nakonfigurovat pomocí prostředí PowerShell služby zálohování. Další informace najdete v tématu [povolit zálohování pro zásobník Azure pomocí prostředí PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Povolení zálohování

1. Otevřete na portálu správy Azure zásobníku na [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Vyberte **další služby** > **infrastruktura zálohování**. Zvolte **konfigurace** v **infrastruktura zálohování** okno.

    ![Azure zásobníku – nastavení zálohování řadiče](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Zadejte cestu ke **umístění úložiště zálohy**. Pomocí řetězce Universal Naming Convention (UNC) pro cestu ke sdílené složce hostované na samostatných zařízení. Řetězec UNC Určuje umístění prostředků, jako jsou sdílené soubory nebo zařízení. Pro službu můžete použít IP adresu. K zajištění dostupnosti zálohovaných dat po havárii, zařízení by měl být v samostatné umístění.
    > [!Note]  
    > Pokud vaše prostředí podporuje překlad adres ze sítě infrastruktury Azure zásobníku podnikovém prostředí, můžete použít plně kvalifikovaný název domény, nikoli IP adresu.
4. Typ **uživatelské jméno** pomocí domény a uživatelské jméno. Například, `Contoso\administrator`.
5. Typ **heslo** pro uživatele.
5. Zadejte heslo znovu **Potvrdit heslo**.
6. Zadat předsdílený klíč v **šifrovací klíč** pole. Záložní soubory jsou šifrované pomocí tohoto klíče. Ujistěte se, že tento klíč uložit na bezpečném místě. Jakmile poprvé nastavit tento klíč nebo klíč Otočit v budoucnu, nelze zobrazit, tento klíč z tohoto rozhraní. Další pokyny ke generování předsdílený klíč, postupujte podle skripty v [povolit zálohování pro zásobník Azure pomocí prostředí PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Vyberte **OK** uložte nastavení zálohování řadiče.

K provedení zálohy, musíte ke stažení nástroje Azure zásobníku a poté spusťte rutinu prostředí PowerShell **Start-AzSBackup** vašeho uzlu Azure zásobníku správy. Další informace najdete v tématu [zálohování Azure zásobníku](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Další postup

 - Naučte se spustit zálohování. V tématu [zálohování Azure zásobníku](azure-stack-backup-back-up-azure-stack.md ).
- Naučte se ověřit, jestli vaše zálohování spustila. V tématu [zálohování potvrzení byla dokončena v portálu pro správu](azure-stack-backup-back-up-azure-stack.md ).
