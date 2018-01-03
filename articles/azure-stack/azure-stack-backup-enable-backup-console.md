---
title: "Povolit zálohování pro zásobník Azure z portálu pro správu | Microsoft Docs"
description: "Službu lze povolte infrastruktury zpět pomocí portálu pro správu, aby zásobník Azure můžete obnovit, pokud dojde k selhání."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Povolit zálohování pro zásobník Azure z portálu pro správu

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Infrastruktura zpět službu povolte pomocí portálu pro správu tak, aby zásobník Azure může generovat zálohy. Pomocí těchto zálohuje ups k obnovení vašeho prostředí, pokud dojde k selhání.

## <a name="enable-backup"></a>Povolení zálohování

1. Otevřete na portálu správy Azure zásobníku na [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Vyberte **další služby** > **infrastruktura zálohování**. Zvolte **konfigurace** v **infrastruktura zálohování** okno.

    ![Azure zásobníku – nastavení zálohování řadiče](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Zadejte cestu ke **umístění úložiště zálohy**. Je nutné použít řetězec Universal Naming Convention (UNC) pro cestu sdílené složky hostované na samostatných zařízení. Řetězec UNC Určuje umístění prostředků, jako jsou sdílené soubory nebo zařízení. Pro službu můžete použít IP adresu. Aby se zajistila dostupnost zálohování dat v případě havárie, by měl být zařízení v samostatných umístění.
    > [!Note]  
    > Pokud vaše prostředí podporuje překlad adres ze sítě infrastruktury Azure zásobníku podnikovém prostředí, můžete použít plně kvalifikovaný název domény, nikoli IP adresu.
4. Typ **uživatelské jméno** pomocí domény a uživatelské jméno. Například, `Contoso\administrator`.
5. Typ **heslo** pro uživatele.
5. Zadejte heslo znovu **Potvrdit heslo**.
6. Zadat předsdílený klíč v **šifrovací klíč** pole. Záložní soubory jsou šifrované pomocí tohoto klíče. Ujistěte se, že tento klíč uložit na bezpečném místě. Jakmile poprvé nastavit tento klíč nebo klíč Otočit v budoucnu, nelze zobrazit, tento klíč z tohoto rozhraní. Další pokyny ke generování předsdílený klíč, postupujte podle skripty v [povolit zálohování pro zásobník Azure pomocí prostředí PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Vyberte **OK** uložte nastavení zálohování řadiče.

K provedení zálohy, musíte ke stažení nástroje Azure zásobníku a poté spusťte rutinu prostředí PowerShell **Start-AzSBackup** vašeho uzlu Azure zásobníku správy. Další informace najdete v tématu [zálohování Azure zásobníku](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Další postup

 - Další informace ke spuštění zálohování, naleznete v [zálohování Azure zásobníku](azure-stack-backup-back-up-azure-stack.md ).
- Naučte se ověřit, jestli vaše zálohování spustila najdete v tématu [zálohování potvrzení byla dokončena v portálu pro správu](azure-stack-backup-back-up-azure-stack.md ).
