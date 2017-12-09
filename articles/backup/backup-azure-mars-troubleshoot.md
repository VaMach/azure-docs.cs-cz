---
title: "Řešení potíží s Azure Backup Agent (MARS Agent) | Microsoft Docs"
description: "Řešení potíží s instalací & registrace agenta Azure Backup"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: d05b951277515f3100aefcfb06a17b661267cb37
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Řešení potíží s Azure Backup Agent nebo registraci konfigurace
## <a name="recommended-steps"></a>Doporučené kroky
Najdete doporučené akce níže uvedené řešení odpovídající chyb, které se můžou vyskytnout během konfigurace nebo registrace agenta Azure Backup.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Chyba: Neplatné přihlašovací údaje úložiště. Soubor je buď poškozený nebo nemá mít poslední přihlašovací údaje přísluší službě obnovení.
| Podrobnosti o chybě | Možné příčiny | Doporučená akce |
| ---     | ---     | ---    |
| **Chyba** </br> *Neplatné přihlašovací údaje úložiště. Soubor je buď poškozený nebo nemá mít poslední přihlašovací údaje přísluší službě obnovení. (ID: 34513)* | <ol><li> Přihlašovací údaje úložiště jsou neplatné (tj. byly stáhnou víc než 48 hodin, než čas registrace).<li>   Agent Azure Backup není možné stáhnout dočasný soubor do dočasné složky systému Windows. <li>Přihlašovací údaje úložiště jsou v umístění v síti. <li>Protokol TLS 1.0 je zakázána.<li> Připojení je blokováno nakonfigurovaného proxy serveru <br> |  <ol><li>Stáhnout nové přihlašovací údaje trezoru<li>Přejděte do možnosti Internetu > zabezpečení > Internet > klikněte na tlačítko Vlastní úroveň > přejděte zobrazit oddílu stažení souboru a vyberte možnost povolit.<li>Možná budete také muset přidejte tento web do [Důvěryhodné servery](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Změňte nastavení pro používání proxy serveru a pojmenujte podrobnosti o proxy serveru <li> Shoda s počítači datum a čas (UTC)<li>Přejděte do C:/Windows/Temp a zkontrolujte, zda jsou soubory více než 60 000 nebo 65 000 (s příponou TMP) a odstraňte tyto soubory.<li>Toto můžete otestovat spuštěním SDP balíčku na serveru. Pokud se zobrazí chyba, kterou stáhne stanovení souboru nejsou povoleny, můžete si být jisti přiměřeně o velký počet souborů v adresáři C:/Windows/Temp.<li>Ujistěte se, že máte nainstalováno rozhraní .NET framework 4.6.2 <li>Pokud jste zakázali TLS 1.0 z důvodu soulad s normami PCI, podívejte se na to [řešení potíží s odkaz](https://support.microsoft.com/help/4022913). <li>Pokud máte antivirový nainstalovaného na serveru, vylučte z kontroly antivirový následující soubory. <ol><li>CBengine.exe<li>CSC.exe (týkající se rozhraní .NET Framework. Není CSC.exe na verzi rozhraní .NET nainstalované na serveru. Vyloučit všechny soubory CSC.exe vázaný na všechny verze rozhraní .NET framework na příslušném serveru.) <li>Cesta k pomocnému složku nebo mezipaměti v umístění. <br>*Výchozí umístění pro pomocné složka nebo cesta k umístění mezipaměti je C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Chyba: Agenta služeb zotavení Microsoft Azure se nepodařilo připojit k Microsoft Azure Backup

| Podrobnosti o chybě | Možné příčiny | Doporučená akce |
| ---     | ---     | ---    |
| **Chyba** </br><ol><li>*Agent služeb zotavení Microsoft Azure se nepodařilo připojit k Microsoft Azure Backup. (ID: 100050) Zkontrolujte síťové nastavení a ujistěte se, že budete moci připojit k Internetu*<li>*Vyžadováno ověření proxy serveru (407)* |Proxy blokování připojení * |  <ol><li>Přejděte do možnosti Internetu > zabezpečení > Internet > klikněte na tlačítko Vlastní úroveň > přejděte zobrazit oddílu stažení souboru a vyberte možnost povolit.<li>Možná budete také muset přidejte tento web do [Důvěryhodné servery](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Změňte nastavení pro používání proxy serveru a pojmenujte podrobnosti o proxy serveru <li>Pokud máte antivirový nainstalovaného na serveru, vylučte z kontroly antivirový následující soubory. <ol><li>CBengine.exe<li>(místo dpmra.exe)<li>CSC.exe (týkající se rozhraní .NET Framework. Není CSC.exe na verzi rozhraní .NET nainstalované na serveru. Vyloučit všechny soubory CSC.exe vázaný na všechny verze rozhraní .NET framework na příslušném serveru.) <li>Cesta k pomocnému složku nebo mezipaměti v umístění <br>*Výchozí umístění pro pomocné složka nebo cesta k umístění mezipaměti je C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Chyba: Nepodařilo se nastavit šifrovací klíč pro zabezpečené zálohy

| Podrobnosti o chybě | Možné příčiny | Doporučená akce |
| ---     | ---     | ---    |      
| **Chyba** </br>*Nepodařilo se nastavit šifrovací klíč pro zabezpečené zálohy. Aktuální operace selhala kvůli vnitřní chyba: Neplatné vstupní chybě služby'. Opakujte operaci po určité době. Pokud potíže potrvají, kontaktujte prosím podporu společnosti Microsoft* |Server je již zaregistrována k jiné trezoru| Zrušte registraci serveru z trezoru a znovu zaregistrujte.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Chyba: Aktivace nebyla úspěšně dokončena. Aktuální operace selhala kvůli vnitřní chybě služby [0x1FC07]

| Podrobnosti o chybě | Možné příčiny | Doporučená akce |
| ---     | ---     | ---    |          
| **Chyba** </br><ol><li>*Aktivace nebyla úspěšně dokončena. Aktuální operace selhala kvůli vnitřní chybě služby [0x1FC07]. Opakujte operaci po určité době. Pokud potíže potrvají, kontaktujte prosím podporu společnosti Microsoft* <li>*Došlo k chybě 34506. Šifrovací přístupové heslo uložené v tomto počítači není správně nakonfigurováno.* | <li> Pracovní složka je umístěna ve svazku, který nemá dostatek místa <li> Pracovní složka je nesprávně přesunout do jiného umístění <li> Chybí soubor OnlineBackup.KEK | <li>Přesuňte pomocné složku nebo umístění mezipaměti na svazek s ekvivalent hodnoty 5 až 10 % volného místa a celkové velikosti zálohovaná data. Odkazovat na postup uvedený v [v tomto článku](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) správně přesunout umístění mezipaměti.<li> Ujistěte se, že je soubor OnlineBackup.KEK k dispozici <br>*Výchozí umístění pro pomocné složka nebo cesta k umístění mezipaměti je C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Kontaktování podpory
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.

## <a name="next-steps"></a>Další kroky
* Získat podrobnosti o [zálohování Windows serveru pomocí agenta Azure Backup](tutorial-backup-windows-server-to-azure.md).
* Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).
