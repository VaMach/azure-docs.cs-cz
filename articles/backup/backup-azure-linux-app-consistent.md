---
title: "Azure Backup: zálohování konzistentní s aplikací virtuálních počítačů Linux | Microsoft Docs"
description: "Chcete-li zaručit zálohování konzistentní s aplikací do Azure, pro virtuální počítače s Linuxem pomocí skriptů. Skripty se vztahují pouze na virtuální počítače s Linuxem v nasazení Resource Manager; skripty se nevztahují na virtuální počítače Windows nebo nasazení service manager. Tento článek vás provede kroky pro konfiguraci skripty, včetně řešení potíží."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "zálohování konzistentní s aplikací; zálohování konzistentní s aplikací virtuálního počítače Azure; Zálohování virtuálních počítačů Linux; Zálohování Azure"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: 378c65bec8fd1f880ed459e76f5e4b5d85e49d2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Zálohování konzistentní s aplikací virtuálních počítačů Linux Azure (preview)

Tento článek hovoří o sady Linux předem skript a po skriptu framework a jak ho lze provést zálohování konzistentní s aplikací Azure Linux virtuálních počítačů.

> [!Note]
> Rozhraní framework skript před a po skript je podporována pouze pro virtuální počítače s Linuxem nasazení Azure Resource Manager. Skripty pro konzistence aplikací nejsou podporovány pro nasazení portálu Service Manager virtuální počítače nebo virtuální počítače s Windows.
>

## <a name="how-the-framework-works"></a>Jak funguje rozhraní

Rozhraní framework poskytuje možnost spustit vlastní skripty před a po skriptů při přenášíte snímky virtuálních počítačů. Předběžné skripty se spouštějí těsně před pořídíte snímek virtuálního počítače, a po skripty se spouštějí ihned po pořídíte snímek virtuálního počítače. To vám dává flexibilní možnost určit aplikaci a prostředí při přenášíte snímky virtuálních počítačů.

V tomto scénáři je důležité zajistit konzistentní s aplikací zálohování virtuálních počítačů. Předběžné skriptu můžete volat rozhraní API nativní aplikace pro uvedení IOs a vyprázdnit obsah v paměti na disk. To zajistí, že snímek bude konzistentní s aplikacemi (to znamená, která se dodává aplikace až když se virtuální počítač spustí po obnovení). Po skriptu slouží k uvolnění IOs. Dělá to pomocí rozhraní API nativní aplikace tak, aby aplikace můžete obnovit normální provozní podmínky post-VM snímku.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Postup konfigurace skript před a po skript

1. Přihlaste se jako kořenový uživatel do virtuálního počítače s Linuxem, které chcete zálohovat.

2. Stáhněte si **VMSnapshotScriptPluginConfig.json** z [Githubu](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig)a potom ho zkopírujete do **/etc/azure** složky na všechny virtuální počítače, které chcete zálohovat. Vytvořte **/etc/azure** adresáře, pokud již neexistuje.

3. Zkopírujte skript před a po skript pro vaši aplikaci na všech virtuálních počítačích, které chcete zálohovat. Skripty můžete zkopírovat do libovolného umístění, do virtuálního počítače. Nezapomeňte aktualizovat úplnou cestu souborů skriptů v **VMSnapshotScriptPluginConfig.json** souboru.

4. Zkontrolujte následující oprávnění pro tyto soubory:

   - **VMSnapshotScriptPluginConfig.json**: oprávnění "600." Například pouze "root" uživatel by měl mít oprávnění "číst" a "zápisu" k tomuto souboru a žádný uživatel by měl mít oprávnění "spustit".

   - **Soubor skriptu před**: oprávnění "700."  Například by měl mít jenom uživatel "root" "číst", "zápisu" a "spustit" oprávnění k tomuto souboru.
  
   - **Po skriptu** oprávnění "700." Například by měl mít jenom uživatel "root" "číst", "zápisu" a "spustit" oprávnění k tomuto souboru.

   > [!Important]
   > Rozhraní framework poskytuje uživatelům spoustu napájení. Je důležité, aby je bezpečné a že pouze "root" uživatel má přístup k souborům kritické JSON a skript.
   > Pokud předchozí požadavky nejsou splněny, skript není spuštěn. Výsledkem je konzistentní zálohování nebo selhání systému souborů.
   >

5. Konfigurace **VMSnapshotScriptPluginConfig.json** podle postupu popsaného tady:
    - **pluginName**: Toto pole, jako je zůstat nebo skripty nemusí fungovat podle očekávání.

    - **preScriptLocation**: zadat úplnou cestu předběžné skriptu ve virtuálním počítači, který se bude zálohovat.

    - **postScriptLocation**: zadat úplnou cestu po skriptu ve virtuálním počítači, který se bude zálohovat.

    - **preScriptParams**: Zadejte volitelné parametry, které je třeba předávané předběžné skriptu. Všechny parametry musí být v uvozovkách a musí být oddělené čárkami, pokud existuje více parametrů.

    - **postScriptParams**: Zadejte volitelné parametry, které je třeba předat po skriptu. Všechny parametry musí být v uvozovkách a musí být oddělené čárkami, pokud existuje více parametrů.

    - **preScriptNoOfRetries**: nastavte počet pokusů před skript by měl opakovat, pokud chybě před ukončením. Zkuste pouze jeden nula znamená a žádné opakování. Pokud dojde k selhání.

    - **postScriptNoOfRetries**: nastavit počet opakování po skript by měl být pokud chybě před ukončením. Zkuste pouze jeden nula znamená a žádné opakování. Pokud dojde k selhání.
    
    - **časový_limit_v_sekundách**: Zadejte jednotlivé časové limity pro předběžné skript a po skript.

    - **continueBackupOnFailure**:, nastavte hodnotu **true** Pokud budete chtít Azure Backup vrátit k konzistentní nebo havárie konzistentní zálohu systému souborů, pokud skript před nebo po skript selže. Toto nastavení na **false** nezdaří zálohování v případě selhání skriptu (s výjimkou případů, kdy máte jeden disk virtuálního počítače, který se vrátí k konzistentní při selhání zálohování bez ohledu na toto nastavení).

    - **fsFreezeEnabled**: Zadejte, zda Linux fsfreeze by měla být volána, když jste pořízení snímku virtuálního počítače k zajištění konzistence systému souborů. Doporučujeme zachovat tohoto nastavení nastavena na **true** Pokud vaše aplikace je závislý na zakázání fsfreeze.

6. Rozhraní framework skriptu je nyní nakonfigurována. Pokud už je nakonfigurované zálohování virtuálních počítačů, další zálohování vyvolá skripty a aktivuje zálohování konzistentní s aplikací. Pokud není nakonfigurováno zálohování virtuálních počítačů, nakonfigurovat ji pomocí [zálohovat virtuální počítače, které jsou k trezory služeb zotavení Azure.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Řešení potíží

Ujistěte se, že přidáte příslušné protokolování při psaní skriptů před a po skriptu a kontrole skriptu protokolů k opravte všechny problémy skriptu. Pokud máte potíže se spouštěním skriptů, naleznete další informace v následující tabulce.

| Chyba | Chybová zpráva | Doporučená akce |
| ------------------------ | -------------- | ------------------ |
| Před ScriptExecutionFailed |Předběžné skript vrátila chybu, takže záloha pravděpodobně nebude konzistentní s aplikací.   | Podívejte se na protokoly selhání vašeho skriptu vyřešit problém.|  
|   POST-ScriptExecutionFailed |    Po skript vrátí chybu, která může mít vliv na stav aplikace. |    Podívejte se na protokoly selhání vašeho skriptu, opravte problém a zkontrolujte stav aplikace. |
| Před ScriptNotFound |  Předběžné skript nebyl nalezen v umístění, který je uveden v **VMSnapshotScriptPluginConfig.json** konfiguračního souboru. |   Zkontrolujte, jestli to před skript se nachází v cestě, který je uveden v konfiguračním souboru, aby zálohování konzistentní s aplikací.|
| POST-ScriptNotFound | Po skript nebyl nalezen v umístění, který je uveden v **VMSnapshotScriptPluginConfig.json** konfiguračního souboru. |   Zkontrolujte, že to po skript se nachází v cestě, který je uveden v konfiguračním souboru, aby zálohování konzistentní s aplikací.|
| IncorrectPluginhostFile | **Pluginhost** souboru, který se dodává s příponou VmSnapshotLinux, je poškozený, takže nelze spustit skript před a po skript a zálohování nebude konzistentní s aplikací. | Odinstalace **VmSnapshotLinux** rozšíření a bude nutné přeinstalovat automaticky s další zálohování na opravě problému. |
| IncorrectJSONConfigFile | **VMSnapshotScriptPluginConfig.json** souboru je nesprávná, takže před skript a po skriptu nelze spustit a zálohování nebude konzistentní s aplikací. | Stáhněte si kopii z [Githubu](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) a nakonfigurujte ji znovu. |
| InsufficientPermissionforPre skriptu | Ke spouštění skriptů, "root" uživatel by měl být vlastníka souboru a soubor musí mít oprávnění "700" (to znamená, musí mít pouze "vlastník" "číst", "zápisu" a "spustit" oprávnění). | Zajistěte, aby uživatel "root" se "vlastník" soubor skriptu a že pouze "vlastník" má "oprávnění ke čtení", "zápisu" a "spustit". |
| InsufficientPermissionforPost skriptu | Ke spouštění skriptů, kořenový uživatel by měl být vlastníka souboru a soubor musí mít oprávnění "700" (to znamená, musí mít pouze "vlastník" "číst", "zápisu" a "spustit" oprávnění). | Zajistěte, aby uživatel "root" se "vlastník" soubor skriptu a že pouze "vlastník" má "oprávnění ke čtení", "zápisu" a "spustit". |
| Před ScriptTimeout | Doba pro provedení konzistentní s aplikací zálohování před skriptu vypršel. | Zkontrolujte skript a zvýšit časový limit v **VMSnapshotScriptPluginConfig.json** soubor, který se nachází v **/etc/azure**. |
| POST-ScriptTimeout | Vypršel časový limit spuštění zálohování konzistentní s aplikací po skriptu. | Zkontrolujte skript a zvýšit časový limit v **VMSnapshotScriptPluginConfig.json** soubor, který se nachází v **/etc/azure**. |

## <a name="next-steps"></a>Další kroky
[Konfigurace zálohování virtuálních počítačů do trezoru služeb zotavení](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
